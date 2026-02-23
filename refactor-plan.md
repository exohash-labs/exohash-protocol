# Exohash Refactor Plan

Generated: 2026-02-23

------------------------------------------------------------------------

## Phase 0 -- Invariants & Documentation

-   Create docs/invariants.md
-   Create docs/refactor-log.md
-   Add integration tests for DKG lifecycle
-   Define degraded-mode policy

------------------------------------------------------------------------

## Phase 1 -- Beacon Hardening

-   Enforce beacon validity rule
-   Implement DKG slashing hooks
-   Distinguish benign absence vs malicious invalid shares
-   Add EndBlocker pruning for BeaconShares

------------------------------------------------------------------------

## Phase 2 -- Module Decoupling

-   Extract engines → pkg/exogames
-   Create x/arena
-   Reduce x/house to financial core
-   Implement ExpectedKeepers interfaces

------------------------------------------------------------------------

## Phase 3 -- Mempool & Crypto Safeguards

-   Rate limit DKG share messages
-   Enforce max committee size at AnteHandler
-   Improve VerifyVoteExtension differentiation logic

------------------------------------------------------------------------

## Phase 4 -- State Pruning & Optimization

-   Prune historical rounds
-   Prune old DKG sessions
-   Run accelerated block simulation for 100k blocks

------------------------------------------------------------------------

## Phase 5 -- Governance & Economic Audit

-   Review parameter bounds
-   Add timelocks for critical changes
-   Audit decimal math & reward distribution

------------------------------------------------------------------------

## Final Stage -- Public Testnet & External Audit

-   Deploy modular architecture
-   Engage Cosmos audit firm
-   Focus review on:
    -   LagrangeCache
    -   Solvency invariants
    -   Beacon enforcement rules
