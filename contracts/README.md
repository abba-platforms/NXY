------------------------------------------------------------

## Smart Contract Implementation Notice

The smart contract files contained within this repository may include truncated or partially redacted implementations of the Naira Index (NXY) protocol.

Certain internal components of the production contract architecture have been intentionally omitted in order to protect proprietary protocol implementation details.

The public repository is intended to document the benchmark methodology, protocol architecture, and governance framework while allowing external observers to understand the operational structure of the NXY benchmark system.

The canonical production deployment of the NXY protocol is verified on-chain on BNB Smart Chain.  
On-chain contract bytecode represents the authoritative implementation of the protocol.

Researchers and observers may independently verify deployed contract behavior through blockchain explorers and on-chain transaction history.

This repository therefore serves as documentation of the protocol architecture and methodology rather than a complete reproduction of the production smart contract source code.

------------------------------------------------------------

### NXY Protocol Contract Modules

**Core Contract**
NairaIndexNXY

**Validation Layer**
OraclePolicyModule

**Publication Layer**
PublicationManager

**Compliance Layer**
ComplianceModule

**Data Access Layer**
AnalyticalLens

**Governance Layer**
TimelockController
