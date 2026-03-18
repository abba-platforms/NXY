# Versioning Policy

The Naira Index (NXY) repository follows a structured versioning framework based on semantic versioning principles.

Version format:

MAJOR.MINOR.PATCH

------------------------------------------------------------

## Version Components

MAJOR

Incremented when there are significant changes to the benchmark architecture, protocol design, or methodology that may affect compatibility or interpretation of the index.

MINOR

Incremented when new features, documentation sections, or protocol capabilities are introduced without altering the core benchmark methodology.

PATCH

Incremented when minor corrections, documentation updates, or non-breaking improvements are made.

------------------------------------------------------------

## Version v1.0.0

The current version, v1.0.0, represents the initial public release of the Naira Index (NXY) protocol documentation and architecture.

This release includes:

Benchmark definition and methodology  
Protocol architecture documentation  
Governance framework  
Economic analysis  
Contracts documentation  
Repository structure  

------------------------------------------------------------

## Versioning Scope

Versioning applies to:

Whitepaper  
Methodology  
Architecture documentation  
Governance framework  
Protocol design  

Smart contract deployments may follow separate versioning depending on upgrade cycles.

------------------------------------------------------------

## Governance and Version Updates

All major and minor version changes are expected to be reflected through protocol governance where applicable.

Version updates are recorded in:

CHANGELOG.md  
Git tags  
Repository documentation  

------------------------------------------------------------

## Methodology Version Binding

Each benchmark observation produced by the NXY protocol is associated with a specific methodology version.

The methodology version defines the parameters and calculation logic under which the index value was derived.

This ensures that all historical benchmark publications remain reproducible and independently verifiable.

Any changes to the benchmark formula, validation rules, or data interpretation must result in a new methodology version.

------------------------------------------------------------

## Backward Compatibility Policy

The NXY benchmark maintains strict separation between historical and current methodology versions.

Updates to the benchmark methodology do not modify or restate previously published index values.

Historical observations remain permanently associated with the methodology version under which they were produced.

This ensures continuity, reproducibility, and auditability of the benchmark over time.

------------------------------------------------------------

## Release Classification

Version updates may correspond to different categories of protocol changes.

These include:

Documentation Releases  
Changes affecting whitepaper, methodology documentation, or repository structure without modifying protocol logic.

Protocol Releases  
Changes affecting benchmark logic, validation rules, governance parameters, or operational behavior.

Smart Contract Releases  
Changes affecting deployed contract logic, upgradeable modules, or on-chain enforcement mechanisms.

Each release type is reflected in the versioning system and recorded in the changelog.

------------------------------------------------------------

## Smart Contract Versioning

Smart contract deployments and upgrades may follow a distinct versioning lifecycle from repository documentation.

Where upgradeable contracts are used, implementation versions may be tracked separately from repository versions.

All contract upgrades are subject to governance procedures and timelock enforcement.

The deployed contract state on-chain represents the authoritative execution layer of the protocol.

------------------------------------------------------------

## On-Chain Version Identification

The NXY protocol may include version identifiers stored or referenced within smart contract state.

These identifiers allow external observers to determine:

The active protocol version  
The methodology version applied to benchmark observations  
The version of contract logic governing the system  

This enables independent verification of protocol behavior directly from blockchain data.

------------------------------------------------------------
