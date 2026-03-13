# IMPORTANT NOTICE:
This file represents a partially redacted version of the production contract.
Certain internal components have been removed to protect proprietary implementation details.
The deployed contract bytecode on BNB Smart Chain remains the canonical reference implementation.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

/**
 * @title NXY — Naira Index (Institutional)
 * @author Simon Kapenda
 * @notice Enterprise-grade, UUPS-upgradeable, benchmark-linked ERC20 instrument for the Naira Index.
 *
 * Design:
 * - Supported chains:
 *   * BNB Chain Mainnet (56)
 *   * BNB Chain Testnet (97)
 * - Reference asset:
 *   * cNGN on BNB Chain Mainnet
 *   * cNGN on BNB Chain Testnet
 * - Benchmark reference: DXY (off-chain, oracle-signed)
 * - Max supply: 100,000,000,000 NXY with 18 decimals
 * - Full premint: entire max supply minted at initialization to treasury
 * - Oracle quorum: ceil(2N/3) over live active authorized oracle signers
 * - Oracle policy floor: at least 3 live active signers and at least 2 distinct live signer classes
 * - Governance-only constitutional state changes
 * - Incident-linked extraordinary publication flow
 * - Recovery flow: compliance-bypassing but NOT global-pause-bypassing and NOT transfer-domain-pause-bypassing
 * - Recovery transfer destination: treasury or governance-approved typed destinations only
 * - Invalidation can be reinstated by incident authority if required
 * - Same version / same valuation-time corrected republishing is not allowed
 * - ERC-7201 namespaced storage
 *
 * Reference-asset assurance note:
 * - This contract pins the reference-asset address and extcodehash for the supported environment.
 * - If cNGN is deployed behind a proxy, extcodehash pins the proxy bytecode, not necessarily the underlying implementation.
 * - That proxy-risk is accepted by design and must be handled operationally and documented in deployment / governance runbooks.
 *
 * Recovery authority note:
 * - Recovery functions are intentionally privileged emergency tools.
 * - They bypass ordinary transfer compliance restrictions, but only under:
 *   * an active incident,
 *   * a blocked source account,
 *   * a reason code,
 *   * a typed destination policy for transfers,
 *   * and when global / transfer-domain pause rules permit execution.
 * - This is a deliberate institutional override surface and must be governed operationally.
 */

