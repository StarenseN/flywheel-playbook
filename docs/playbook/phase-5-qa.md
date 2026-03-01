---
icon: lucide/search-check
---

# Phase 5 -- QA the Beads

Run repeated review passes over the beads until revisions flatline. Check your beads N times, implement once.

---

## Beads QA Prompt (§4.2 QA the Beads) -- Repeat N Times

```
Reread AGENTS.md so it's still fresh in your mind.

We recently transformed a markdown plan into beads. I want you to very carefully
review and analyze these beads using bd and bv (robot flags only).

Check over each bead super carefully:

- does it make sense?
- is it optimally scoped?
- are dependencies correct?
- is any important work missing?
- are tests (unit + e2e) comprehensive enough, with detailed logging?
- are we missing security, performance, or ops beads?

DO NOT OVERSIMPLIFY THINGS.
DO NOT LOSE ANY FEATURES OR FUNCTIONALITY.

If improvements are needed, revise the beads accordingly using only bd.
```

## Recommended Pass Focus

| Pass | Focus |
|:-----|:------|
| 1 | **Completeness** -- are all plan items represented? |
| 2 | **Dependencies** -- are orderings correct? Missing links? |
| 3 | **Scope** -- are beads too large or too small? |
| 4 | **Tests** -- is coverage comprehensive with detailed logging? |
| 5 | **Security/ops** -- are there beads for threats, alerts, monitoring? |

Run 5-15 passes. The number is not arbitrary -- it's however many it takes until changes become reordering and renaming instead of finding missing work.

!!! warning
    The two most common failure modes are **oversimplifying** and **losing features**. Agents under-scoping beads during QA is the single most common failure mode. Watch for it.

!!! important
    **Stop condition:** Beads changes are mostly reordering or renaming, not missing work or wrong dependencies. The graph has converged.
