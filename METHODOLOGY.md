# Naira Index (NXY) Methodology

Version: 1.0  
Date: March 2026  
Protocol: NXY Benchmark System  
Network: BNB Smart Chain  

------------------------------------------------------------

## 1. Overview

The Naira Index (NXY) is a blockchain-native benchmark designed to represent the relative strength of the Nigerian Naira within the global currency environment.

The benchmark references the Nigerian Naira relative to the global U.S. Dollar benchmark represented by the US Dollar Index (DXY).

The objective of the benchmark is to provide a transparent, deterministic, and reproducible macroeconomic reference indicator that can be validated through smart contract infrastructure and publicly verifiable data inputs.

The NXY benchmark is implemented as an on-chain protocol that records validated benchmark observations on the BNB Smart Chain.

------------------------------------------------------------

## 2. Definitions

**Benchmark**  
A rule-based index calculated according to a predefined methodology that provides a reference indicator for economic analysis.

**Benchmark Level**  
The numerical value of the NXY index produced for a given observation.

**Publication**  
A validated benchmark observation recorded on-chain by the NXY protocol.

**Valuation Timestamp**  
The time associated with the underlying economic data used to calculate the benchmark level.

**Oracle Signer**  
An authorized participant that submits benchmark observations to the protocol.

**Signer Class**  
A classification category for oracle signers used to enforce diversity within the validation process.

**Reference Asset**  
The on-chain representation of the Nigerian Naira used for validation within the protocol.

**Methodology Version**  
A version identifier associated with the benchmark methodology used to generate a benchmark observation.

------------------------------------------------------------

## 3. Benchmark Objective

The purpose of the NXY benchmark is to provide a standardized reference index representing the value dynamics of the Nigerian Naira relative to global dollar conditions.

The benchmark functions as a macroeconomic reference indicator rather than a financial product or trading instrument.

Potential analytical uses include:

- macroeconomic analysis  
- currency monitoring  
- benchmark research  
- financial modeling  
- programmable financial infrastructure

------------------------------------------------------------

## 4. Benchmark Formula

The NXY index is calculated using the following relationship:

NXY = DXY / (NGN per USD)

Where:

DXY = US Dollar Index level  
NGN per USD = Nigerian Naira exchange rate per U.S. Dollar  

This relationship produces a normalized benchmark value representing the relative strength of the Nigerian Naira within the global dollar benchmark environment.

------------------------------------------------------------

## 5. On-Chain Fixed-Point Representation

Within the smart contract implementation, benchmark values are represented using fixed-point integer arithmetic.

The protocol stores values using 18 decimal precision.

The primary data fields are:

dxyLevelE18  
cngnPerUsdE18  
nxyLevelE18

The benchmark value is computed as:

nxyLevelE18 = floor(dxyLevelE18 * 10^18 / cngnPerUsdE18)

Where:

dxyLevelE18 represents the DXY value scaled by 10^18  
cngnPerUsdE18 represents the NGN per USD value scaled by 10^18  

This representation ensures deterministic integer arithmetic within the blockchain execution environment.

------------------------------------------------------------

## 6. Data Inputs

The benchmark relies on two primary economic inputs.

US Dollar Index (DXY)

The DXY measures the strength of the U.S. Dollar relative to a basket of major global currencies including:

- Euro (EUR)  
- Japanese Yen (JPY)  
- British Pound (GBP)  
- Canadian Dollar (CAD)  
- Swedish Krona (SEK)  
- Swiss Franc (CHF)

NGN/USD Exchange Rate

The Nigerian Naira exchange rate relative to the U.S. Dollar.

Within the protocol architecture this relationship is referenced through the cNGN stablecoin representation of the Nigerian Naira.

------------------------------------------------------------

## 7. On-Chain Reference Asset

The NXY protocol uses the cNGN stablecoin as the on-chain reference layer representing the Nigerian Naira.

The reference asset is used for validation purposes within the smart contract system.

Reference asset verification ensures that the benchmark inputs correspond to the correct on-chain representation of the Nigerian currency.

------------------------------------------------------------

## 8. Oracle Validation

Benchmark observations are submitted by authorized oracle signers.

The oracle validation module enforces multiple safeguards including:

- signature verification  
- signer diversity validation  
- reference asset verification  
- timestamp validation  
- methodology parameter validation

Publications must include signatures from multiple signer classes to satisfy diversity requirements.

Observations that fail validation checks are rejected.

------------------------------------------------------------

## 9. Observation Timing Rules

Each benchmark publication contains a valuation timestamp.

Observations must satisfy the following timing constraints:

- valuation timestamp must not exceed the current block time  
- valuation timestamp must not exceed the maximum staleness tolerance  
- observations must be submitted in chronological order

These rules ensure that benchmark publications follow a consistent temporal sequence.

------------------------------------------------------------

## 10. Benchmark Publication

Validated observations are recorded on-chain by the publication manager module.

Each publication includes:

- index level  
- valuation timestamp  
- reference asset identifier  
- oracle signature metadata  
- methodology version identifier

Accepted publications become part of the immutable benchmark history stored on-chain.

------------------------------------------------------------

## 11. Benchmark Continuity Policy

