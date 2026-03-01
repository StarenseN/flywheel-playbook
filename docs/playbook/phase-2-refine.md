---
icon: lucide/iteration-cw
---

# Phase 2 -- Refine the Plan

Iterate through critique rounds until improvements become incremental. This is where 85% of the time goes.

---

## The Praise Rounds (first 3 rounds)

Push the model past its default posture. These look ridiculous. They work. The model is not a static function. It responds to the emotional register of the conversation.

**Round 1 -- §2.1 Praise Push I:**
```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

**Round 2 -- §2.2 Praise Push II:**
```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

**Round 3 -- §2.3 Praise Push III:**
```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

## Single-Model Critique (Round Type A -- §2.4)

After the praise rounds, switch to analytical mode:

```
Carefully review this entire plan for me and come up with your best revisions in terms of:

- better architecture
- missing or improved features
- reliability and failure handling
- security and privacy
- performance and scalability
- clarity and implementability for coding agents
- testing depth (unit + e2e)
- operational robustness (observability, alerts, rollbacks)

For each proposed change:

1. Give a detailed analysis and rationale/justification.
2. Provide git-diff style changes relative to the original markdown plan shown below.

<PASTE THE COMPLETE PLAN HERE>
```

## Multi-Model Competition (Round Type B -- §2.5)

Fire competing models simultaneously -- this is a parallel compute job, not a conversation.

```
I asked 3 competing LLMs to do the exact same thing and they came up with pretty
different plans which you can read below. I want you to REALLY carefully analyze
their plans with an open mind and be intellectually honest about what they did
that's better than your plan. Then I want you to come up with the best possible
revisions to your plan that artfully and skillfully blends the "best of all worlds"
to create a true, ultimate, superior hybrid version of the plan that best achieves
our stated goals; you should provide me with a complete series of git-diff style
changes to your original plan to turn it into the new, enhanced, much longer and
detailed plan that integrates the best of all the plans with every good idea included.

Current plan: <PASTE CURRENT PLAN HERE>
Competing outputs: <PASTE OTHER MODEL OUTPUTS HERE>
```

## Integrating Feedback

```
Read AGENTS.md and keep all tool rules in mind.

Now integrate the following review feedback into <PLAN_FILE_PATH> in-place.
Be meticulous: keep the plan cohesive, consistent, and remove contradictions.

At the end, list:
- changes you strongly agree with
- changes you somewhat agree with
- changes you disagree with (and why)

<PASTE THE COMPLETE REVIEW OUTPUT HERE>
```

## Premortem (before you call it done)

```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

## Recommended Cadence

| Round | Focus |
|:------|:------|
| 1-3 | Praise rounds (push past default quality) |
| 4 | Architecture, data model, component boundaries |
| 5 | Security, failure modes, edge cases |
| 6 | Performance, scalability, operational concerns |
| 7 | Testing depth, observability, developer experience |
| 8 | Multi-model competition + synthesis |
| 9 | Final pass: clarity, consistency, agent-operability |

## Risk Register and Recovery Runbook

The plan also needs:

- A **Risk Register** -- seven risks minimum: what breaks first, what's hardest to test, what requires the most dangerous code. Severity, status, mitigation.
- A **Recovery Runbook** -- if the system gets into a bad state (corrupted DB, sync failure, mid-migration crash), how do we get it back? Write this while you still understand the design.

## Plan QA Checklist

Every plan must pass this before converting to beads:

- [ ] **Goals** -- measurable, user-facing outcomes
- [ ] **Non-goals** -- explicit scope boundaries to prevent creep
- [ ] **Architecture** -- components, boundaries, invariants, data flow
- [ ] **Threat model** -- realistic attacker model + mitigations
- [ ] **Secrets** -- where they live, how they're injected, what never enters logs
- [ ] **Failure modes** -- retries, timeouts, backoff, idempotency rules
- [ ] **Performance** -- explicit SLOs with concrete numbers, measurement plan
- [ ] **Observability** -- structured logs, metrics, traces, alert thresholds
- [ ] **Testing** -- unit + integration + e2e, fixtures, logging, "how we debug failures"
- [ ] **Rollout** -- feature flags, migrations, rollback steps
- [ ] **Risk Register** -- minimum 7 risks with severity and mitigation
- [ ] **Recovery Runbook** -- actual recovery procedures, not just failure identification

!!! important
    **Stop condition:** You're seeing only minor wording tweaks and no meaningful architectural, testing, or operational improvements. The plan has converged.
