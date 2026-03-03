---
icon: lucide/file-text
---

## 2.3 Writing the Plan (Phases 1-3)

### 2.3.1 Start With a Short Prompt

Your first planning prompt should be 2-5 sentences describing **what** you want to build, not **how**. Resist the urge to enumerate features, specify implementation details, or "help" the model avoid tangents. Those constraints come later, during refinement rounds.

> *"I see that you start the planning process with very short prompts. When I build plans I am tempted to describe features I want, or 'help' the model avoid tangents, so my prompt bloats out to two pages of text. Should I resist that early and refine the constraints later?"*
> — ACFS community member
>
> *"Yeah, resist the temptation early on."*
> — Community response

Use a web interface with extended thinking enabled (ChatGPT Pro or Claude web app). The web interface is ideal for initial planning because extended reasoning tokens are cheap in subscription mode, and the model can think for several minutes before producing output.

---

### 2.3.2 The First Draft: Letting the Model Think Big

The first draft will be large and imperfect. That is expected. The FrankenSQLite initial spec dump was 8,628 lines at 01:17 AM; by 03:50 AM it had been revised through six major versions.

Do not constrain the first draft. Let the model go wide. It will include features you don't need, edge cases you hadn't considered, and architectural choices you may disagree with. All of that is raw material for the refinement phases. Pruning is easier than inventing.

The first output is a starting point, not a deliverable. If it were good enough to execute directly, you wouldn't need the methodology.

---

### 2.3.3 Iterating: Diff-Based Revisions, Not Rewrites

After the first draft, refinement happens through a series of structured passes. The critical discipline: revisions are **diff-based edits to the existing document**, never full rewrites.

> *"Figure out what we should do and then start making a series of small edits to the spec document in a way that is integrated, coherent, cohesive, and perfectly harmonized with the existing spec doc."*
> — Jeffrey Emanuel

Requiring diffs (rather than rewrites) prevents two failure modes: vague advice ("consider adding error handling") and context destruction (rewriting a section loses the reasoning embedded in the current version).

The recommended revision cadence:

| Round | Focus |
|-------|-------|
| 1-3 | Praise rounds: push the model past its default quality ceiling |
| 4 | Architecture, data model, component boundaries |
| 5 | Security, failure modes, edge cases |
| 6 | Performance, scalability, operational concerns |
| 7 | Testing depth, observability, developer experience |
| 8 | Multi-model competition (see 2.3.4) |
| 9 | Final pass: clarity, consistency, agent-readability |

The praise rounds (rounds 1-3) deserve special mention. These are short, escalating prompts that push the model past its initial "good enough" output:

- Round 1: *"That's a decent start but it barely scratches the surface and is light years away from being OPTIMAL. Please try again and revise your existing plan document in-place to make it MUCH, MUCH, MUCH better in EVERY WAY."*
- Round 2: *"I believe in you, you can do this! Show me how brilliant you really are!"*
- Round 3: *"Give me your ABSOLUTE BEST work. This is your chance to show the world what frontier AI can produce."*

These are not motivational fluff. Models respond to emotional register. A prompt that signals high stakes produces more careful, thorough output than one that accepts "decent."

---

### 2.3.4 Multi-Model Competition

After the praise rounds produce a solid draft, send the plan to 2-3 competing frontier models. Ask each for two deliverables:

1. A **revised plan** incorporating their improvements.
2. A **narrative document** arguing for their changes and against the original.

Then feed each model's output to the others. Ask them to evaluate honestly: what did the competitor get right? What did they get wrong?

> *"When I have Codex & Claude go back and forth over a plan I ask them for two documents: its revised plan AND narrative doc arguing its position."*
> — Jeffrey Emanuel

The competition prompt (PL-07 in the master prompt set):

> *"I asked 3 competing LLMs to do the exact same thing and they came up with pretty different plans which you can read below. I want you to REALLY carefully analyze their plans with an open mind and be intellectually honest about what they did that's better than your plan."*

This phase is not optional for serious projects. Single-model plans have single-model blind spots. The plan that survives multi-model scrutiny is significantly more robust than any single model's output.

> *"Just having different models review and check each others plans and beads has been very helpful for me."*
> — ACFS community member

---

### 2.3.5 Plan QA: The Checklist Before You Move On

Before converting the plan to beads, verify it against this checklist:

