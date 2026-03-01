---
icon: lucide/gauge
---

# Phase 8 -- Performance

Profile-driven optimization as a scheduled phase, not reactive debugging.

---

This is not reactive. It is a scheduled bead.

Create beads for profiling: `bd-proj.flamegraph` and `bd-proj.heaptrack`. Assign them like any other work. The agents profile, analyze, write documents.

## Deep Performance Audit

For serious performance work, use the full methodology:

```
First read ALL of AGENTS.md and README.md super carefully.
Then fully understand the code, architecture, and purpose of the project.

Once you've deeply understood the entire system, investigate:
- Are there gross inefficiencies in the core system?
- Where would changes actually move the needle on latency/throughput?
- Where can we prove changes are functionally isomorphic (same outputs)?
- Where is there a clearly better algorithm or data structure?

Consider: N+1 elimination, zero-copy, buffer reuse, scatter-gather I/O,
serialization costs, bounded queues + backpressure, striped locks, memoization,
dynamic programming, lazy evaluation, streaming/chunked processing,
pre-computation, index-based lookup, binary search, two-pointer, prefix sums.

METHODOLOGY:
A) Baseline first: run tests + representative workload, record p50/p95/p99
B) Profile before proposing: capture CPU + allocation + I/O profiles
C) Equivalence oracle: define explicit golden outputs + invariants
D) Isomorphism proof per change: short proof sketch why outputs cannot change
E) Opportunity matrix: rank by (Impact x Confidence) / Effort before implementing
F) Minimal diffs: one performance lever per change, no unrelated refactors
G) Regression guardrails: add benchmark thresholds or monitoring hooks
```

!!! tip
    The methodology requirements prevent "optimization theater." Profile before proposing. Prove correctness before shipping. One lever per diff.

!!! important
    **Stop condition:** All profiling beads complete. No hotspot above threshold.
