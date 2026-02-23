# Exohash Comprehensive Architecture Review

Generated: 2026-02-23

------------------------------------------------------------------------

## 1. Executive Summary

Exohash is a bespoke Cosmos SDK application centered around a
consensus-native threshold randomness beacon (`x/beacon`) and a
capital-enforced settlement engine (`x/house`). The protocol's defining
properties are:

-   Deterministic randomness inside consensus (ABCI++)
-   Protocol-level solvency enforcement
-   Low-latency probabilistic market execution

While the cryptographic and consensus architecture is sophisticated and
well-reasoned, the application layer exhibits monolithic coupling
(particularly inside `x/house`). To achieve mainnet-grade robustness,
the system requires strategic decoupling, explicit enforcement of
randomness validity rules, implementation of DKG slashing, and
aggressive state pruning.

------------------------------------------------------------------------

## 2. High-Level Architecture

### Consensus Layer

-   CometBFT
-   ABCI++ Vote Extensions
-   Deterministic finality

### Core Modules

  Module         Responsibility
  -------------- ----------------------------------------
  x/beacon       DKG + Threshold BLS randomness
  x/house        Escrow, reserve accounting, settlement
  x/exohgov      Custom governance
  x/valrewards   Validator USDC rewards

Target future split:

-   x/beacon → Randomness
-   x/house → Capital & solvency only
-   x/arena → Orchestration state machine
-   pkg/exogames → Stateless game engines

------------------------------------------------------------------------

## 3. x/beacon Analysis

### Cold Path (Epoch DKG)

Phases: 1. Commitments 2. Share exchange 3. Public key registration 4.
Finalization

Strengths: - Lagrange interpolation caching - Subgroup validation for
BLS - Committee rotation with bounded churn

### Hot Path (Per-block randomness)

-   ExtendVote() → BLS share
-   PrepareProposal() → aggregate + inject
-   ProcessProposal() → validation
-   PreBlocker() → randomness set or fallback

### Critical Architectural Issue

Currently, missing/invalid beacon outputs do not cause block rejection.
Instead, a fallback hash-chain randomness is used.

Risk: If markets consume randomness, proposers can indirectly influence
which randomness source is used unless degraded-mode or rejection rules
are enforced.

### Required Hardening

-   Define explicit validity rule when DKG active
-   Implement DKG slashing (missed commit, missed share, invalid share)
-   Add aggressive pruning of BeaconShares

------------------------------------------------------------------------

## 4. x/house Analysis

Current state: - Bankroll management - LP share accounting - Escrow and
reserve enforcement - Round orchestration - Embedded game engines

### Architectural Risk

Financial invariants and game logic are tightly coupled. This increases
audit surface and systemic risk.

### Required Refactor

Split into:

#### x/house (Financial Layer)

-   Deposits
-   Withdrawals
-   LP shares
-   Reserve enforcement
-   Settlement

#### x/arena (Orchestration Layer)

-   Round lifecycle
-   Match state transitions
-   Timeout handling
-   RNG binding

#### pkg/exogames (Stateless Engines)

-   Pure deterministic reducers
-   Input: config, entry state, RNG
-   Output: RoundOutcome
-   No keeper access

------------------------------------------------------------------------

## 5. Governance & Rewards

### x/exohgov

-   Custom governance parameters
-   Requires safety bounds and activation timelocks

### x/valrewards

-   USDC reward distribution
-   Requires strict reconciliation of module balances
-   Careful decimal math validation

------------------------------------------------------------------------

## 6. State Management Risks

### BeaconShares

Must prune aggressively (e.g., keep last 100 heights).

### Arena / Rounds

Move settled rounds to historical prefix and prune after retention
window.

### Hard Safety Bounds

Committee size must be compile-time bounded.

------------------------------------------------------------------------

## 7. Architectural Principles

1.  Minimize consensus-critical surface
2.  Minimize capital-critical surface
3.  Engines must be stateless
4.  Randomness validity must be explicit
5.  Governance must not bypass hard-coded safety bounds

------------------------------------------------------------------------

## 8. Conclusion

The protocol core is sophisticated and promising. The primary work
remaining is architectural hardening, decoupling, and enforcement of
economic security rules before mainnet deployment.