In the event of abnormal data conditions, the protocol provides mechanisms to maintain benchmark continuity.

If a publication is determined to be invalid, governance may invalidate the affected observation.

Following invalidation, the protocol restores the most recent valid benchmark publication.

If oracle submissions are temporarily unavailable, the benchmark retains the last valid publication as the current reference value.

These mechanisms ensure continuity during periods of operational disruption.

------------------------------------------------------------

## 12. Benchmark Governance

Protocol governance operates through a timelock-controlled governance system.

Governance responsibilities include:

- approval or removal of oracle signers  
- approval of data source sets  
- methodology parameter updates  
- protocol configuration changes

Governance actions cannot arbitrarily generate benchmark values or bypass the oracle publication process.

------------------------------------------------------------

## 13. Methodology Versioning

The benchmark methodology is version controlled.

Each publication includes a methodology identifier corresponding to the methodology version used during calculation.

If methodology parameters are updated, governance records the new version identifier.

Historical publications remain associated with the methodology version under which they were produced.

------------------------------------------------------------

## 14. Benchmark Integrity Safeguards

The NXY protocol incorporates safeguards designed to protect benchmark integrity.

These include:

- deterministic benchmark calculation  
- separation of oracle and governance responsibilities  
- oracle signer diversity requirements  
- transparent governance procedures  
- immutable on-chain publication records

These mechanisms ensure that benchmark values are derived through rule-based processes rather than discretionary decisions.

------------------------------------------------------------

## 15. Transparency

The NXY protocol provides transparency through open infrastructure.

- Smart contract code is publicly accessible.  
- Benchmark observations are permanently recorded on-chain.  
- Governance actions are visible through blockchain transactions.  
- Methodology documentation is maintained in the public repository.

These features allow independent observers to reproduce and verify benchmark calculations.

------------------------------------------------------------

## 16. Benchmark Cessation Policy

Termination of the benchmark requires governance approval through the timelock governance framework.

If benchmark publication is discontinued, historical benchmark data remains permanently preserved on the blockchain.

Termination does not alter historical benchmark observations.

------------------------------------------------------------

## 17. Worked Example

Example benchmark calculation:

DXY = 98.95  
USD/NGN = 1401.40

NXY = 98.95 / 1401.40

NXY ≈ 0.0706

Example stronger Naira scenario:

DXY = 100  
USD/NGN = 1200

NXY = 100 / 1200

NXY = 0.0833

Example weaker Naira scenario:

DXY = 100  
USD/NGN = 1600

NXY = 100 / 1600

NXY = 0.0625

------------------------------------------------------------

## 18. Benchmark Calculation Unit and Display Convention

The NXY benchmark is a dimensionless ratio index.

Benchmark values represent the ratio between the global dollar benchmark environment and the Nigerian Naira exchange rate environment.

Within the smart contract implementation, benchmark values are stored using fixed-point integer representation with 18 decimal precision.

The canonical benchmark value is the integer representation recorded on-chain.

Displayed values may be rounded for presentation purposes in analytical systems, dashboards, or research publications. However, the authoritative benchmark value is always the fixed-point value stored by the protocol.

------------------------------------------------------------

## 19. Data Source Governance

The integrity of the benchmark depends on the reliability and diversity of the data sources used by oracle participants.

- Oracle signers are responsible for collecting benchmark inputs from approved data sources.

- Source sets used by oracle participants must be approved through protocol governance.

- Governance may authorize multiple independent source sets in order to reduce dependence on any single data provider.

- Source sets may be updated, expanded, or replaced through the governance timelock mechanism when necessary to preserve benchmark reliability.

This approach allows the protocol to maintain data resilience while ensuring that benchmark inputs remain transparent and verifiable.

------------------------------------------------------------

## 20. Benchmark Neutrality

The NXY protocol operates as an observational benchmark system.

The protocol does not execute foreign exchange trades, manage currency reserves, or intervene in currency markets.

Benchmark values are derived exclusively from external economic data inputs and deterministic calculation rules defined in this methodology.

Because the protocol does not influence exchange rates or market activity, the benchmark functions purely as a reference indicator describing the relationship between the Nigerian Naira and the global dollar benchmark environment.

------------------------------------------------------------

## 21. Methodology Change Control

Institutional benchmark governance requires controlled procedures for updating benchmark methodology.

Changes to the NXY methodology require approval through the protocol governance framework.

Governance proposals affecting the benchmark methodology must pass through the timelock-controlled governance process before they can be executed.

This delay ensures that methodology changes remain publicly observable prior to activation.

When methodology revisions occur, the protocol records a new methodology version identifier associated with subsequent benchmark publications.

Historical benchmark observations remain permanently associated with the methodology version under which they were produced.

This structure preserves transparency while allowing the benchmark methodology to evolve over time.

------------------------------------------------------------

## 22. Disclaimer

The NXY benchmark is a macroeconomic reference index and does not represent a financial product, security, or investment instrument.

The benchmark provides a normalized representation of the Nigerian Naira relative to the global dollar benchmark environment.

The index should be interpreted as an analytical reference indicator rather than a trading mechanism.

------------------------------------------------------------

End of Methodology
