# Naira Index (NXY) Whitepaper

## A Blockchain-Native Benchmark Referencing the Nigerian Naira

By [Simon Kapenda](https://linkedin.com/in/simonkapenda)        
Company: [Abba Industries Inc.](https://abbaii.com)     
Version 1.0        
Status: Mainnet Deployed        
Network: BNB Chain          
Repository: [https://github.com/abba-platforms/NXY](https://github.com/abba-platforms/NXY)          

---

# Abstract

The Naira Index (NXY) is a blockchain-native benchmark designed to reference the Nigerian Naira against the United States Dollar Index (DXY), using [cNGN](https://cngn.co) as the on-chain Naira reference layer. The protocol introduces an institutional-grade architecture through which benchmark observations are validated, published, governed, and archived directly on a public blockchain.

NXY is implemented as a modular protocol deployed on BNB Chain. The system consists of a core contract, an oracle policy module responsible for validating benchmark publications, a compliance module enforcing protocol-level restrictions, a publication manager responsible for maintaining the canonical benchmark state, a compliance administration contract, an analytical lens, and a governance timelock controlling administrative actions.

The benchmark is designed to express the relative strength of the Nigerian Naira within a global dollar context. DXY provides the global dollar benchmark. cNGN provides the on-chain Naira reference layer. The NXY level is then produced through a defined benchmark relationship between those two inputs.

This architecture allows benchmark publications to be produced and validated deterministically while remaining fully transparent and auditable. The protocol is designed for integration into digital financial systems, analytics platforms, macroeconomic research systems, and programmable market infrastructure.

---

# 1. Introduction

Benchmark indices play a fundamental role in global financial markets. They provide standardized reference values used in pricing, portfolio analysis, derivative construction, and macroeconomic observation. Currency benchmarks, equity indices, and sector-specific indicators are widely used to measure market performance and to facilitate financial products.

One of the most widely referenced currency benchmarks in the world is the United States Dollar Index, commonly known as DXY. DXY measures the strength of the United States Dollar relative to a basket of major global currencies, including the Euro, Japanese Yen, British Pound, Canadian Dollar, Swedish Krona, and Swiss Franc. When DXY rises, it generally indicates a strengthening United States Dollar relative to that basket. When DXY declines, it generally indicates a weakening United States Dollar.

Despite the importance of benchmark systems, most benchmark calculation, validation, and distribution infrastructures remain centralized. Index administration typically occurs through proprietary systems operated by private institutions.

The Naira Index introduces a different model. Rather than distributing benchmark data through centralized channels, NXY publishes benchmark observations through an on-chain protocol. This approach provides a transparent and verifiable record of benchmark publications while enabling integration with programmable financial systems.

NXY is specifically designed to benchmark the Nigerian Naira against DXY, using cNGN as the on-chain reference layer for Naira-denominated value. By implementing deterministic validation policies and governance-controlled updates, the NXY protocol creates a benchmark infrastructure that is auditable, programmable, and institutionally structured.

---

# 2. Design Principles

The design of the NXY protocol is guided by several core principles.

First, benchmark publications must be deterministic. A valid publication must satisfy predefined validation rules and must be verifiable through the oracle validation system.

Second, governance actions must be transparent. Administrative changes occur through a timelock-governed process, ensuring that protocol changes are observable before execution.

Third, system architecture must remain modular. Separating validation, compliance enforcement, publication management, administration, and read access allows the protocol to evolve without compromising operational stability.

Fourth, benchmark data must remain auditable. Every accepted publication, invalidation event, restoration event, and governance action is recorded on-chain, providing a verifiable historical record of benchmark activity.

Fifth, the benchmark must remain methodologically neutral. The protocol is designed to publish a benchmark reference value. It is not designed to operate as a trading venue, a reserve manager, or a market intervention mechanism.

---

# 3. Benchmark Definition

NXY is a benchmark that references the Nigerian Naira within a global dollar context.

The benchmark uses two primary inputs.

The first input is DXY, the United States Dollar Index, which serves as the global dollar benchmark.

The second input is the Naira-per-dollar exchange relationship represented on-chain through cNGN. Because cNGN functions as the on-chain Naira reference layer, the protocol uses cNGN per USD as the benchmark's on-chain Naira-side input.

Accordingly, the benchmark is defined conceptually as:

NXY = DXY divided by NGN per USD

In protocol terms, the operational relationship is:

NXY = DXY divided by cNGN per USD

under the assumption that cNGN is the on-chain Naira reference layer.

This means NXY rises when the Naira strengthens relative to the dollar benchmark environment and declines when the Naira weakens relative to the dollar benchmark environment.

---

# 4. Core Benchmark Formula

The benchmark methodology must define the mathematical process used to derive the published NXY level.

Let the following variables be defined.

DXY_t = United States Dollar Index level at time t
NGNUSD_t = Nigerian Naira per one United States Dollar at time t
cNGNUSD_t = cNGN per one United States Dollar at time t
NXY_t = Naira Index level at time t

Under the protocol's reference-layer design, cNGNUSD_t is used as the on-chain representation of NGNUSD_t.

The benchmark formula is therefore:

NXY_t = DXY_t / cNGNUSD_t

or equivalently:

NXY_t = DXY_t / NGNUSD_t

where the second expression represents the economic meaning of the benchmark and the first expression represents the operational on-chain implementation.

This formula produces a unitless benchmark ratio. It is not a claim of ownership, redemption, or entitlement. It is a benchmark level derived from the relationship between the global dollar benchmark and the on-chain Naira reference layer.

---

# 5. On-Chain Fixed-Point Representation

The protocol uses fixed-point integer representation for publication and storage.

Let:

dxyLevelE18_t = DXY_t scaled by 10^18
cngnPerUsdE18_t = cNGNUSD_t scaled by 10^18
nxyLevelE18_t = NXY_t scaled by 10^18

The on-chain publication convention is:

nxyLevelE18_t = floor( dxyLevelE18_t * 10^18 / cngnPerUsdE18_t )

where floor(x) denotes truncation toward zero for deterministic integer arithmetic.

This representation allows benchmark values to be published, validated, archived, and retrieved in a deterministic manner without relying on floating-point arithmetic.

The methodology therefore distinguishes between the economic formula and the storage formula.

Economic formula:

NXY_t = DXY_t / cNGNUSD_t

Storage formula:

nxyLevelE18_t = floor( dxyLevelE18_t * 10^18 / cngnPerUsdE18_t )

---

# 6. Illustrative Simulation

Using the benchmark relationship:

NXY = DXY divided by NGN per USD

and the following illustrative macro inputs:

DXY approximately 98.95
USD/NGN approximately 1401.40

the simulated benchmark level is:

NXY approximately 98.95 / 1401.40
NXY approximately 0.0706

This simulation indicates that the benchmark level increases when the Naira strengthens and decreases when the Naira weakens, holding the dollar benchmark relationship constant.

This is precisely the directional behavior intended by the protocol design.

---

# 7. Protocol Architecture

The NXY protocol is composed of several specialized smart contracts that together manage benchmark validation, governance, publication, compliance, and data accessibility.

At the center of the architecture is the core contract. The core contract coordinates the operation of the system and links the other protocol modules together.

The oracle policy module is responsible for validating benchmark publications. It verifies oracle signatures, enforces signer diversity requirements, checks publication bounds, validates source-set references, and ensures that publications conform to the protocol's configured methodology and operational state.

The publication manager maintains the canonical record of accepted publications. It stores publication history, manages lifecycle transitions, and allows the protocol to invalidate, restore, or reinstate publications during exceptional events.

The compliance module enforces protocol-level compliance rules and can apply restrictions where necessary.

The compliance administration contract provides controlled administrative access to compliance functions.

Governance actions are controlled through a timelock controller. This contract introduces a mandatory delay between the scheduling and execution of governance actions, providing transparency and preventing immediate administrative changes.

An analytical lens contract provides read-optimized access to protocol data and enables external systems to query the state of the benchmark efficiently.

---

# 8. Architecture Flow

The contract architecture can be described as follows.

```text
                        +---------------------------+
                        |     TimelockController    |
                        |   Governance Authority    |
                        +-------------+-------------+
                                      |
                                      v
                        +---------------------------+
                        |      NairaIndexNXY        |
                        |      Core Proxy/Core      |
                        +------+------+------+------+
                               |      |      |
                               |      |      |
                               v      v      v
                   +-----------+   +--+------+-------------+
                   | Oracle    |   | Compliance Module     |
                   | Policy    |   | Restriction Layer     |
                   | Module    |   +-----------------------+
                   +-----+-----+
                         |
                         v
               +------------------------+
               |   Publication Manager  |
               | Canonical NXY State    |
               +-----------+------------+
                           |
                           v
               +------------------------+
               |      Core Lens         |
               |  Read-only Analytics   |
               +------------------------+

Additional administration:

               +------------------------+
               |  Compliance Admin      |
               |  Controlled Admin      |
               +------------------------+
```

This modular structure separates publication validation from publication storage, and separates compliance administration from benchmark governance.

---

# 9. Core Contract

The NairaIndexNXY core contract serves as the central coordination layer of the protocol. It connects the oracle validation system, compliance enforcement module, and publication manager.

The core contract does not itself calculate benchmark values from first principles during normal operation. Instead, it acts as the benchmark controller, receiving validated publications and ensuring that the protocol modules operate consistently.

The contract is implemented using the UUPS upgradeable proxy pattern. This design allows the implementation logic to evolve while preserving the stability of the deployed proxy address, which functions as the canonical operational address of the protocol.

The core contract is also responsible for binding the deployed oracle policy module, compliance module, and publication manager into a single coherent benchmark system.

---

# 10. Oracle Validation

Benchmark publications are validated through the oracle policy module. This module ensures that proposed benchmark values meet the protocol's validation rules before they are accepted.

Oracle signers are organized into classes. Each class represents a category of participant in the publication process. The protocol enforces diversity requirements so that no single category of signer can control benchmark publications.

Publications must contain valid signatures from approved oracle signers. The oracle policy module verifies signature authenticity and checks that the required number of distinct signer classes participated in the publication.

The oracle policy module also validates the benchmark context of a publication. This includes checking the methodology state, source-set configuration, publication bounds, and reference asset consistency. It validates the use of the correct on-chain Naira reference layer through reference asset checks and code-hash validation.

The module therefore functions as the institutional control surface for publication integrity.

---

# 11. Publication Management

The publication manager is responsible for maintaining the canonical NXY benchmark state.

When the oracle module validates a publication, the publication manager records the observation and updates the current benchmark state. Each publication includes metadata such as valuation time, source-set identifier, incident identifier, signer metadata, and publication hash.

The contract maintains a history of accepted publications and stores a ring buffer of recent observations. This design allows efficient retrieval of historical data while maintaining a permanent on-chain record.

The publication manager also handles exceptional conditions such as publication invalidation, restoration, and reinstatement. If a publication is determined to be invalid, the protocol can revert to the most recent valid observation.

---

# 12. Publication Lifecycle

The publication lifecycle can be described as follows.

```text
Submitted Publication
        |
        v
Oracle Validation
        |
        +---- rejected ---> no state change
        |
        v
Accepted Publication
        |
        +---- incident review ---> invalidated publication
        |
        v
Canonical Active Publication
        |
        +---- restoration path ---> last good publication activated
        |
        v
Reinstated Publication (if governance restores a valid invalidated publication)
```

This lifecycle allows the benchmark to preserve continuity while maintaining transparent records of operational exceptions.

---

# 13. Compliance Layer

The compliance module enforces protocol-level restrictions. It allows the system to apply compliance rules without modifying the core benchmark logic.

This design enables regulatory, operational, or administrative constraints to be implemented independently of the benchmark publication system.

The compliance administration contract provides a controlled administrative surface for compliance management while preserving separation from the benchmark publication logic.

---

# 14. Governance

Protocol governance is controlled by a timelock controller contract.

All governance actions must first be scheduled through the timelock before they can be executed. This introduces a mandatory delay that allows observers to review upcoming changes.

Governance actions include updating oracle configuration parameters, modifying publication bounds, approving oracle signers, updating methodology identifiers, configuring source sets, and controlling module-level benchmark policies.

The timelock ensures that protocol administration remains transparent and auditable.

---

# 15. Reference Asset

The NXY protocol references the Nigerian Naira through a stablecoin representation on BNB Chain.

Reference Asset
[cNGN Stablecoin](https://cngn.co)
0xa8AEA66B361a8d53e8865c62D142167Af28Af058

Within the protocol, cNGN serves as the on-chain Naira reference layer. The oracle validation system verifies the contract code hash of the reference asset to ensure that the correct asset is used during benchmark publication validation.

This design does not mean cNGN is the benchmark. Rather, cNGN is the on-chain reference layer used to express the Naira side of the benchmark relationship.

---

# 16. Mainnet Deployment

The NXY protocol is deployed on BNB Chain mainnet.

Core Proxy
0xfDA959889a4b8e189D17cA161C74A348cc1Ca57B

Core Implementation
0x5eF22FDd5495855e91AD83f708A97BEE46c1E2aE

Oracle Policy Module
0x0BfA4CF9E8e75c933B698000EEd14998B04C3CeE

Compliance Module
0x3918CCEc1D8D5148757259F7FdC1E0935607866b

Publication Manager
0x01Efaf16915080057e14646D8Cf6Ac15Fb172cD6

Compliance Administration
0xc05A67D07592a3E644b67E4223b2fce7817209EE

Analytical Lens
0x141b706b0b75Fdf6F2aC7f83c6EB89F1BF0239A3

Governance Timelock
0x0e5493b11edbC2d251423c8ae22A40Ce9afDd3be

The core proxy is the canonical operational address of the benchmark system. The implementation contract contains the current logic used by the proxy under the UUPS upgrade pattern.

---

# 17. Tokenomics

The Naira Index (NXY) has a fixed maximum supply of 100,000,000,000 NXY.

No supply may be created above this maximum amount. The supply framework is designed to support market access, institutional alignment, strategic capital formation, treasury resilience, and orderly liquidity formation. The allocation model is intended to balance protocol growth with governance discipline and long-term benchmark credibility.

The total maximum supply is allocated as follows:

* 35.00% to Centralized Exchange Listing and Market Distribution
* 25.00% to Founding, Management, and Staff
* 15.00% to Strategic Investment
* 20.00% to Treasury
* 5.00% to Liquidity

The corresponding NXY token amounts are:

* Centralized Exchange Listing and Market Distribution: 35,000,000,000 NXY
* Founding, Management, and Staff: 25,000,000,000 NXY
* Strategic Investment: 15,000,000,000 NXY
* Treasury: 20,000,000,000 NXY
* Liquidity: 5,000,000,000 NXY

The allocation structure is shown below.

```
+---------------------------------------------+------------+----------------------+
| Allocation Category                         | Percentage | Token Amount         |
+---------------------------------------------+------------+----------------------+
| CEX Listing and Market Distribution         | 35.00%     | 35,000,000,000 NXY   |
| Founding, Management, and Staff             | 25.00%     | 25,000,000,000 NXY   |
| Strategic Investment                        | 15.00%     | 15,000,000,000 NXY   |
| Treasury                                    | 20.00%     | 20,000,000,000 NXY   |
| Liquidity                                   | 5.00%      |  5,000,000,000 NXY   |
+---------------------------------------------+------------+----------------------+
| Total                                       | 100.00%    | 100,000,000,000 NXY  |
+---------------------------------------------+------------+----------------------+
```

The supply distribution can also be represented as follows.

```
                       NXY MAX SUPPLY
                    100,000,000,000 NXY
                               |
--------------------------------------------------------------------------
|                        |                  |             |              |
v                        v                  v             v              v

35,000,000,000         25,000,000,000    15,000,000,000 20,000,000,000 5,000,000,000
CEX Listing and        Founding,         Strategic       Treasury       Liquidity
Market Distribution    Management,       Investment
and Staff
```

The tokenomics model is not intended to function as a speculative release schedule. It is intended to define the economic role of each supply bucket, the governance controls attached to each bucket, and the institutional restrictions that govern release, vesting, and market deployment.

## 17.1 Fixed Supply Discipline

The NXY supply is fixed. This means the protocol operates without discretionary inflation above the maximum amount of 100,000,000,000 NXY.

The fixed supply model supports benchmark credibility in three ways.

First, it establishes issuance discipline. Market participants can verify that the benchmark token cannot be expanded arbitrarily after deployment.

Second, it improves governance clarity. Any release of NXY must come from an already defined and allocated bucket rather than from unbounded future minting.

Third, it improves institutional reviewability. A fixed supply framework allows investors, analysts, exchanges, and governance observers to evaluate the benchmark's economic structure without uncertainty about future inflation.

The fixed supply model therefore forms part of the benchmark's broader integrity framework.

## 17.2 Centralized Exchange Listing and Market Distribution

The Centralized Exchange Listing and Market Distribution allocation consists of 35,000,000,000 NXY, representing 35.00% of total maximum supply.

This allocation exists to support exchange onboarding, listing programs, launch pools, controlled market distribution, venue inventory support, exchange-side market access, and geographically diversified benchmark availability.

Because NXY is intended to operate as benchmark infrastructure rather than a closed internal token, broad market distribution is a legitimate strategic requirement. However, this category must be handled as a controlled distribution pool rather than as unrestricted immediately liquid supply.

The CEX allocation should therefore be released in tranches tied to listing strategy, venue readiness, market depth, and governance authorization. Releases from this bucket should correspond to identifiable distribution objectives such as:

* primary listing inventory
* exchange launch campaigns
* benchmark exposure distribution programs
* secondary market access expansion
* venue-specific market support

This category should not be treated as a discretionary reserve for unmanaged disposal. Each release should be documented and mapped to a defined market access purpose.

The operational logic of this category can be illustrated as follows.

```
               CEX LISTING AND MARKET DISTRIBUTION
                       35,000,000,000 NXY
                                 |
      -------------------------------------------------------
      |                     |                 |              |
      v                     v                 v              v
Exchange Listing      Launch Programs    Venue Inventory   Controlled
Inventory             and Distribution   and Pairing       Market Release

```

The institutional treatment for this bucket is as follows:

* release in staged tranches
* tie release to specific venue objectives
* avoid unrestricted immediate full circulation
* maintain internal release ledger and governance disclosure
* separate exchange inventory from liquidity inventory

## 17.3 Founding, Management, and Staff

The Founding, Management, and Staff allocation consists of 25,000,000,000 NXY, representing 25.00% of total maximum supply.

Institutionally, this category should not remain economically undifferentiated. It is stronger to define this allocation as a controlled alignment pool composed of separate internal sub-buckets for founder leadership, management, and staff incentives.

The internal structure is shown below.

```
+---------------------------------------------+----------------------+
| Internal Sub-Allocation                     | Token Amount         |
+---------------------------------------------+----------------------+
| Founding and Core Leadership                | 15,000,000,000 NXY   |
| Management and Senior Operators             |  5,000,000,000 NXY   |
| Staff and Long-Term Incentive Pool          |  5,000,000,000 NXY   |
+---------------------------------------------+----------------------+
| Total                                       | 25,000,000,000 NXY   |
+---------------------------------------------+----------------------+

```

This internal differentiation is preferable because founder control, executive alignment, and staff incentives do not serve the same economic function.

Founding and Core Leadership allocation exists to align long-term protocol stewardship with long-term benchmark development.

Management and Senior Operators allocation exists to align ongoing execution capacity with long-duration protocol objectives.

Staff and Long-Term Incentive allocation exists to support retention, performance alignment, and ecosystem operating continuity.

The release discipline for this category is critical. Immediate unrestricted liquidity for this bucket would materially weaken institutional credibility. Accordingly, this category should be governed by formal lock-up and vesting.

A recommended institutional vesting model is:

* Founding and Core Leadership: 12-month cliff, followed by 48-month linear vesting
* Management and Senior Operators: 12-month cliff, followed by 36-month linear vesting
* Staff and Long-Term Incentive Pool: 6-month cliff, followed by 24 to 48-month linear vesting depending on award structure

The release logic can be illustrated as follows.

```
            FOUNDING, MANAGEMENT, AND STAFF
                    25,000,000,000 NXY
                           |
    -------------------------------------------------------
    |                      |                              |
    v                      v                              v
Founding and Core      Management and Senior        Staff and Long-Term
Leadership             Operators                    Incentive Pool
15,000,000,000         5,000,000,000               5,000,000,000
|                      |                              |
v                      v                              v
12m cliff + 48m       12m cliff + 36m             6m cliff + linear
linear vesting        linear vesting              vesting

```

This structure strengthens institutional alignment while reducing the risk of concentrated early token release.

## 17.4 Strategic Investment

The Strategic Investment allocation consists of 15,000,000,000 NXY, representing 15.00% of total maximum supply.

This allocation is intended for long-term strategic counterparties capable of advancing the benchmark ecosystem. This may include institutional backers, data partners, financial infrastructure counterparties, distribution partners, ecosystem operators, strategic advisors, and capital providers whose participation materially strengthens the benchmark's long-term position.

This bucket should not be treated as a general sale pool for short-duration speculative allocation. It should instead be reserved for counterparties whose participation improves benchmark adoption, governance maturity, technical infrastructure, data integrity, or market distribution.

Strategic allocation decisions should be linked to defined strategic outcomes such as:

* exchange expansion
* benchmark distribution
* data source strengthening
* infrastructure integration
* ecosystem credibility
* jurisdictional market access

Recommended treatment of this category includes:

* negotiated lock-ups
* milestone-based releases
* structured vesting
* transparent internal allocation records
* restrictions on immediate unrestricted secondary-market sale

The strategic investment pathway can be represented as follows.

```
                STRATEGIC INVESTMENT
                 15,000,000,000 NXY
                         |
    --------------------------------------------------
    |                   |               |            |
    v                   v               v            v
Institutional         Data and Source   Infrastructure  Strategic
Partners              Partners          Partners        Capital
```

A properly controlled strategic bucket strengthens institutional maturity without undermining market structure.

## 17.5 Treasury

The Treasury allocation consists of 20,000,000,000 NXY, representing 20.00% of total maximum supply.

The treasury is the protocol's long-term strategic reserve. It exists to support benchmark continuity, protocol administration, ecosystem development, legal structuring, infrastructure maintenance, research, contingency operations, and governance-approved strategic expenditures.

This bucket should not be interpreted as discretionary founder inventory. Institutionally, treasury supply should be governed as a strategic reserve subject to governance controls, reporting discipline, and policy-based use.

Treasury capital may support categories such as:

* protocol operations
* infrastructure and maintenance
* legal and compliance structuring
* ecosystem development
* research and analytics
* emergency reserve support
* governance-approved strategic initiatives

Recommended treasury policy is as follows:

* treasury deployment subject to governance authorization
* no unilateral discretionary extraction
* documented release purpose
* periodic treasury reporting
* separation between treasury reserve and market distribution inventory

The treasury flow can be represented as follows.

```
                       TREASURY
                20,000,000,000 NXY
                           |
------------------------------------------------------------------
|                |                |              |               |
v                v                v              v               v

Operations      Infrastructure    Legal and      Research and    Strategic
and Admin       and Maintenance   Compliance     Analytics       Reserve Use
```

A treasury of this size is appropriate for a benchmark protocol if and only if governance discipline is strong and treasury releases are policy-based.

## 17.6 Liquidity

The Liquidity allocation consists of 5,000,000,000 NXY, representing 5.00% of total maximum supply.

This category exists to support market formation and benchmark accessibility. It is intended for designated liquidity provisioning, approved market-making support, pair support, venue-side inventory balancing, and orderly secondary market operation.

Because NXY is benchmark infrastructure, liquidity support is operationally important. However, this bucket must remain distinct from the CEX listing and market distribution bucket. The CEX bucket supports exchange access and distribution. The liquidity bucket supports actual market functioning once access exists.

The use cases include:

* exchange pair liquidity support
* approved market maker operations
* benchmark trading depth support
* venue-specific liquidity deployment
* controlled rebalancing under policy

The liquidity deployment framework is also represented as follows.

```
                       LIQUIDITY
                 5,000,000,000 NXY
                           |
    --------------------------------------------------
    |                   |               |            |
    v                   v               v            v
Exchange Pair         Market Making    Trading Depth   Venue Support
Support               Support          Support         Operations

```
This bucket should not be repurposed as a hidden discretionary release pool. Every deployment from this category should be operationally justified and recorded.

## 17.7 Institutional Release Discipline

Institutional tokenomics is defined not only by percentage allocation, but by release policy. Without release discipline, a token allocation framework may be mathematically coherent yet economically weak.

The NXY release discipline should therefore follow a category-specific model.

```
+---------------------------------------------+--------------------------------------+
| Allocation Category                         | Recommended Release Discipline       |
+---------------------------------------------+--------------------------------------+
| CEX Listing and Market Distribution         | Tranche-based operational release    |
| Founding, Management, and Staff             | Cliff plus long linear vesting       |
| Strategic Investment                        | Lock-up plus scheduled vesting       |
| Treasury                                    | Governance-controlled release        |
| Liquidity                                   | Operational deployment only          |
+---------------------------------------------+--------------------------------------+

```
A stronger benchmark-grade release schedule is:

```
+---------------------------------------------+--------------------------------------+
| Allocation Category                         | Recommended Timing Structure         |
+---------------------------------------------+--------------------------------------+
| Founding and Core Leadership                | 12m cliff + 48m linear vesting      |
| Management and Senior Operators             | 12m cliff + 36m linear vesting      |
| Staff and Long-Term Incentive Pool          | 6m cliff + 24 to 48m vesting        |
| Strategic Investment                        | Negotiated lock-up + vesting        |
| CEX Listing and Market Distribution         | Venue-linked tranche release        |
| Treasury                                    | Timelock and governance controlled  |
| Liquidity                                   | Deployment as needed under policy   |
+---------------------------------------------+--------------------------------------+

```

This category-specific discipline reduces the probability of disorderly release, strengthens governance credibility, and better aligns token structure with institutional benchmark positioning.

## 17.8 Market Structure Logic

The tokenomics framework follows a layered market structure rather than a flat distribution model.

```
                    NXY ECONOMIC STRUCTURE
                 100,000,000,000 NXY MAX SUPPLY
                               |
-------------------------------------------------------------------
|                        |                  |           |           |
v                        v                  v           v           v
Market Access           Long-Term           Strategic    Reserve      Market
and Distribution        Alignment           Capital      Governance   Function
35.00%                  25.00%              15.00%       20.00%       5.00%
CEX                     Founding,           Strategic    Treasury     Liquidity
Listing and             Management,         Investment
Market                  and Staff
Distribution

```
The logic of this model is as follows.

CEX Listing and Market Distribution creates access.

Founding, Management, and Staff creates long-duration alignment.

Strategic Investment creates ecosystem leverage.

Treasury creates continuity and resilience.

Liquidity creates market function.

This structure is institutionally stronger than a flat undifferentiated distribution model because each bucket has a clearly separable economic role.

## 17.9 Tokenomics Integrity Statement

The NXY tokenomics framework is designed to support benchmark distribution, governance resilience, market accessibility, and long-term ecosystem stewardship.

The fixed maximum supply establishes hard issuance discipline. The allocation model defines the economic roles of exchange access, team alignment, strategic capital, treasury reserve, and liquidity support. The vesting and release discipline recommended for each bucket is intended to reduce short-term imbalance and strengthen long-term institutional credibility.

The credibility of the tokenomics framework depends not only on the percentage split, but on governance control, release transparency, operational segregation of supply buckets, and discipline in treasury and market-use policy.

For that reason, the NXY tokenomics framework should be interpreted as a governed benchmark-capital structure rather than a simple static token allocation table.

## 17.10 Institutional Allocation Summary

The proposed allocation is retained in institutional form if paired with internal segmentation and release controls.

The institutional tokenomics is therefore:

```
+---------------------------------------------+------------+----------------------+
| Allocation Category                         | Percentage | Token Amount         |
+---------------------------------------------+------------+----------------------+
| CEX Listing and Market Distribution         | 35.00%     | 35,000,000,000 NXY  |
| Founding, Management, and Staff             | 25.00%     | 25,000,000,000 NXY  |
| Strategic Investment                        | 15.00%     | 15,000,000,000 NXY  |
| Treasury                                    | 20.00%     | 20,000,000,000 NXY  |
| Liquidity                                   | 5.00%      |  5,000,000,000 NXY  |
+---------------------------------------------+------------+----------------------+
| Total                                       | 100.00%    | 100,000,000,000 NXY |
+---------------------------------------------+------------+----------------------+

```

This should be adopted together with:

* internal subdivision of the 25.00% founder and team bucket
* formal vesting policy
* treasury release governance
* operational separation between CEX inventory and liquidity inventory
* internal disclosure records for material token releases

This section completes the economic structure of NXY in a manner consistent with institutional benchmark infrastructure.

---

# 18. Security Model

Security in the NXY protocol is achieved through layered safeguards.

Governance changes are delayed through the timelock controller. Oracle publications require signatures from multiple approved signers with signer-class diversity. Publication bounds prevent extreme values from being accepted. Reference asset code-hash checks prevent unexpected asset substitution. Incident handling mechanisms allow invalid data to be removed while preserving historical transparency.

Together, these mechanisms create a robust framework for maintaining benchmark integrity.

---

# 19. Transparency

All protocol actions are publicly observable. Benchmark publications, governance actions, signer updates, source-set configuration changes, and incident declarations are permanently recorded on the blockchain.

This design ensures that the entire lifecycle of the benchmark can be independently verified.

---

# 20. Benchmark Methodology

The Naira Index is constructed as a benchmark designed to reflect the relative value dynamics between the Nigerian Naira and the global dollar benchmark environment represented by DXY. The benchmark is intended to operate as a standardized reference instrument rather than a representation of any single trading venue or exchange facility.

The calculation methodology is deterministic and governed by the oracle validation module. Each publication contains the NXY benchmark level, the DXY reference level, and the cNGN-per-USD value used as the on-chain Naira reference layer.

The benchmark does not attempt to price DXY in Naira as a traded instrument. Rather, it expresses the relative benchmark relationship:

NXY = DXY divided by cNGN per USD

The methodology parameters, including the methodology hash, basket definition hash, and data-source definition hash, are recorded on-chain. Any modification to these values requires governance approval through the timelock controller.

---

# 21. Oracle Governance Policy

Oracle signers are responsible for producing benchmark publications. The oracle system is designed to ensure that no single participant can unilaterally determine the benchmark value.

Signers are organized into distinct signer classes. Each class represents a category of participant within the publication process. The protocol requires that publications contain signatures from multiple signer classes in order to ensure diversity in the validation process.

Oracle signers are approved through governance. The timelock-controlled governance process authorizes the addition or removal of signers and determines the class to which each signer belongs.

Signer participation is governed by activation timestamps that determine when a signer becomes eligible to participate in benchmark publications. Governance may deactivate a signer if operational, security, or governance conditions require it.

This design ensures that the oracle layer remains both transparent and resilient.

---

# 22. Incident and Market Disruption Policy

Financial markets occasionally experience abnormal conditions that may affect the reliability of benchmark data. The NXY protocol includes mechanisms for managing such situations.

The protocol defines incident states that allow the system to respond to abnormal market conditions. An incident may be declared when the integrity of a publication is in doubt or when the underlying data environment becomes unreliable.

During an incident, the publication manager may invalidate the latest publication if it is determined that the data is incorrect. The protocol then retains the most recent valid publication as the canonical reference.

If necessary, governance may activate market disruption mode. This mode temporarily adjusts validation behavior in order to allow controlled operation during periods of instability.

Once conditions normalize, the protocol can restore or reinstate previously accepted publications. All incident actions are recorded on-chain to maintain transparency.

---

# 23. Data Source Policy

The accuracy of a benchmark depends on the integrity of its underlying data sources. The NXY protocol manages data sources through the concept of source sets.

A source set represents a defined group of data inputs used to construct a benchmark observation. Each source set is identified by a unique identifier and associated policy hash.

Source sets are approved through governance. The oracle validation module verifies that publications reference an approved source set before they can be accepted.

Source sets may represent combinations of institutional data providers, aggregated market feeds, or other verified reference inputs.

The protocol can require multiple approved source sets to operate simultaneously. This requirement provides redundancy and improves the robustness of the benchmark publication process.

---

# 24. Benchmark Publication Policy

Benchmark publications occur when oracle signers submit validated observations to the protocol. Each publication represents a new benchmark observation derived from the current market environment.

Publications include the NXY benchmark level, DXY reference level, cNGN-per-USD input, and supporting metadata. The publication manager verifies that the submission has been validated by the oracle policy module before recording the observation.

Publications must occur within the acceptable time window defined by the oracle validation parameters. Submissions that exceed this window are rejected.

Each accepted publication becomes the canonical benchmark value until a new validated publication replaces it.

The publication history is stored permanently on-chain and can be retrieved through the protocol's analytical interfaces.

---

# 25. Index Integrity Safeguards

The NXY protocol incorporates multiple safeguards designed to protect the integrity of the benchmark.

Oracle validation ensures that publications originate from approved signers. Signer-class diversity prevents any single category of participant from dominating the publication process.

Publication bounds restrict acceptable value ranges, preventing abnormal values from entering the system.

The protocol also verifies the contract code hash of the reference asset used in calculations. This verification ensures that the correct asset contract is used during validation.

If a publication is determined to be invalid, the protocol allows governance to invalidate the observation and revert to the most recent valid publication.

Together, these safeguards create a robust system for maintaining benchmark integrity.

---

# 26. Upgrade Governance Policy

The NXY protocol uses an upgradeable proxy architecture to allow controlled evolution of the system.

Upgrades are governed through the timelock controller. Any upgrade must first be scheduled through the governance process and must respect the mandatory delay before execution.

This delay ensures that protocol participants have time to review and analyze proposed changes.

Upgrades may include improvements to contract logic, security enhancements, or operational optimizations. However, all upgrades remain subject to governance transparency.

The proxy architecture allows the protocol to evolve while preserving the stability of the deployed core proxy address.

---

# 27. Risk Disclosure

Like any blockchain-based infrastructure, the NXY protocol operates within a set of inherent risks.

Smart contract risk arises from the possibility of vulnerabilities in contract code. Although the protocol is designed with security safeguards, software systems may contain unforeseen defects.

Oracle risk arises from the reliance on external participants to produce benchmark observations. The protocol mitigates this risk through signer diversity and validation policies.

Reference asset risk arises from the reliance on cNGN as the on-chain Naira reference layer. Changes in the stability, operation, or availability of the reference asset could affect benchmark operation.

Benchmark input risk arises from the reliance on DXY as an external benchmark input. Errors, disruptions, or inconsistencies in DXY sourcing could affect benchmark publications.

Governance risk arises from the possibility that governance participants could approve changes that alter protocol behavior.

Blockchain infrastructure risk arises from reliance on the underlying blockchain network.

These risks are inherent to decentralized financial infrastructure and should be considered by all participants.

---

# 28. Protocol Operational Phases

The lifecycle of the NXY protocol can be understood in several operational phases.

The first phase is protocol deployment. During this phase, the core contracts and supporting modules are deployed and verified on the blockchain.

The second phase involves oracle configuration. Governance defines signer classes, approves oracle participants, configures source sets, and sets methodology state and benchmark bounds.

The third phase activates the publication system. Once oracle configuration is complete, benchmark publications can begin.

The final phase represents ongoing protocol operation. During this phase, governance may update parameters, add signers, change source-set policy, or modify methodology definitions through timelock-controlled processes.

This phased approach allows the protocol to transition from deployment to full operation in a controlled manner.

---

# 29. Data Accessibility

All protocol data is publicly accessible through the blockchain.

Benchmark publications are stored within the publication manager and can be retrieved through read-only contract calls. Historical observations can be accessed through the protocol's observation buffer and publication archive.

The analytical lens contract provides a simplified interface for retrieving protocol state information. This contract allows external systems to access benchmark data efficiently without requiring deep knowledge of the underlying contract architecture.

Developers, analysts, and researchers can therefore integrate NXY data into external applications and analytics platforms.

---

# 30. Benchmark Neutrality

The NXY protocol publishes benchmark data but does not participate in financial markets.

The protocol does not execute trades, manage assets, or represent ownership of any currency. Instead, it provides a reference value that can be used by external systems for analysis or financial applications.

The benchmark does not represent an investment product and should not be interpreted as financial advice.

Its role is to provide a transparent benchmark reference within programmable financial infrastructure.

---

# 31. Legal Classification

The Naira Index is a benchmark reference index. It does not represent a security, derivative instrument, debt claim, or ownership interest in any asset.

The protocol publishes a numerical reference value derived from benchmark methodology and external market data. This value is intended to function as a benchmark indicator rather than a financial product.

Participants using the benchmark remain responsible for ensuring compliance with applicable laws and regulations.

---

# 32. Terminology

Certain technical terms appear frequently throughout this document.

A publication refers to a validated benchmark observation submitted to the protocol.

A source set represents the approved data inputs used to construct benchmark values.

A signer class represents a category of oracle signer used to enforce diversity in the validation process.

The reference asset is the on-chain representation of the Nigerian Naira used during benchmark validation.

A valuation timestamp is the timestamp associated with the observed benchmark input values used in a publication.

A methodology hash is the on-chain identifier representing the approved benchmark methodology state.

Reference asset code hash refers to the code-hash fingerprint used to verify the configured on-chain reference asset.

Incident state refers to the protocol condition activated when abnormal benchmark conditions occur.

Market disruption mode refers to a controlled operating state used during extraordinary conditions.

Invalidation refers to removal of a publication from the active canonical benchmark sequence.

Reinstatement refers to restoration of a valid invalidated publication into active state.

---

# 33. Version Control

The NXY whitepaper and associated protocol documentation are maintained within the project repository.

Updates to the whitepaper may occur when methodology definitions evolve or when protocol functionality expands.

Each revision is tracked through repository version control to ensure transparency.

Methodology identifiers stored on-chain correspond to specific documented versions of the benchmark methodology.

---

# 34. Technical Appendix

The NXY protocol includes several contract interfaces and data structures that define the behavior of the system.

A publication includes benchmark fields such as nxyLevelE18, dxyLevelE18, cngnPerUsdE18, valuationTime, sourceSetId, incidentId, and signer metadata.

Oracle validation relies on signature verification, signer-class diversity checks, methodology-state checks, source-set validation, reference asset verification, and publication-bound enforcement.

The publication manager maintains storage structures that record accepted publications, restoration publications, and recent observations.

The deployed contract map consists of the core proxy, implementation, oracle policy module, compliance module, publication manager, compliance admin, analytical lens, and governance timelock.

These technical details can be reviewed directly within the smart contract repository.

---

# 35. Benchmark Governance Charter

The Naira Index protocol operates under a governance framework designed to preserve the integrity, neutrality, and transparency of the benchmark.

Governance authority is exercised through the protocol's timelock controller. This contract enforces a mandatory delay between the scheduling and execution of governance actions. The delay ensures that proposed changes remain observable before execution and provides the opportunity for review by participants and observers.

Governance authority includes responsibility for maintaining the operational parameters of the protocol. This includes approving oracle signers, managing signer classes, defining source sets, updating methodology identifiers, and adjusting operational bounds.

Governance decisions are executed exclusively through the on-chain timelock mechanism. No administrative changes may occur outside of this process. This ensures that governance actions remain publicly observable and permanently recorded on the blockchain.

The governance structure is designed to maintain the neutrality of the benchmark. Governance does not determine benchmark values and cannot directly publish benchmark observations. Instead, governance establishes the rules and operational framework within which the oracle system produces validated publications.

---

# 36. Index Administration Policy

The NXY protocol operates as a benchmark administration system. The protocol defines the rules under which benchmark values are validated, recorded, and made available to external systems.

Index administration is performed through the interaction of the protocol's modules. The oracle policy module validates benchmark observations, the publication manager records validated publications, and the compliance module enforces protocol-level restrictions where required.

The administration process ensures that benchmark publications follow the methodology and validation rules defined by the protocol. Publications that do not satisfy these requirements are rejected and cannot modify the canonical benchmark state.

The administration system also supports exceptional procedures for managing abnormal conditions. Through incident-management mechanisms, the protocol can invalidate erroneous publications or restore the most recent valid observation.

Index administration does not involve discretionary calculation of benchmark values. The protocol does not generate benchmark levels through administrative action. Instead, the administration layer ensures that submitted observations comply with the validation policies defined by the oracle module.

---

# 37. Index Calculation Formula

An institutional benchmark must provide a precise mathematical definition describing how the benchmark level is calculated from its underlying observations.

The NXY benchmark measures the relative value of the Nigerian Naira against the global dollar benchmark environment represented by DXY, using cNGN as the on-chain Naira reference layer.

Let the following variables be defined.

DXY_t = United States Dollar Index level at time t
cNGNUSD_t = cNGN per one United States Dollar at time t
NXY_t = Naira Index benchmark level at time t

The benchmark level is defined as:

NXY_t = DXY_t / cNGNUSD_t

This means the NXY level is the DXY level divided by the on-chain Naira-per-dollar reference-layer value.

If the Naira strengthens, cNGNUSD_t declines and NXY_t rises.

If the Naira weakens, cNGNUSD_t rises and NXY_t declines.

The protocol uses fixed-point scaling for deterministic blockchain representation. Let:

dxyLevelE18_t = DXY_t scaled by 10^18
cngnPerUsdE18_t = cNGNUSD_t scaled by 10^18
nxyLevelE18_t = NXY_t scaled by 10^18

Then the storage representation is:

nxyLevelE18_t = floor( dxyLevelE18_t * 10^18 / cngnPerUsdE18_t )

where floor(x) denotes truncation toward zero.

This formula defines how the benchmark is economically interpreted and how it is represented in protocol publication fields.

---

# 38. Observation Timing and Publication Frequency

Institutional benchmarks require clearly defined policies governing when observations may be submitted and how frequently benchmark levels are expected to be updated.

The NXY protocol operates on a periodic observation model in which validated observations of the benchmark inputs are submitted by authorized oracle participants. Each observation contains a valuation timestamp representing the time at which the underlying inputs were measured.

The protocol enforces ordering guarantees for observations. Observations must be submitted in chronological order relative to previously accepted publications. This ensures that the benchmark time series remains strictly monotonic in its progression of timestamps.

To prevent stale or manipulated observations from entering the benchmark, the protocol defines acceptable valuation timestamp windows. An observation timestamp must fall within an acceptable interval relative to the block timestamp at the time of submission.

Let:

T_block = block timestamp at submission
T_obs = observation timestamp

Then the observation must satisfy:

T_block - Delta_max <= T_obs <= T_block + Delta_forward

where Delta_max represents the maximum acceptable staleness tolerance and Delta_forward represents the maximum allowable forward timestamp skew.

Observations that violate these constraints are rejected by the oracle validation module.

Duplicate publications are prevented through ordering and publication-state rules enforced by the publication manager contract. Each accepted publication is assigned a unique chronological place within the benchmark history.

---

# 39. Data Quality and Validation Policy

Institutional benchmark providers maintain formal data-quality policies describing how raw input data is evaluated prior to inclusion in the benchmark. The NXY protocol enforces such policies through its oracle validation logic.

All observations submitted to the NXY protocol must satisfy deterministic validation checks before they are accepted for publication. These checks ensure that benchmark inputs remain consistent with expected market conditions and historical ranges.

One of the primary safeguards implemented by the protocol is abnormal-value detection. Observations that deviate significantly from previously published benchmark levels may be rejected if they exceed predefined deviation thresholds.

Let:

NXY_prev = previous benchmark level
NXY_candidate = candidate benchmark level derived from the submitted observation

Then the protocol may enforce a constraint of the form:

abs( NXY_candidate - NXY_prev ) <= Delta_threshold

where Delta_threshold represents the maximum acceptable deviation between consecutive benchmark levels under normal operating mode.

Additional validation checks include reference asset verification. Observations referencing incorrect or inconsistent reference assets are rejected by the oracle policy module. The smart contract verifies that the observed asset matches the configured reference asset and that its code hash remains consistent with protocol expectations.

These safeguards collectively ensure that the NXY benchmark reflects credible market observations and that corrupted or manipulated data cannot propagate through the benchmark history.

---

# 40. Benchmark Continuity Policy

Institutional benchmarks must provide clear procedures ensuring continuity during operational disruptions.

The NXY protocol implements benchmark continuity mechanisms through its publication manager contract. This component maintains the historical sequence of benchmark publications and enforces rules governing invalidation, restoration, and reinstatement.

If an observation is later determined to be invalid due to data errors or validation failures, the protocol allows governance-authorized actors to invalidate the corresponding publication. The invalidation process removes the affected publication from the active benchmark sequence while preserving a record of the event for historical transparency.

Following invalidation, the protocol may restore the benchmark to the most recent valid observation preceding the invalidated entry. This ensures that the benchmark continues to provide a stable reference even when erroneous data is detected.

In circumstances where oracle submissions are temporarily unavailable, the benchmark retains the last valid publication as the current reference value until a valid new publication is accepted.

These continuity mechanisms ensure that the benchmark remains stable and usable as a reference indicator even during periods of operational uncertainty.

---

# 41. Governance Transparency

Transparency is a fundamental requirement for institutional benchmark governance. Observers must be able to independently verify governance actions and understand how protocol parameters evolve over time.

The NXY protocol implements governance transparency through its on-chain governance model and timelock-controlled administrative functions. All governance operations are executed through publicly visible blockchain transactions that can be independently inspected.

Governance proposals affecting protocol configuration pass through a timelock delay before execution. This delay ensures that protocol participants and observers have sufficient time to review proposed changes prior to activation.

Each governance action generates an immutable transaction record containing the parameters of the change and the identity of the executing account. These records are permanently stored on-chain and can be inspected through blockchain explorers or analytical tools.

Through this mechanism, external observers can track the complete history of governance actions affecting the benchmark.

---

# 42. Benchmark Independence Statement

The NXY protocol does not operate a trading venue and does not facilitate currency exchange transactions. The protocol does not hold reserves of Nigerian Naira or United States Dollars and does not engage in market-making activities.

The benchmark does not influence exchange rates and does not attempt to stabilize or manipulate foreign exchange markets. Instead, the protocol observes externally generated benchmark inputs and derives a normalized benchmark level based on those observations.

Because the protocol functions as an observational benchmark system, its role is limited to publishing a deterministic representation of the relationship between the Nigerian Naira and the global dollar benchmark environment represented by DXY.

This independence ensures that the benchmark remains a neutral reference instrument suitable for analytical and informational purposes.

---

# 43. Index Usage and Integration

Although the NXY protocol itself does not operate financial products, the benchmark may be integrated by external systems that require a transparent macroeconomic reference.

Potential integration contexts include financial analytics platforms that monitor macroeconomic conditions affecting the Nigerian currency environment. Research institutions may incorporate the benchmark into analytical models evaluating currency trends and dollar-strength relationships.

Decentralized financial protocols may reference the benchmark when constructing instruments or contracts linked to macroeconomic indicators. Because the benchmark is published on-chain, these systems can retrieve benchmark values directly from the blockchain without relying on centralized data providers.

The protocol itself does not prescribe any specific use case. Instead, it provides a neutral benchmark reference that external systems may choose to incorporate according to their own requirements.

---

# 44. Open Source and Repository Policy

The NXY protocol is implemented as open-source infrastructure. The canonical implementation of the protocol is maintained in the public repository located at:

[https://github.com/abba-platforms/NXY](https://github.com/abba-platforms/NXY)

The repository contains the smart contract source code, documentation, deployment information, and historical revision records for the protocol. All changes to the protocol codebase are recorded through version control, allowing observers to review the evolution of the implementation over time.

Because the repository is publicly accessible, developers and researchers may inspect the source code to verify the correctness of the benchmark logic. The open-source model enables independent verification of the protocol and supports collaborative review of its architecture.

Documentation updates and methodology revisions are also recorded through the repository's version history.

---

# 45. Smart Contract Audit and Review

Security and correctness of the smart contract implementation are essential for maintaining trust in the benchmark.

The development process includes internal code review procedures designed to evaluate the correctness of the benchmark logic and the safety of the contract architecture. Developers examine potential failure scenarios and verify that critical invariants are preserved.

Because the protocol is open source, the smart contracts may also be inspected by independent developers and security researchers. This open-inspection model enables continuous community review of the protocol implementation.

Future independent security audits may be conducted to further evaluate the robustness of the protocol against adversarial conditions. Any such audits and their findings should be documented transparently within the protocol repository.

---

# 46. Benchmark Evolution Policy

Institutional benchmarks must define how methodological changes are introduced over time while preserving the integrity of historical data.

Changes to the protocol configuration may be introduced through governance proposals executed via the timelock-controlled administrative framework. These proposals may modify operational parameters or introduce new functionality consistent with the benchmark design principles.

Historical benchmark publications remain immutable once recorded on-chain. Even if the methodology evolves in future versions of the protocol, previously published benchmark levels remain permanently preserved within the blockchain history.

Version identifiers associated with the protocol implementation allow observers to determine which methodology version produced a given publication.

Through this structured evolution policy, the NXY benchmark can adapt to future requirements while preserving methodological transparency and historical continuity.

---

# 47. Benchmark Purpose Statement

The NXY benchmark is established to provide a transparent, deterministic, and publicly verifiable reference representing the relative value of the Nigerian Naira against the United States Dollar Index, using cNGN as the on-chain Naira reference layer.

The purpose of the benchmark is to transform observed benchmark relationships into a normalized reference level that can be independently verified and reproduced by observers using publicly available data. By expressing the NGN/USD relationship through a DXY-benchmarked index structure, the benchmark allows analysts, researchers, and financial systems to evaluate changes in the value environment of the Nigerian Naira through a consistent reference scale.

The benchmark therefore functions as a macroeconomic indicator and reference instrument. Its design allows external systems to use the index as an informational input for research, financial analysis, and integration into broader economic models.

---

# 48. Methodology Integrity Statement

The credibility of any benchmark depends on the consistency and transparency of its methodology. The NXY benchmark operates according to a rule-based methodology designed to ensure that benchmark values are derived through deterministic processes rather than discretionary decision-making.

All benchmark calculations follow the mathematical formula defined in the methodology. Observations submitted to the protocol are validated according to deterministic rules implemented within the smart contract architecture. These rules govern data validation, ordering constraints, timestamp verification, source-set checks, and publication sequencing.

Because the methodology is implemented through transparent smart contract logic and timelock-governed administrative controls, the calculation of the benchmark cannot be modified arbitrarily by any individual participant.

Historical benchmark publications remain immutable once recorded. Even if future governance decisions modify operational parameters or extend protocol functionality, previously published benchmark values cannot be altered retroactively within the blockchain record.

---

# 49. Benchmark Termination Policy

Institutional benchmark documentation must define procedures governing the potential discontinuation of the benchmark.

Termination of the benchmark would require an explicit governance decision executed through the protocol's timelock-controlled governance framework. Such a decision would involve a proposal describing the rationale for discontinuation and the operational steps required to suspend further benchmark publications.

If benchmark termination were approved through governance, the protocol would cease accepting new observations and no additional benchmark publications would be produced. However, the historical benchmark record would remain permanently preserved within the blockchain.

Because all previously published benchmark values are recorded as immutable blockchain transactions, termination would not affect historical data. Observers would retain full access to the complete benchmark history through the blockchain ledger and associated analytical tools.

---

# 50. Conclusion

The Naira Index establishes a blockchain-native benchmark infrastructure designed for programmable financial systems. It provides a transparent, deterministic, and publicly verifiable benchmark representing the Nigerian Naira within a global dollar benchmark context through DXY, using cNGN as the on-chain Naira reference layer.

Through its blockchain-native architecture, the protocol replaces traditional centralized benchmark administration with verifiable smart contract logic. Benchmark methodology, governance procedures, and publication processes are implemented through modular contracts deployed on BNB Chain, enabling independent verification of every benchmark observation.

The architecture separates responsibilities across multiple protocol components, including the core controller, oracle validation system, compliance enforcement layer, and publication management framework. This modular design improves operational resilience and ensures that no single component can unilaterally alter benchmark outcomes.

All benchmark publications are permanently recorded on-chain, providing a transparent and immutable historical record of benchmark values. Governance actions affecting the protocol are executed through a timelock-controlled framework that ensures changes to benchmark methodology are observable and subject to review before execution.

By combining deterministic index calculation with transparent governance and open-source implementation, the NXY protocol provides a credible and auditable benchmark infrastructure suitable for analytical, research, and reference purposes.

The benchmark does not operate as a trading venue, financial product, or market intervention mechanism. Instead, it functions as an observational index that derives a normalized benchmark level from DXY and the cNGN-based on-chain Naira reference layer. This design preserves benchmark neutrality while allowing external systems to integrate NXY as a macroeconomic reference indicator.

Through open-source publication, transparent governance procedures, and deterministic smart contract enforcement, the Naira Index establishes a new model for blockchain-native benchmark infrastructure.

---

End of Whitepaper
