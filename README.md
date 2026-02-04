# Exohash Protocol

![License](https://img.shields.io/badge/license-MIT-blue)
![Status](https://img.shields.io/badge/status-devnet-orange)
![Consensus](https://img.shields.io/badge/consensus-CometBFT-green)

**The Sovereign Probability Engine.**

Exohash is a specialized Layer-1 blockchain built for high-frequency, provably fair probabilistic markets. It moves the "House" from a private server to a protocol module, enforcing solvency, fairness, and settlement via deterministic state machines.

## 📚 Repository Overview

This repository contains the **Protocol Interfaces (Protobuf Definitions)** for the Exohash network. It serves as the canonical source of truth for:
1.  **Client Integration:** Frontend SDKs, Indexers, and Wallets.
2.  **State Inspection:** Understanding the data structures of the Bankroll, Beacon, and Engine modules.
3.  **Governance:** Parameter definitions for the DAO.

> **Note:** The Core Node implementation (Go) is currently in **Private Devnet**. It will be open-sourced following the completion of our primary audit cycle.

---

## 🏗 Architecture

Exohash is built on the Cosmos SDK but replaces standard modules with a custom purpose-built stack.

### 1. Entropy Layer (`x/beacon`)
* **Package:** `exohash.beacon.v1`
* **Mechanism:** Synchronous BLS Threshold Signatures (BLS-12-381).
* **Function:** Produces unbiasable, distributed randomness in every block header. Unlike VRFs, this is native to the consensus layer, allowing for immediate settlement execution.

### 2. Liquidity Layer (`x/house`)
* **Package:** `exohash.house.v1`
* **Function:** The "House" is a protocol module, not a wallet.
* **Solvency Predicate:** Every incoming transaction is checked against the module's reserves. If `MaxPossiblePayout > (Reserves * RiskCap)`, the transaction is atomically rejected.
* **Settlement:** Holds funds in escrow during the randomness reveal phase.

### 3. Execution Layer (`x/engines`)
* **Package:** `exohash.engines.v1`
* **Function:** A registry of deterministic state reducers (Game Engines).
* **Logic:** Engines define the rules, odds, and state transitions. They do not hold funds; they emit payout instructions which `x/house` executes.

### 4. Incentive Layer (`x/valrewards`, `x/exohrewards`)
* **Function:** Deterministic fee routing.
* **Flow:** Protocol fees (Yield) are collected in USDC and routed programmatically to Validators (for securing the beacon) and Stakers (for governance).

---

## 📦 Directory Structure

The Protobuf files are organized by module and version to ensure compatibility.

```bash
proto/exohash/
├── beacon/              # Distributed Randomness Beacon
│   └── v1/
│       ├── dkg.proto    # Key Generation & Share Exchange
│       └── tx.proto     # Beacon Transactions
├── house/               # Solvency & Settlement Module
│   └── v1/
│       ├── bankroll.proto
│       └── bet.proto
├── engines/             # Game Logic Registry
│   └── v1/
│       └── params.proto
└── exohrewards/         # Fee Distribution Logic
    └── v1/
```

---

## 🛠 Integration

### Using with Buf
We strictly follow [Buf](https://buf.build/) standards.

1. **Linting:**
   ```bash
   buf lint
   ```

2. **Generating Code:**
   If you are building a Go client or Indexer, you can generate the types:
   ```bash
   buf generate
   ```

### Querying the Devnet
(Endpoints will be populated as public RPC nodes come online)

* **Public RPC:** `https://rpc.devnet.exohash.io` (Coming Soon)
* **Public gRPC:** `https://grpc.devnet.exohash.io` (Coming Soon)

---

## ⚠️ Disclaimer

**Exohash is currently in Devnet.**
The `.proto` definitions in this repository are subject to breaking changes as we optimize the storage layout and gas consumption of the execution engines.

Copyright © 2026 Exohash Labs.
