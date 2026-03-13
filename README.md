# Naira Index (NXY)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE) ![Built with Solidity](https://img.shields.io/badge/built%20with-Solidity-363636) ![ERC-20 Compliant](https://img.shields.io/badge/ERC--20-Compliant-brightgreen) [![Security Policy](https://img.shields.io/badge/Security-Policy-blue.svg)](SECURITY.md)
![Creator](https://img.shields.io/badge/Creator-Simon%20Kapenda-lightgrey.svg)

# Blockchain-Native Benchmark Referencing the Nigerian Naira

Benchmarked Against the US Dollar Index (DXY)

By [Simon Kapenda](https://linkedin.com/in/simonkapenda).     
**Date:** March 12, 2026      
**Protocol Network:** BNB Smart Chain  
**Repository Status:** Active Infrastructure

------------------------------------------------------------

## Protocol Overview

The Naira Index (NXY) is a blockchain-native benchmark designed to reference the Nigerian Naira within the global currency environment represented by the US Dollar Index (DXY).

The benchmark publishes normalized macroeconomic observations derived from publicly observable foreign exchange inputs and validated through deterministic smart contract infrastructure.

The objective of the protocol is to create a transparent, reproducible, and auditable reference indicator that can be integrated into financial analytics systems, research platforms, decentralized infrastructure, and programmable financial systems.

NXY does not function as a financial product or investment instrument.

The protocol operates strictly as a benchmark publication system.

------------------------------------------------------------

## Mainnet Deployment

The NXY protocol is deployed on **BNB Smart Chain** using a modular smart contract architecture.  
Each contract performs a specific role within the benchmark validation, publication, and governance framework.

------------------------------------------------------------

### Core Proxy (Main)

Address

0xfDA959889a4b8e189D17cA161C74A348cc1Ca57B

Description

The Core Proxy contract represents the primary access point of the NXY protocol.

The proxy implements an upgradeable architecture that routes external interactions to the active implementation contract while preserving the canonical storage state.

This design allows protocol logic upgrades without changing the core contract address.

------------------------------------------------------------

### Core Implementation

Address

0x5eF22FDd5495855e91AD83f708A97BEE46c1E2aE

Description

The Core Implementation contract contains the operational logic of the NXY protocol.

Responsibilities include:

- protocol coordination  
- oracle validation orchestration  
- publication lifecycle management
- interaction with governance modules  

The implementation contract operates behind the proxy and may be upgraded through governance.

------------------------------------------------------------

### Oracle Policy Module

Address

0x0BfA4CF9E8e75c933B698000EEd14998B04C3CeE

Description

The Oracle Policy Module validates benchmark observations submitted by oracle signers.

Validation checks include:

- signature verification  
- signer class diversity validation  
- timestamp validation  
- methodology verification  
- reference asset verification  

Only validated observations may proceed to publication.

------------------------------------------------------------

### Compliance Module

Address

0x3918CCEc1D8D5148757259F7FdC1E0935607866b

Description

The Compliance Module enforces protocol-level integrity constraints.

Responsibilities include:

- protocol compliance checks  
- operational safeguards  
- validation policy enforcement  

The module operates independently of the benchmark publication logic.

------------------------------------------------------------

### Publication Manager

Address

0x01Efaf16915080057e14646D8Cf6Ac15Fb172cD6

Description

The Publication Manager maintains the canonical record of benchmark observations.

Responsibilities include:

- storing benchmark publications  
- maintaining historical observation sequences  
- tracking the current benchmark state  

Each accepted publication becomes part of the immutable on-chain benchmark history.

------------------------------------------------------------

### Compliance Administration

Address

0xc05A67D07592a3E644b67E4223b2fce7817209EE

Description

The Compliance Administration contract manages configuration parameters associated with the compliance module.

Administrative actions include:

- compliance parameter updates  
- policy adjustments  
- protocol-level configuration changes

Administrative changes require governance authorization.

------------------------------------------------------------

### Analytical Lens

Address

0x141b706b0b75Fdf6F2aC7f83c6EB89F1BF0239A3

Description

The Analytical Lens contract provides read-optimized access to protocol data.

External systems can query this contract to retrieve:

- benchmark observations  
- protocol state  
- publication history  
- validation metadata

This contract allows efficient data access without modifying protocol state.

------------------------------------------------------------

### Governance Timelock

Address

0x0e5493b11edbC2d251423c8ae22A40Ce9afDd3be

Description

The Governance Timelock contract manages protocol governance.

All administrative actions must pass through a mandatory delay before execution.

Governance responsibilities include:

- oracle signer management  
- methodology updates  
- protocol parameter updates  
- contract upgrades  

The timelock ensures transparency and reviewability of governance actions.

------------------------------------------------------------

## Contract Interaction Architecture

The NXY protocol separates validation, publication, and governance responsibilities across multiple contracts.

```
                   +----------------------+
                   |    Oracle Signers    |
                   |  Benchmark Inputs    |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   | Oracle Policy Module |
                   | Observation Validator|
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   | Publication Manager  |
                   | Canonical Index      |
                   | Observation History  |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   |     Core Proxy       |
                   |   Protocol Router    |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   |  Core Implementation |
                   |  Benchmark Logic     |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   |  Compliance Module   |
                   | Integrity Enforcement|
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   | Governance Timelock  |
                   | Parameter Control    |
                   +----------------------+
```

------------------------------------------------------------

## Oracle Publication Flow

Benchmark observations are submitted and validated through the following process.

```
1. Oracle signers collect benchmark inputs
2. Observation submitted to Oracle Policy Module
3. Signature verification and validation checks
4. Validated observation forwarded to Publication Manager
5. Benchmark state updated on-chain
6. Analytical Lens exposes observation for external queries
```

------------------------------------------------------------

## Governance Control Flow

Protocol governance follows a timelock-controlled execution process.

```
1. Governance proposal created
2. Proposal scheduled in timelock contract
3. Mandatory delay period
4. Governance execution transaction
5. Protocol parameter update
```

This mechanism ensures that all administrative actions remain transparent and observable before execution.

------------------------------------------------------------

## Benchmark Formula

The NXY benchmark references the Nigerian Naira relative to the global dollar benchmark environment.

Calculation relationship:

```
NXY = DXY / (NGN per USD)
```

Where:

```
DXY = US Dollar Index
NGN per USD = Nigerian Naira exchange rate per US Dollar
```

Example simulation:

```
DXY = 98.95
USD/NGN = 1401.40

NXY = 98.95 / 1401.40
NXY ≈ 0.0706
```

Interpretation:

```
If NGN strengthens relative to USD → NXY increases
If NGN weakens relative to USD → NXY decreases
```

------------------------------------------------------------

## Protocol Architecture

The NXY protocol uses a modular smart contract architecture designed to separate benchmark validation, publication, compliance, and governance responsibilities.

```
                    +----------------------+
                    |   Oracle Signers    |
                    |  Benchmark Inputs   |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Oracle Policy Module |
                    | Validation Engine    |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Publication Manager  |
                    | Index State Storage  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    |   NXY Core Contract  |
                    | Protocol Controller  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Governance Timelock  |
                    | Parameter Control    |
                    +----------------------+
```

A full explanation of the contract architecture and component interactions is provided in:

./ARCHITECTURE.md

------------------------------------------------------------

## Smart Contract Components

The NXY protocol consists of several specialized smart contract modules.

**Core Contract**

Coordinates the interaction between protocol modules and maintains the canonical benchmark state.

**Oracle Policy Module**

Validates benchmark observations through signature verification, signer diversity requirements, and methodology enforcement.

**Publication Manager**

Maintains the historical record of benchmark observations and manages the canonical index state.

**Compliance Module**

Applies protocol-level restrictions and validation rules.

**Governance Timelock**

Controls administrative operations and enforces governance delays.

**Analytical Lens**

Provides read-optimized access to benchmark data for external systems and analytics platforms.

------------------------------------------------------------

## On-Chain Reference Layer

The NXY protocol references the Nigerian Naira through the cNGN stablecoin.

**Reference Asset**

cNGN Stablecoin  
https://cngn.co

The reference asset allows the protocol to validate that submitted benchmark observations correspond to the correct on-chain representation of the Nigerian Naira.

------------------------------------------------------------

## Repository Structure

```
/
├── README.md
├── ARCHITECTURE.md
├── WHITEPAPER.md
├── METHODOLOGY.md
├── GOVERNANCE.md
├── ECONOMIC_ANALYSIS.md
├── DISCLAIMER.md
├── LICENSE.md
├── SECURITY.md
└── contracts/
```

./README.md

Provides a high-level overview of the NXY protocol and repository structure.

./ARCHITECTURE.md

Describes the smart contract architecture, component responsibilities, and protocol interaction flows.

./WHITEPAPER.md

Defines the benchmark design, protocol architecture, and methodology framework.

./METHODOLOGY.md

Defines the deterministic benchmark calculation rules and validation parameters.

./GOVERNANCE.md

Specifies the governance framework controlling protocol administration.

./ECONOMIC_ANALYSIS.md

Provides macroeconomic context and benchmark market analysis.

./DISCLAIMER.md

Defines legal disclaimers associated with the benchmark.

./LICENSE.md

Specifies the open-source license governing the repository.

contracts/

Contains the smart contract implementation of the NXY protocol.

------------------------------------------------------------

## Deployment

**Network:** BNB Smart Chain    

The NXY protocol is deployed on the BNB Smart Chain using modular smart contracts that manage benchmark validation, publication, and governance.

Core protocol components include:

```
- Core Contract
- Oracle Policy Module
- Compliance Module
- Publication Manager
- Governance Timelock
- Analytical Lens
```

Deployment details and contract addresses are documented within the repository.

------------------------------------------------------------

## Economic Context

The NXY benchmark operates within the macro-financial environment of the Nigerian currency system.

Approximate macroeconomic indicators:

```
- Broad Money Supply (M2)     ≈ $88 Billion
- Annual FX Turnover          ≈ $103 Billion
- Foreign Exchange Reserves   ≈ $34.8 Billion
- Nominal GDP                 ≈ $334 Billion
```

These indicators define the macroeconomic environment referenced by the benchmark.

The index functions as a normalized macroeconomic reference layer within this currency system.

------------------------------------------------------------

## Governance Framework

The NXY protocol is administered through a timelock-controlled governance system.

Governance responsibilities include:

```
- oracle signer approval
- signer class configuration
- data source set management
- methodology identifier updates
- reference asset configuration
- protocol parameter updates
- incident governance
- protocol upgrades
```

Governance actions are recorded on-chain and remain publicly auditable.

Governance cannot directly determine benchmark values.

------------------------------------------------------------

## Benchmark Integrity Safeguards

The protocol incorporates multiple safeguards designed to preserve benchmark integrity.

```
- deterministic benchmark calculation
- oracle signature verification
- signer diversity enforcement
- timestamp validation
- reference asset verification
- publication bounds checking
- timelock governance controls
- immutable on-chain observation history
```

These safeguards ensure that benchmark observations remain transparent and reproducible.

------------------------------------------------------------

## Transparency

The NXY protocol is designed to provide complete operational transparency.

```
- smart contract source code is publicly accessible
- benchmark observations are permanently recorded on-chain
- governance actions are publicly visible
- methodology documentation is maintained within the repository
```

Independent observers can verify benchmark calculations and protocol operations.

------------------------------------------------------------

## Disclaimer

The NXY benchmark is a macroeconomic reference index.

The protocol does not represent:

```
- a financial product
- a security
- a derivative instrument
- a trading venue
- a currency reserve system
```

The benchmark publishes normalized reference values derived from external market data.

------------------------------------------------------------

## Repository

Official repository

https://github.com/abba-platforms/NXY

------------------------------------------------------------
