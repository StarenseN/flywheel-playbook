---
icon: lucide/sparkles
---

# Phase 3 -- Alien Artifacts

Inject theoretically optimal constructs that standard engineering would never produce.

---

> "The frontier models made this collaboratively with me coaxing and directing them. This is a new breed of software. This is an Alien Artifact."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026614142598033732))

A named, scheduled phase. Not optional. Not opportunistic.

Take the locked plan to a model strong at formal reasoning and mathematics. Tell it to add alien artifacts -- BOCPD, VOI, conformal prediction, e-processes. Constructs too advanced for standard engineering practice but possibly exactly right for your problem.

Think of it as: a brilliant but alien mathematician reviews your plan and suggests theoretically optimal constructs. Elegant, correct, and nothing a standard engineer would think to add. The output goes into a new version of the plan.

## The Alien Artifact Prompt (PL-13)

```
You are a brilliant mathematician and theoretical computer scientist reviewing
a software engineering plan. Your job is to identify places where mathematically
sophisticated constructs would be strictly superior to the standard engineering
approaches currently specified.

Look for opportunities to inject:
- Bayesian Online Changepoint Detection (BOCPD) for regime shifts
- Value of Information (VOI) for decision-making under uncertainty
- Conformal prediction for distribution-free confidence intervals
- E-processes / anytime-valid sequential testing
- Information-theoretic bounds for compression or communication
- Optimal stopping theory for resource allocation
- Concentration inequalities for tail bounds

For each suggestion:
1. What it replaces in the current plan
2. Why the mathematical construct is strictly better
3. The specific algorithm or formula to implement
4. Edge cases where it degrades gracefully to the naive approach

Be precise. Provide equations where relevant. Do not suggest constructs that
add complexity without measurable benefit.

<PASTE THE COMPLETE PLAN HERE>
```

## How to Run This Phase

1. Take the converged plan from Phase 2
2. Fire the prompt above at a model strong in formal reasoning (o3 or equivalent)
3. Review the suggestions -- reject anything that adds complexity without clear benefit
4. Integrate accepted artifacts into the plan
5. Version the output explicitly (e.g., V1.5 = "alien-artifact discipline")
6. After this, the plan is locked. It does not change unless you explicitly unlock it.

## What Good Artifacts Look Like

From frankensqlite:

- **BOCPD for write-pattern regime detection** -- replaced a fixed-interval heuristic for page compaction triggers. The BOCPD version adapts to actual workload shifts in real time.
- **Conformal prediction for query latency SLOs** -- replaced hard-coded percentile thresholds with distribution-free confidence intervals that tighten as data accumulates.
- **VOI-guided test selection** -- replaced uniform random test sampling with information-gain-maximized selection during QA passes.

These survived every subsequent review unchanged. That is the signal. If an alien artifact gets rewritten or removed in later phases, it was not the right construct.

!!! tip
    In frankensqlite, Version 1.5 was literally named "alien-artifact discipline" and added mathematically sophisticated constructs that survived every subsequent review unchanged. This phase is what pushes plans beyond standard engineering.

!!! important
    **Stop condition:** The alien artifact pass is complete and the plan version is incremented.