import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import {UUPSUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import {ERC20Upgradeable} from "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
import {ERC20PermitUpgradeable} from "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";
import {PausableUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";
import {AccessControlUpgradeable} from "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";
import {ReentrancyGuardUpgradeable} from "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

import {INXYComplianceModule} from "./INXYComplianceModule.sol";
import {INXYOraclePolicyModule} from "./INXYOraclePolicyModule.sol";
import {INXYPublicationManager} from "./INXYPublicationManager.sol";

interface ITimelockMinDelay {
    function getMinDelay() external view returns (uint256);
}

contract NairaIndexNXY is
    Initializable,
    ERC20Upgradeable,
    ERC20PermitUpgradeable,
    PausableUpgradeable,
    AccessControlUpgradeable,
    ReentrancyGuardUpgradeable,
    UUPSUpgradeable
{
    bytes32 public constant GOVERNANCE_ROLE = keccak256("GOVERNANCE_ROLE");
    bytes32 public constant UPGRADER_ROLE = keccak256("UPGRADER_ROLE");
    bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
    bytes32 public constant RECOVERY_ROLE = keccak256("RECOVERY_ROLE");

    uint256 public constant BNB_MAINNET_CHAIN_ID = 56;
    uint256 public constant BNB_TESTNET_CHAIN_ID = 97;

    address public constant CNGN_BNB_MAINNET = 0xa8AEA66B361a8d53e8865c62D142167Af28Af058;
    address public constant CNGN_BNB_TESTNET = 0x8a078b182bA9649c03982c2a80CDcc81cdc99dA8;

    uint8 public constant CNGN_DECIMALS = 6;
    bytes32 public constant CNGN_SYMBOL_HASH = keccak256(bytes("cNGN"));

    uint256 public constant MAX_SUPPLY = 100_000_000_000 * 10**18;
    uint256 public constant MIN_TIMELOCK_DELAY = 1 days;

    uint8 public constant INCIDENT_STATE_ACTIVE = 1;

    uint8 public constant RECOVERY_DESTINATION_NONE = 0;
    uint8 public constant RECOVERY_DESTINATION_TREASURY = 1;
    uint8 public constant RECOVERY_DESTINATION_ESCROW = 2;
    uint8 public constant RECOVERY_DESTINATION_REGULATOR = 3;
    uint8 public constant RECOVERY_DESTINATION_REMEDIATION = 4;

    error ZeroAddress();
    error UnsupportedChain(uint256 chainId);

    error InvalidTimelock();
    error TimelockDelayTooShort(uint256 actualDelay, uint256 minimumRequired);

    error InvalidOraclePolicyModule();
    error InvalidComplianceModule();
    error InvalidPublicationManager();

    error ArrayLengthMismatch();
    error BatchTooLarge();
    error AddressesNotStrictlyAscending();

    error TransfersPaused();

    error RecoveryReasonCodeRequired();
    error IncidentIdRequired();
    error IncidentNotActive(bytes32 incidentId);
    error UnapprovedRecoveryDestination(address destination);
    error InvalidRecoveryDestinationType(uint8 destinationType);
    error RecoveryDestinationTypeNotAllowed(uint8 reasonCode, uint8 destinationType);
    error RecoverySourceNotRestricted(address account);

    error InvalidReferenceAsset();
    error InvalidReferenceAssetDecimals();
    error InvalidReferenceAssetSymbol();
    error InvalidReferenceAssetName();
    error InvalidReferenceAssetCodehash();

    event TreasurySet(address indexed treasury);
    event OraclePolicyModuleSet(address indexed oraclePolicyModule);
    event ComplianceModuleSet(address indexed complianceModule);
    event PublicationManagerSet(address indexed publicationManager);
    event TimelockValidated(address indexed timelock, uint256 minDelay);

    event RecoveryDestinationConfigured(address indexed destination, uint8 indexed destinationType);
    event RecoveryReasonDestinationMaskSet(uint8 indexed reasonCode, uint256 allowedTypeMask);

    event TransfersPausedSet(bool paused);

    event RecoveryTransfer(
        address indexed from,
        address indexed to,
        uint256 amount,
        bytes32 indexed referenceId,
        uint8 reasonCode,
        bytes32 incidentId
    );

    event RecoveryBurn(
        address indexed from,
        uint256 amount,
        bytes32 indexed referenceId,
        uint8 reasonCode,
        bytes32 incidentId
    );

    struct Layout {
        mapping(address => uint8) recoveryDestinationType;
        mapping(uint8 => uint256) recoveryReasonAllowedDestinationTypeMask;

        address treasury;
        address oraclePolicyModule;
        address complianceModule;
        address publicationManager;

        bool transfersPaused;
        bool privilegedTransferBypass;
    }

    /// @custom:storage-location erc7201:nxy.storage.main.slim
    bytes32 private constant LAYOUT_SLOT =
        0x35d9bc1434f68bd3720fdacc4d1851914f8f8f8cb67f67c93b3b9aafe3942000;

    function _layout() private pure returns (Layout storage s) {
        bytes32 slot = LAYOUT_SLOT;
        assembly {
            s.slot := slot
        }
    }

    /// @custom:oz-upgrades-unsafe-allow constructor
    constructor() {
        _disableInitializers();
    }

    function initialize(
        address timelock_,
        address treasury_,
        address oraclePolicyModule_,
        address complianceModule_,
        address publicationManager_
    ) external initializer {
        if (!_isSupportedChain(block.chainid)) revert UnsupportedChain(block.chainid);
        if (
            timelock_ == address(0) ||
            treasury_ == address(0) ||
            oraclePolicyModule_ == address(0) ||
            complianceModule_ == address(0) ||
            publicationManager_ == address(0)
        ) revert ZeroAddress();

        uint256 minDelay = _validateTimelock(timelock_);
        _assertOraclePolicyModuleCompatible(oraclePolicyModule_);
        _assertComplianceModuleCompatible(complianceModule_);
        _assertPublicationManagerCompatible(publicationManager_);
        _validateReferenceAssetBasic(_referenceAsset());

        __ERC20_init("Naira Index", "NXY");
        __ERC20Permit_init("Naira Index");
        __Pausable_init();
        __AccessControl_init();
        __ReentrancyGuard_init();
        __UUPSUpgradeable_init();

        _grantRole(DEFAULT_ADMIN_ROLE, timelock_);
        _grantRole(GOVERNANCE_ROLE, timelock_);
        _grantRole(UPGRADER_ROLE, timelock_);
        _grantRole(PAUSER_ROLE, timelock_);
        _grantRole(RECOVERY_ROLE, timelock_);

        _setRoleAdmin(GOVERNANCE_ROLE, DEFAULT_ADMIN_ROLE);
        _setRoleAdmin(UPGRADER_ROLE, DEFAULT_ADMIN_ROLE);
        _setRoleAdmin(PAUSER_ROLE, DEFAULT_ADMIN_ROLE);
        _setRoleAdmin(RECOVERY_ROLE, DEFAULT_ADMIN_ROLE);

        Layout storage s = _layout();
        s.treasury = treasury_;
        s.oraclePolicyModule = oraclePolicyModule_;
        s.complianceModule = complianceModule_;
        s.publicationManager = publicationManager_;

        _mint(treasury_, MAX_SUPPLY);

        emit TimelockValidated(timelock_, minDelay);
        emit TreasurySet(treasury_);
        emit OraclePolicyModuleSet(oraclePolicyModule_);
        emit ComplianceModuleSet(complianceModule_);
        emit PublicationManagerSet(publicationManager_);
    }

    function _authorizeUpgrade(address newImplementation)
        internal
        view
        override
        onlyRole(UPGRADER_ROLE)
    {
        if (newImplementation == address(0)) revert ZeroAddress();
    }

    function setOraclePolicyModule(address oraclePolicyModule_) external onlyRole(GOVERNANCE_ROLE) {
        if (oraclePolicyModule_ == address(0)) revert ZeroAddress();
        _assertOraclePolicyModuleCompatible(oraclePolicyModule_);
        _layout().oraclePolicyModule = oraclePolicyModule_;
        emit OraclePolicyModuleSet(oraclePolicyModule_);
    }

    function setComplianceModule(address complianceModule_) external onlyRole(GOVERNANCE_ROLE) {
        if (complianceModule_ == address(0)) revert ZeroAddress();
        _assertComplianceModuleCompatible(complianceModule_);
        _layout().complianceModule = complianceModule_;
        emit ComplianceModuleSet(complianceModule_);
    }

    function setPublicationManager(address publicationManager_) external onlyRole(GOVERNANCE_ROLE) {
        if (publicationManager_ == address(0)) revert ZeroAddress();
        _assertPublicationManagerCompatible(publicationManager_);
        _layout().publicationManager = publicationManager_;
        emit PublicationManagerSet(publicationManager_);
    }

    function setRecoveryDestinationType(address destination, uint8 destinationType)
        external
        onlyRole(GOVERNANCE_ROLE)
    {
        if (destination == address(0)) revert ZeroAddress();
        _validateRecoveryDestinationType(destinationType);
        _layout().recoveryDestinationType[destination] = destinationType;
        emit RecoveryDestinationConfigured(destination, destinationType);
    }

    function batchSetRecoveryDestinationType(
        address[] calldata destinations,
        uint8[] calldata destinationTypes
    ) external onlyRole(GOVERNANCE_ROLE) {
        if (destinations.length != destinationTypes.length) revert ArrayLengthMismatch();
        if (destinations.length > 200) revert BatchTooLarge();
        _assertStrictlyAscendingAddresses(destinations);

        Layout storage s = _layout();
        for (uint256 i = 0; i < destinations.length; ++i) {
            _validateRecoveryDestinationType(destinationTypes[i]);
            s.recoveryDestinationType[destinations[i]] = destinationTypes[i];
            emit RecoveryDestinationConfigured(destinations[i], destinationTypes[i]);
        }
    }

    function setRecoveryReasonDestinationMask(uint8 reasonCode, uint256 allowedTypeMask)
        external
        onlyRole(GOVERNANCE_ROLE)
    {
        if (reasonCode == 0) revert RecoveryReasonCodeRequired();
        _layout().recoveryReasonAllowedDestinationTypeMask[reasonCode] = allowedTypeMask;
        emit RecoveryReasonDestinationMaskSet(reasonCode, allowedTypeMask);
    }

    function setTransfersPaused(bool paused_) external onlyRole(PAUSER_ROLE) {
        _layout().transfersPaused = paused_;
        emit TransfersPausedSet(paused_);
    }

    function pause() external onlyRole(PAUSER_ROLE) {
        _pause();
    }

    function unpause() external onlyRole(PAUSER_ROLE) {
        _unpause();
    }

    function recoveryTransfer(
        address from,
        address to,
        uint256 amount,
        bytes32 referenceId,
        uint8 reasonCode,
        bytes32 incidentId
    ) external onlyRole(RECOVERY_ROLE) nonReentrant {
        Layout storage s = _layout();

        if (paused()) revert EnforcedPause();
        if (s.transfersPaused) revert TransfersPaused();
        if (from == address(0) || to == address(0)) revert ZeroAddress();
        if (reasonCode == 0) revert RecoveryReasonCodeRequired();
        if (incidentId == bytes32(0)) revert IncidentIdRequired();
        if (uint8(INXYPublicationManager(s.publicationManager).incidentStateOf(incidentId)) != INCIDENT_STATE_ACTIVE) {
            revert IncidentNotActive(incidentId);
        }

        if (!INXYComplianceModule(s.complianceModule).isBlocked(from)) {
            revert RecoverySourceNotRestricted(from);
        }

        uint8 destinationType = to == s.treasury ? RECOVERY_DESTINATION_TREASURY : s.recoveryDestinationType[to];
        if (destinationType == RECOVERY_DESTINATION_NONE) revert UnapprovedRecoveryDestination(to);

        uint256 allowedMask = s.recoveryReasonAllowedDestinationTypeMask[reasonCode];
        if ((allowedMask & (uint256(1) << destinationType)) == 0) {
            revert RecoveryDestinationTypeNotAllowed(reasonCode, destinationType);
        }

        s.privilegedTransferBypass = true;
        _transfer(from, to, amount);
        s.privilegedTransferBypass = false;

        emit RecoveryTransfer(from, to, amount, referenceId, reasonCode, incidentId);
    }

    function recoveryBurn(
        address from,
        uint256 amount,
        bytes32 referenceId,
        uint8 reasonCode,
        bytes32 incidentId
    ) external onlyRole(RECOVERY_ROLE) nonReentrant {
        Layout storage s = _layout();

        if (paused()) revert EnforcedPause();
        if (s.transfersPaused) revert TransfersPaused();
        if (from == address(0)) revert ZeroAddress();
        if (reasonCode == 0) revert RecoveryReasonCodeRequired();
        if (incidentId == bytes32(0)) revert IncidentIdRequired();
        if (uint8(INXYPublicationManager(s.publicationManager).incidentStateOf(incidentId)) != INCIDENT_STATE_ACTIVE) {
            revert IncidentNotActive(incidentId);
        }

        if (!INXYComplianceModule(s.complianceModule).isBlocked(from)) {
            revert RecoverySourceNotRestricted(from);
        }

        s.privilegedTransferBypass = true;
        _burn(from, amount);
        s.privilegedTransferBypass = false;

        emit RecoveryBurn(from, amount, referenceId, reasonCode, incidentId);
    }

    function treasury() external view returns (address) {
        return _layout().treasury;
    }

    function oraclePolicyModule() external view returns (address) {
        return _layout().oraclePolicyModule;
    }

    function complianceModule() external view returns (address) {
        return _layout().complianceModule;
    }

    function publicationManager() external view returns (address) {
        return _layout().publicationManager;
    }

    function referenceAsset() external view returns (address) {
        return _referenceAsset();
    }

    function maxSupply() external pure returns (uint256) {
        return MAX_SUPPLY;
    }

    function burnedSupply() external view returns (uint256) {
        return MAX_SUPPLY - totalSupply();
    }

    function latestPublication()
        external
        view
        returns (
            uint256 version,
            uint64 valuationTime,
            uint256 nxyLevelE18,
            uint256 dxyLevelE18,
            uint256 cngnPerUsdE18,
            bytes32 publicationHash,
            bytes32 sourceSetId,
            bytes32 incidentId,
            bytes32 signerSetHash,
            bool valid,
            bool restoredFromLastGood
        )
    {
        return INXYPublicationManager(_layout().publicationManager).latestPublication();
    }

    function lastGoodPublication()
        external
        view
        returns (
            uint256 version,
            uint64 valuationTime,
            uint256 nxyLevelE18,
            uint256 dxyLevelE18,
            uint256 cngnPerUsdE18,
            bytes32 publicationHash,
            bytes32 sourceSetId,
            bytes32 incidentId,
            bytes32 signerSetHash,
            bool valid,
            bool restoredFromLastGood
        )
    {
        return INXYPublicationManager(_layout().publicationManager).lastGoodPublication();
    }

    function _update(address from, address to, uint256 value) internal override {
        Layout storage s = _layout();

        if (from == address(0)) {
            super._update(from, to, value);
            return;
        }

        if (paused()) revert EnforcedPause();
        if (s.transfersPaused) revert TransfersPaused();

        if (s.privilegedTransferBypass) {
            if (to == address(0) || to == s.treasury) {
                super._update(from, to, value);
                return;
            }

            INXYComplianceModule(s.complianceModule).validateTransfer(from, to, value, true);
            super._update(from, to, value);
            return;
        }

        INXYComplianceModule(s.complianceModule).validateTransfer(from, to, value, false);
        super._update(from, to, value);
    }

    function _validateTimelock(address timelock_) internal view returns (uint256 minDelay) {
        if (timelock_ == address(0) || timelock_.code.length == 0) revert InvalidTimelock();

        try ITimelockMinDelay(timelock_).getMinDelay() returns (uint256 delay_) {
            minDelay = delay_;
        } catch {
            revert InvalidTimelock();
        }

        if (minDelay < MIN_TIMELOCK_DELAY) {
            revert TimelockDelayTooShort(minDelay, MIN_TIMELOCK_DELAY);
        }
    }

    function _assertStrictlyAscendingAddresses(address[] calldata accounts) internal pure {
        if (accounts.length == 0) return;
        address prev = address(0);
        for (uint256 i = 0; i < accounts.length; ++i) {
            address current = accounts[i];
            if (current == address(0)) revert ZeroAddress();
            if (i != 0 && current <= prev) revert AddressesNotStrictlyAscending();
            prev = current;
        }
    }

    function _assertOraclePolicyModuleCompatible(address oraclePolicyModule_) internal view {
        if (oraclePolicyModule_ == address(0) || oraclePolicyModule_.code.length == 0) {
            revert InvalidOraclePolicyModule();
        }

        try INXYOraclePolicyModule(oraclePolicyModule_).oracleSetReady() returns (bool) {} catch {
            revert InvalidOraclePolicyModule();
        }
    }

    function _assertComplianceModuleCompatible(address complianceModule_) internal view {
        if (complianceModule_ == address(0) || complianceModule_.code.length == 0) {
            revert InvalidComplianceModule();
        }

        try INXYComplianceModule(complianceModule_).controller() returns (address controller_) {
            if (controller_ != address(this)) revert InvalidComplianceModule();
        } catch {
            revert InvalidComplianceModule();
        }
    }

    function _assertPublicationManagerCompatible(address publicationManager_) internal view {
        if (publicationManager_ == address(0) || publicationManager_.code.length == 0) {
            revert InvalidPublicationManager();
        }

        try INXYPublicationManager(publicationManager_).controller() returns (address controller_) {
            if (controller_ != address(this)) revert InvalidPublicationManager();
        } catch {
            revert InvalidPublicationManager();
        }

        try INXYPublicationManager(publicationManager_).latestPublication()
            returns (
                uint256,
                uint64,
                uint256,
                uint256,
                uint256,
                bytes32,
                bytes32,
                bytes32,
                bytes32,
                bool,
                bool
            )
        {} catch {
            revert InvalidPublicationManager();
        }
    }

    function _validateRecoveryDestinationType(uint8 destinationType) internal pure {
        if (
            destinationType != RECOVERY_DESTINATION_NONE &&
            destinationType != RECOVERY_DESTINATION_TREASURY &&
            destinationType != RECOVERY_DESTINATION_ESCROW &&
            destinationType != RECOVERY_DESTINATION_REGULATOR &&
            destinationType != RECOVERY_DESTINATION_REMEDIATION
        ) revert InvalidRecoveryDestinationType(destinationType);
    }

    function _validateReferenceAssetBasic(address token) internal view {
        if (token.code.length == 0) revert InvalidReferenceAsset();

        (bool okDecimals, bytes memory retDecimals) =
            token.staticcall(abi.encodeWithSignature("decimals()"));
        if (!okDecimals || retDecimals.length < 32) revert InvalidReferenceAsset();
        uint8 decimals_ = abi.decode(retDecimals, (uint8));
        if (decimals_ != CNGN_DECIMALS) revert InvalidReferenceAssetDecimals();

        (bool okSymbol, bytes memory retSymbol) =
            token.staticcall(abi.encodeWithSignature("symbol()"));
        if (!okSymbol || retSymbol.length == 0) revert InvalidReferenceAssetSymbol();
        string memory symbol_ = abi.decode(retSymbol, (string));
        if (keccak256(bytes(symbol_)) != CNGN_SYMBOL_HASH) revert InvalidReferenceAssetSymbol();

        (bool okName, bytes memory retName) =
            token.staticcall(abi.encodeWithSignature("name()"));
        if (!okName || retName.length == 0) revert InvalidReferenceAssetName();
        string memory name_ = abi.decode(retName, (string));
        if (bytes(name_).length == 0) revert InvalidReferenceAssetName();
    }

    function _isSupportedChain(uint256 chainId) internal pure returns (bool) {
        return chainId == BNB_MAINNET_CHAIN_ID || chainId == BNB_TESTNET_CHAIN_ID;
    }

    function _referenceAsset() internal view returns (address) {
        if (block.chainid == BNB_MAINNET_CHAIN_ID) return CNGN_BNB_MAINNET;
        if (block.chainid == BNB_TESTNET_CHAIN_ID) return CNGN_BNB_TESTNET;
        revert UnsupportedChain(block.chainid);
    }
}
