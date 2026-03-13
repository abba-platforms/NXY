# CHANGELOG        

All notable changes to the Naira Index (NXY) protocol repository are documented in this file.

This project follows a structured documentation release process.  
Each release records changes to protocol documentation, architecture specifications, and governance policies.

Versioning follows a semantic-style documentation versioning approach.

------------------------------------------------------------

## v1.0.0 — 2026-03-13
Status: Initial Public Documentation Release

------------------------------------------------------------

### Added

Repository documentation framework describing the architecture, methodology, governance, and macroeconomic context of the NXY benchmark protocol.

The following documents were introduced:

README.md  
Protocol overview and repository landing documentation.

WHITEPAPER.md  
Comprehensive explanation of the NXY benchmark architecture, methodology, and protocol design.

ARCHITECTURE.md  
Technical description of the smart contract architecture and protocol interaction flows.

METHODOLOGY.md  
Formal benchmark calculation methodology referencing the Nigerian Naira relative to the US Dollar Index (DXY).

GOVERNANCE.md  
Description of the protocol governance framework and timelock-controlled administrative system.

ECONOMIC_ANALYSIS.md  
Macroeconomic context describing the Nigerian currency environment and potential benchmark adoption scenarios.

DISCLAIMER.md  
Legal and operational disclaimer defining the scope and limitations of the NXY benchmark protocol.

LICENSE.md  
Open-source licensing terms governing the repository.

------------------------------------------------------------

### Contracts Documentation

Introduced the contracts directory documenting the smart contract architecture of the NXY protocol.

contracts/README.md  
Overview of the modular smart contract architecture and contract responsibilities.

Documented protocol module structure:

Core Contract  
NairaIndexNXY

Validation Layer  
OraclePolicyModule

Publication Layer  
PublicationManager

Compliance Layer  
ComplianceModule

Data Access Layer  
AnalyticalLens

Governance Layer  
TimelockController

------------------------------------------------------------

### Core Contract Reference

Added a redacted reference implementation of the core protocol contract.

contracts/NairaIndexNXY.sol.md

The contract documents the operational structure of the NXY protocol while intentionally omitting certain supporting modules in order to protect proprietary implementation details.

A notice was added to clarify that the repository version of the contract represents an architectural reference rather than a complete deployable implementation.

The canonical operational implementation remains the deployed smart contract system on BNB Smart Chain.

------------------------------------------------------------

### Documentation Improvements

Expanded the repository documentation to provide a complete architectural overview of the NXY benchmark protocol.

Added protocol architecture diagrams describing the interaction between oracle validation, benchmark publication, compliance enforcement, and governance control.

Added mainnet deployment documentation including contract addresses and component descriptions.

Added repository structure documentation to clarify the relationship between protocol documentation and smart contract modules.

------------------------------------------------------------

### Protocol Context

Documented the economic context in which the NXY benchmark operates, including reference indicators for the Nigerian currency system such as:

Broad money supply (M2)  
Foreign exchange turnover  
Foreign exchange reserves  
Nominal GDP

These indicators provide macroeconomic context for the benchmark environment referenced by the protocol.

------------------------------------------------------------


## [1.0.0] — 2026-03-13

### Initial Institutional Documentation Release

This release establishes the first complete institutional documentation framework for the Naira Index (NXY) protocol.

The repository now includes the full documentation set required to describe the benchmark architecture, methodology, governance structure, security framework, and macroeconomic context of the protocol.

------------------------------------------------------------

### Added

WHITEPAPER.md

Introduced the full institutional whitepaper describing the NXY protocol architecture, benchmark design principles, oracle validation framework, governance structure, and benchmark lifecycle.

The whitepaper defines the benchmark relationship between the Nigerian Naira and the global dollar environment using the US Dollar Index (DXY) and cNGN as the on-chain Naira reference layer.

------------------------------------------------------------

METHODOLOGY.md

Added the formal benchmark methodology specification defining the mathematical structure of the NXY benchmark.

The methodology defines the benchmark formula:

NXY = DXY / (NGN per USD)

This document also defines:

data input requirements  
oracle validation rules  
benchmark publication procedures  
methodology versioning rules  
benchmark integrity safeguards  

------------------------------------------------------------

ECONOMIC_ANALYSIS.md

Added a macroeconomic analysis document describing the economic environment referenced by the NXY benchmark.

The analysis provides context using the following macroeconomic indicators:

Nigeria broad money supply (M2)  
Nigeria foreign exchange market turnover  
Nigeria nominal GDP  
Nigeria external balance and reserve conditions  

The document also presents simulated benchmark adoption scenarios based on monetary base and FX market depth.

------------------------------------------------------------

GOVERNANCE.md

Added the governance framework describing how protocol parameters and benchmark methodology may be updated through on-chain governance.

The governance model includes:

timelock-controlled governance operations  
oracle authorization management  
methodology version management  
transparent governance records  

------------------------------------------------------------

ARCHITECTURE.md

Added a detailed description of the NXY smart contract architecture.

The architecture document defines the functional roles of the core benchmark components including:

Core benchmark contract  
Oracle policy module  
Compliance module  
Publication manager  
Compliance administration module  
Protocol analytics lens  

The document also includes architectural flow diagrams describing the benchmark publication lifecycle.

------------------------------------------------------------

SECURITY.md

Added a security framework document describing the safeguards implemented by the protocol.

The document outlines:

oracle validation safeguards  
governance restrictions  
benchmark integrity protections  
smart contract operational controls  

------------------------------------------------------------

DISCLAIMER.md

Added the legal and analytical disclaimer describing the nature of the benchmark.

The document clarifies that the NXY benchmark is a macroeconomic reference indicator and not a financial instrument or investment product.

------------------------------------------------------------

README.md

Added the institutional repository overview describing the purpose of the NXY protocol and the structure of the repository documentation.

The README includes:

protocol overview  
contract deployment references  
tokenomics overview  
repository structure  

------------------------------------------------------------

Tokenomics Documentation

Introduced the NXY token allocation framework.

Maximum supply:

100,000,000,000 NXY

Allocation structure:

35% centralized exchange distribution  
25% founding team and development staff  
15% strategic investors  
20% protocol treasury  
5% liquidity provisioning  

The token supply is permanently capped and cannot be increased.

------------------------------------------------------------

### Infrastructure

The NXY protocol contracts have been deployed to the BNB Smart Chain mainnet.

The deployment includes:

Core benchmark contract  
Oracle validation module  
Compliance module  
Publication manager  
Compliance administration module  
Protocol analytics lens  

All contracts have been verified on the BNB Smart Chain block explorer.

------------------------------------------------------------

### Repository

https://github.com/abba-platforms/NXY

------------------------------------------------------------

Future releases of the repository may include:

expanded benchmark documentation  
additional analytical reports  
methodology revisions through governance  
security review documentation
