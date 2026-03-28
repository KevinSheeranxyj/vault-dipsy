## Vault-dipsy

A practical solidity-baed **Multi-sign Treasury** project for secure asset management.
This project is designed as a real-world learning and portfolio-ready implementation of multi-signature treasury wallet, with a strong focus on:

- secure fund costody
- multi-party approval
- transaction lifecycle management
- extensible arthitecture
- testablility and auditability
## Table of Contents

- [Introduction](#introduction)
- [Goals](#goals)
- [Core Features](#core-features)
- [Architecture](#architecture)
- [Architecture Diagram](#architecture-diagram)
- [Transaction Lifecycle](#transaction-lifecycle)
- [Permission Model](#permission-model)
- [Project Structure](#project-structure)
- [Frameworks and Tools](#frameworks-and-tools)
- [Quick Start](#quick-start)
- [Main Contract Responsibilities](#main-contract-responsibilities)
- [Function Overview](#function-overview)
- [Security Considerations](#security-considerations)
- [Testing Strategy](#testing-strategy)
- [Future Improvements](#future-improvements)
- [Interview-Ready Summary](#interview-ready-summary)
- [License](#license)

## Introduction


## Goals

The main goals of this project are:

- eliminate single-point control over treasury assets
- require multi-party approval for critical operations
- make transaction history transparent and auditable
- provide a clean architecture that is easy to extend
- simulate real-world protocol governance patterns
- demonstrate security-first Solidity development

## Core Features

- configurable signer set
- configurable approval threshold
- transaction proposal system
- signer confirmation and revocation flow
- transaction cancellation support
- execution of ETH transfers and arbitrary contract calls
- self-governed signer and threshold management
- complete event emission for indexing and monitoring
- test-ready architecture with mock targets and attack simulations


### 1. Signer Management Layer

This layer is responsible for governance participants.

**Responsibilities:**
- add signer
- remove signer
- replace signer
- update threshold

**Design principle:**  
Signer structure changes should not be controlled by a privileged EOA.  
They should only be executable through the treasury itself after multisig approval.

---

### 2. Transaction Proposal Layer

This layer manages pending treasury actions.

Each transaction proposal contains:
- target contract or recipient
- ETH value
- calldata
- execution status
- cancellation status
- approval count
- optional timelock metadata

Example:

```solidity
struct Transaction {
    address to;
    uint256 value;
    bytes data;
    bool executed;
    bool cancelled;
    uint256 confirmationCount;
    uint256 submittedAt;
    uint256 eta;
}
```


## Flow
```text
Submit
  |
  v
Pending -----> Cancelled
  |
  v
Confirmed Enough
  |
  +-----> Queued -----> Executed
  |
  +-----> Executed
```


## Project Structure
```text
vault-dipsy/
├── src/
│   ├── MultiSigTreasury.sol
│   ├── interfaces/
│   │   └── IMultiSigTreasury.sol
│   ├── libraries/
│   │   ├── Errors.sol
│   │   └── Events.sol
│   └── mocks/
│       ├── MockTarget.sol
│       └── ReentrantTarget.sol
├── test/
│   ├── MultiSigTreasury.t.sol
│   ├── MultiSigTreasuryFuzz.t.sol
│   └── helpers/
│       └── TestUtils.sol
├── script/
│   └── DeployMultiSig.s.sol
├── lib/
├── foundry.toml
└── README.md
```