- [ ] **Goals** are measurable, user-facing outcomes
- [ ] **Non-goals** are explicit (scope boundaries prevent creep)
- [ ] **Architecture** defines components, boundaries, invariants, and data flow
- [ ] **Threat model** includes a realistic attacker model with mitigations
- [ ] **Secrets** — where stored, how injected, never in logs
- [ ] **Failure modes** define retries, timeouts, backoff, and idempotency rules
- [ ] **Performance** includes explicit SLOs with concrete numbers
- [ ] **Observability** — structured logs, metrics, traces, alerts
- [ ] **Testing** covers unit, integration, and end-to-end, with fixture strategies
- [ ] **Rollout** includes feature flags, migration steps, and rollback procedures
- [ ] **Risk register** lists minimum 7 risks with severity and mitigation
- [ ] **Recovery Runbook** — actual recovery procedures for each failure mode

Then run a premortem:

> *"Before we proceed, I want you to do a 'premortem' on this plan. Imagine we're 6 months in the future and this approach has completely failed. What went wrong? What assumptions did we make that turned out to be false?"*

The stop condition for planning: you are seeing only minor wording tweaks and no meaningful architectural, testing, or operational improvements. The plan has converged.

> *"Check over each bead super carefully; are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? It's a lot easier and faster to operate in 'plan space' before we start implementing these things!"*
> — Jeffrey Emanuel

---

### 2.3.6 Version-Controlling the Plan

Every meaningful revision of the plan is a git commit. Tag major versions. You will want to diff v3 against v5 later.

The FrankenSQLite plan went through 26 named versions in a single overnight session:

```
01:17 AM  Initial spec dump, 8,628 lines
02:45 AM  V1.3 — scope doctrine + ECS substrate
03:14 AM  V1.4 — Codex synthesis pass
03:16 AM  V1.5 — alien-artifact discipline (Claude math pass)
03:31 AM  V1.6a through V1.6h (8 sub-versions in 20 minutes)
11:07 AM  V1.7 — section-by-section deep audit
11:50 AM  V1.7a through V1.7j (10 more sub-versions)
```

After each project, an `ANALYSIS_OF_SPEC_DOC_DIFFS.md` is generated: a systematic review classifying every change in the spec's history (logic fix, architectural fix, alien artifact, clarification, noise). This post-mortem trains intuition for future planning sessions.

> *"It's a multi-stage process. Look at the git history for that plan and you'll see all the earlier incarnations."*
> — Jeffrey Emanuel

---

### 2.3.7 The ACFS Wizard (Automated Planning)

Jeff has automated the planning methodology into a tool called the ACFS Wizard. It enforces quality gates, alternates models, and tracks state via file existence.

**Track Selection:**
- **Track A** (50-100 beads): Linear flow, single plan author. Use for small-medium projects.
- **Track B** (300+ beads): Competing proposals from 4 AIs, then synthesized. Use for large projects.

**Quality Gates (non-negotiable):**

| Gate | After | Pass Criteria |
|:-----|:------|:-------------|
| G1 | Project Meta | Size justified, constraints specific, 1+ real risk |
| G2 | Problem Brief | Problem < 5 sentences, MVP 3-5 items, 3+ out-of-scope |
| G3 | Plan v1 | All 13+ sections substantive, no TBD/placeholders |
| G4 | Plan v2 | 5+ substantive changes from v1, SA-10 score >= 10/16 |
| G4.5 | Plan v3 | No BLOCKING/CRITICAL issues from solidification |
| G5 | Verification | "Flat" state reached, each pass used different AI |

**The SA-10 Quality Rubric:** Plans are scored 0-16. Thresholds: 14-16 = proceed, 10-13 = proceed with notes, 6-9 = fix first, 0-5 = major revision.

**Solidification Passes:** After the main review, 4 specialist passes run in parallel (each by a different AI): Implementation feasibility, Operations readiness, Security audit, Dependencies check.

**State Detection:** The wizard uses file existence as state. No database:
```
No 00_PROJECT_META.md    → create project meta
No 03_PLAN_v1.md         → draft the plan
No plan_quality_review.md → run SA-10 scoring
```

The wizard is available at the [ACFS Wizard repo](https://github.com/StarenseN/ACFS_Wizard_Openclaw). Use `/start` to begin, `/dashboard` for multi-model feedback.

---

!!! tip "Related pages on this site"

    - [Phase 1: Draft](../playbook/phase-1-draft.md)
    - [Phase 2: Refine](../playbook/phase-2-refine.md)
    - [Phase 3: Alien Artifacts](../playbook/phase-3-alien-artifacts.md)
    - [Jeff Teaches](../jeff-teaches.md) — Jeff's methodology in his own words
    - [Prompt Dispatch Table](../reference/dispatch.md)

