# Exohash Protocol Invariants

Generated: 2026-02-23

------------------------------------------------------------------------

## Randomness Invariants

1.  When DKG is active, randomness for height H must derive from
    threshold BLS aggregation.
2.  Fallback randomness may only be used in explicit degraded mode.
3.  Proposer must not be able to choose between randomness sources.

------------------------------------------------------------------------

## Solvency Invariants

1.  Reserved ≥ MaxPayout at all times.
2.  Paid ≤ Reserved.
3.  Escrow balance must never become negative.
4.  Withdrawals must respect reserve caps.

------------------------------------------------------------------------

## Governance Invariants

1.  Critical parameters have hard upper/lower bounds.
2.  Activation occurs at epoch boundary + timelock.
3.  Committee size cannot exceed compile-time limit.

------------------------------------------------------------------------

## State Safety Invariants

1.  Historical data must be pruned deterministically.
2.  Per-height cryptographic artifacts must not accumulate indefinitely.
3.  Archive prefixes must be separate from working state.
