# The Flywheel Planning Playbook — Definitive Edition

*How to build software with AI agent swarms. Everything. No fluff.*

*Based on Jeffrey Emanuel's methodology. Synthesized from 5 repos, 69 sessions, 8,914 tweets, and the Flywheel Planning Playbook by @voidserf. This is the version Jeff would write if he sat down and put it all in one place.*

---

## The Premise

Most people using AI agents are doing it wrong. Not slightly wrong. Catastrophically wrong. They open Claude, describe what they want, accept whatever comes back, and move on. Then they wonder why the code is brittle, the architecture is incoherent, and they spent three days debugging something an agent wrote in four minutes.

Here is the fundamental insight: **planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous.**

The effort split:

| Activity | How most people work | How this works |
|----------|---------------------|----------------|
| Planning | ~10% | ~85% |
| Implementation | ~70% | ~10% |
| Review/Fix | ~20% | ~5% |

That's not a typo. Eighty-five percent planning. A 6-hour planning session prevents 60 hours of agent chaos. A bad plan wastes multi-agent hours across parallel workers. A good plan pays for itself before the first line of code.

And there's a deeper flywheel most people miss: **each project you build becomes a library for the next project.** The tools compound. FrankenTUI feeds FrankenSQLite feeds FrankenTerm feeds Agent Mail Rust. The February 2026 Rust Agent Mail port stacks 10 self-built libraries. That compounding — each project consuming previous projects and becoming infrastructure for the next — is the actual flywheel. The planning methodology is just the engine that makes each rotation clean enough to build on.

---

## The 12 Principles

These are the ideas behind the workflow. Understand these first; the phases are just the implementation.

### 1. Planning is leverage

High-quality planning compresses implementation time, reduces rework, and improves architecture and UX before code exists. This is not "writing docs before coding." This is recognizing that the plan IS the product — code is just the plan's compiled form.

### 2. Iterative refinement beats single-shot

No single pass — no matter how good the model — catches everything. Repeated critique cycles reliably improve robustness, testability, and clarity. Four to five refinement rounds is the floor before diminishing returns; on a serious project, total revision count including multi-model passes, alien artifacts, and post-merge alignment will hit 8-15.

### 3. Diff-based revisions are the key mechanic

Requiring git-diff style edits prevents vague advice and forces concrete, integratable changes. When a model says "consider adding error handling," that is worth nothing. When it produces a diff that adds error handling to a specific function with specific retry logic, that is worth everything. Always demand diffs against the current plan.

### 4. Three spaces, not two

Most people think there are two spaces: plan space and code space. Wrong. There are three:

1. **Plan space** — where the architecture lives (PLAN.md)
2. **Bead-map space** — where the execution graph lives (master-todo-bead-map.md)
3. **Code space** — where the implementation lives (src/)

The plan is read-once. After beads are created, PLAN.md is closed. Agents read the bead-map during execution, never the plan. The bead-map is the bridge artifact that connects design intent to executable work.

### 5. Use the right model for the job

Match task complexity to model capability:

| Task | Best model | Why |
|------|-----------|-----|
| High-volume deterministic work | GPT-5.2 / Codex (xhigh-thinking) | Reliable, fast, cheap |
| Creative/strategic reasoning | Claude Opus | Best architectural judgment |
| Long plan critiques | Extended reasoning via web app | Cheap relative to code tokens |
| Formal reasoning / alien artifacts | o3 or equivalent | Mathematical depth |

But: **do not specialize agents.** Diversify models; don't constrain roles. Every agent is a fungible generalist. The bead system handles coordination, not role descriptions. The single exception is the dedicated commit agent (P11), which never edits code.

### 6. Beads are an execution graph, not a todo list

You're converting narrative intent into granular tasks with explicit dependencies so agents can coordinate safely. Each bead has: scope, acceptance criteria, context, rationale, dependencies, constraints. An agent working on a bead should not need to constantly refer back to the plan.

### 7. Check your beads N times, implement once

The asymmetry: revising a bead takes seconds; fixing a wrong implementation takes hours. Repeated beads review catches missing tasks, wrong dependencies, unclear acceptance criteria, and missing test coverage. Run 5-15 QA passes until changes are mostly reordering and renaming, not discovering missing work.

### 8. Avoid communication purgatory

Coordination is valuable, but progress requires agents to pick actionable beads and ship. If an agent is spending more time coordinating than coding, something is wrong. Be proactive about starting tasks; inform fellow agents when you start work, don't ask for permission. Mark beads appropriately so others can see what's claimed.

### 9. Fresh-eyes review loops work

After agents implement, run repeated "reread what you wrote and fix obvious issues" loops until changes flatline. Each pass catches things the previous one missed. The first pass catches obvious bugs. The second catches architectural inconsistencies. The third catches edge cases. By the fourth or fifth, you're getting clean results. Run these as numbered sessions — Part 1, Part 2, all the way to Part 23 if needed.

### 10. Alien artifacts are a scheduled discipline

Before converting the plan to beads, deliberately inject mathematically advanced constructs — BOCPD, VOI, conformal prediction, e-processes. This is a named phase, not an ad-hoc addition. Take the plan to a model strong at formal reasoning and tell it to add "alien artifacts" — theoretically optimal constructs that a brilliant but alien mathematician might suggest. This is what pushes plans beyond standard engineering into something genuinely novel.

### 11. Every word in a prompt is a constraint

Short prompts that force the model beyond its default posture beat long prompts that enumerate everything it should think about. This is counterintuitive. Models are not search engines with a personality; they are collaborators. They respond to how you frame things. They respond to stakes. They respond to praise. Use that.

### 12. Session hygiene is sacred

Your orchestration session is for orchestration only. Reviews happen in separate side sessions. If you mix planning commands with review output, the orchestrator's context gets polluted and it loses track of where you are in the workflow. One session, one purpose. Spawn dedicated sessions for review, debugging, and exploration.

---

## The Phases

Nine phases. Sequential. Do not skip phases or begin implementation before plan stabilization. Each phase has a stop condition — an explicit signal that tells you when to advance.

---

### Phase 0 — Prerequisites

**Goal:** Environment is ready for agent-driven development.

Before you start planning, make sure the scaffolding is in place:

- [ ] Your repo has an `AGENTS.md` explaining tool rules, model selection, safety constraints, and coordination rules
- [ ] Beads is initialized (`bd` or `br`) for the project
- [ ] You've decided where your canonical plan lives (e.g., `PLAN.md` in repo root)
- [ ] Agent coordination tooling is available (Agent Mail, if using multi-agent)
- [ ] You have access to a strong reasoning model for plan critique (web app with extended thinking)

**AGENTS.md is the project constitution.** Not documentation. An operating manual for minds that will wake up without memory of this conversation. It tells them exactly what this project is, what tools exist, what the bead workflow looks like, what they must never do, and where to find things. I write this myself or heavily direct it. This is not optional.

The mandatory section is the Bead Execution Protocol:

```
## Bead Execution Protocol (MANDATORY)
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Implement it completely
3. Before closing the bead: read over ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
4. Only then: `br close <id>`
5. Communicate to fellow agents via Agent Mail
6. Pick next bead. Repeat.
Do NOT commit (the commit agent handles this).
Do NOT stop between beads. Keep the loop going.
```

**Stop condition:** All checklist items completed.

---

### Phase 1 — Draft the Plan

**Goal:** Produce a single, detailed markdown plan that is agent-operable — not just human-readable.

Use a web app with strong reasoning enabled (ChatGPT Pro with extended thinking, or Claude with extended thinking). The web interface is ideal because extended reasoning is cheap relative to code generation tokens.

A human-readable plan says "implement authentication." An agent-operable plan says "implement JWT-based authentication using RS256, with refresh token rotation, 15-minute access token TTL, stored in httpOnly cookies, with a `/auth/refresh` endpoint that validates the refresh token and issues a new pair."

The prompt:

```
You are a senior architect and staff engineer. We are starting a greenfield project:

Project: <PROJECT_NAME>
Users: <TARGET_USERS>
Problem: <PROBLEM_STATEMENT>
Constraints: <CONSTRAINTS>
Environment: <LANGUAGE/STACK/PLATFORM>

Write a single, extremely detailed markdown plan we can implement with coding agents.

Requirements:
- Start with Goals / Non-goals.
- Include a clear architecture section: components, boundaries, invariants, data flow.
- Include data model / schemas (as needed).
- Include security & privacy model (threats, mitigations, secrets handling).
- Include performance targets (with concrete numbers) and an instrumentation plan.
- Include error handling, retries, and "no silent fallback" philosophy.
- Include a test plan: unit + integration + e2e (with detailed logging expectations).
- Include rollout plan: feature flags, migrations, backwards compatibility (if relevant).
- Include a task breakdown section that could be converted into a dependency graph.

Be explicit and operational. Avoid vague advice.
Assume this plan will be executed by multiple parallel agents.
```

Save the result as your canonical plan file. The first output is always bad. That's not a failure, that's a starting point.

**Stop condition:** You have a comprehensive first draft.

---

### Phase 2 — Refine the Plan

**Goal:** Iterate through critique rounds until improvements become incremental.

This is where 85% of the time goes. Not writing code. Not debugging. Reading plans, attacking them, improving them.

#### The Praise Rounds (first 3 rounds)

Push the model past its default posture. These look ridiculous. They work.

**Round 1 — P30:**
```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

**Round 2 — P31:**
```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

**Round 3 — P32:**
```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

The model is not a static function. It responds to the emotional register of the conversation. Push it.

#### Single-Model Critique (Round Type A — P04)

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

#### Multi-Model Competition (Round Type B — P15)

Now send the plan to competing models. Fire them simultaneously — this is a parallel compute job, not a conversation.

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

Current plan:
<PASTE CURRENT PLAN HERE>

Competing outputs:
<PASTE OTHER MODEL OUTPUTS HERE>
```

#### Integrating Feedback

After each critique, apply it:

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

#### Premortem (before you call it done)

Before advancing, do one premortem pass:

```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

#### Recommended Cadence

| Round | Focus |
|-------|-------|
| 1-3 | Praise rounds (push past default quality) |
| 4 | Architecture, data model, component boundaries |
| 5 | Security, failure modes, edge cases |
| 6 | Performance, scalability, operational concerns |
| 7 | Testing depth, observability, developer experience |
| 8 | Multi-model competition + synthesis |
| 9 | Final pass: clarity, consistency, agent-operability |

The plan also needs:
- A **Risk Register** — seven risks minimum, severity, mitigation
- A **Recovery Runbook** — if the system gets into a bad state, how do we get it back? Write this while you still understand the design

#### Plan QA Checklist (audit before advancing)

Every plan must pass this before converting to beads:

- [ ] **Goals** — measurable, user-facing outcomes
- [ ] **Non-goals** — explicit scope boundaries to prevent creep
- [ ] **Architecture** — components, boundaries, invariants, data flow
- [ ] **Threat model** — realistic attacker model + mitigations
- [ ] **Secrets** — where they live, how they're injected, what never enters logs
- [ ] **Failure modes** — retries, timeouts, backoff, idempotency rules
- [ ] **Performance** — explicit SLOs with concrete numbers, measurement plan
- [ ] **Observability** — structured logs, metrics, traces, alert thresholds
- [ ] **Testing** — unit + integration + e2e, fixtures, logging, "how we debug failures"
- [ ] **Rollout** — feature flags, migrations, rollback steps
- [ ] **Risk Register** — minimum 7 risks with severity and mitigation
- [ ] **Recovery Runbook** — actual recovery procedures, not just failure identification

**Stop condition:** You're seeing only minor wording tweaks and no meaningful architectural, testing, or operational improvements. The plan has converged.

---

### Phase 3 — Alien Artifacts

**Goal:** Inject theoretically optimal constructs that standard engineering would never produce.

This is a named, scheduled phase. Not optional. Not opportunistic.

Take the locked plan to a model that is particularly strong at formal reasoning and mathematics. Tell it to add alien artifacts — BOCPD, VOI, conformal prediction, e-processes. Things that are too advanced for standard engineering practice but that might be exactly right for your problem.

The output goes into a new version of the plan. Version the output explicitly (e.g., V1.5 = "alien-artifact discipline"). After this, the plan is locked. It does not change unless you explicitly unlock it.

**Stop condition:** The alien artifact pass is complete and the plan version is incremented.

---

### Phase 4 — Convert Plan to Beads

**Goal:** Transform the stable plan into a granular execution graph with epics, tasks, subtasks, and explicit dependencies.

The prompt (P05):

```
Reread AGENTS.md so it's fresh in your mind.

Now read ALL of <PLAN_FILE_PATH>.

Please take ALL of that and elaborate on it more and then create a comprehensive
and granular set of beads for all this with:

- epics + tasks + subtasks (as needed)
- dependency structure overlaid (blocks/related/parent-child/discovered-from)
- detailed comments so the beads are self-contained and self-documenting

Include relevant background, reasoning/justification, constraints, and acceptance
criteria so we never need to refer back to <PLAN_FILE_PATH>.

Also include beads for:

- comprehensive unit tests (meaningful coverage, not shallow mocks)
- e2e/integration scripts with great, detailed logging
- observability/alerts for silent-failure risk areas

Use only the bd tool to create and modify beads and add dependencies. Be exhaustive.
```

Each bead must include: clear scope, acceptance criteria, context and rationale, dependencies (blocks/related/parent-child), constraints. Don't forget beads for: tests, observability, performance profiling, security checks.

Then create the **master-todo-bead-map.md**:

```
## Section 3: Rendering Engine
- [ ] Core diffing algorithm → bd-10i.3.1
- [ ] PackedRgba blending → bd-10i.3.2
- [ ] CellAttrs + style flags → bd-10i.3.3
```

This exists so agents never reopen the plan. The plan is read-once. The map is the execution surface.

**Stop condition:** All plan sections have been converted to beads with dependencies. No plan content is left unrepresented.

---

### Phase 5 — QA the Beads

**Goal:** Run repeated review passes over the beads until revisions flatline.

This is "check your beads N times, implement once." Each pass catches different problems.

The prompt (P06):

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

**Critical guardrails:** The two most common failure modes are oversimplifying and losing features. Agents under-scoping beads during QA is the most common failure mode. Watch for it.

| Pass | Focus |
|------|-------|
| 1 | Completeness — are all plan items represented? |
| 2 | Dependencies — are orderings correct? Missing links? |
| 3 | Scope — are beads too large or too small? |
| 4 | Tests — is coverage comprehensive with detailed logging? |
| 5 | Security/ops — are there beads for threats, alerts, monitoring? |

Run 5-15 passes. The number is not arbitrary — it's however many it takes until changes become reordering and renaming instead of finding missing work.

**Stop condition:** Beads changes are mostly reordering or renaming, not missing work or wrong dependencies. The graph has converged.

---

### Phase 6 — Implement with Agent Swarm

**Goal:** Deploy coordinated agents to execute beads systematically.

The prompt for each new coding agent (P08):

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

That's it. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication.

For fresh agents or agents after context compaction (P18):

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads, working on the tasks
systematically and meticulously and tracking your progress via beads and agent mail
messages. Don't get stuck in "communication purgatory" where nothing is getting done;
be proactive about starting tasks that need to be done, but inform your fellow agents
via messages when you do so and mark beads appropriately.
```

When agents hit context compaction (P20):

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

Simple. The agent just lost half its context window. Give it the constitution back before it makes any decisions.

#### Agent Coordination

- **Claim before starting** — mark beads `in_progress` before working
- **Communicate blockers and discoveries**, not just status
- **Group commits by bead/feature slice**, not by time
- **Bead IDs in commit messages are the coordination surface** — agents read git messages to know what's done. Agent Mail is for higher-level coordination.
- **Agent failure is normal operations** — Gemini agents will hit shell restrictions. Log it, handle it gracefully, move on. Don't panic over expected failure modes.

#### The Commit Agent

One agent is different: the commit agent. It runs only P11:

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

It does not modify code. It reads the diff, groups changes into logical commits, writes detailed messages with bead IDs, and commits. That is its entire job.

#### Typical Formation

| Agent type | Count | Role |
|---|---|---|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only |

Scale based on bead graph size and budget. All implementation agents are fungible generalists. Don't specialize.

**Stop condition:** All beads are implemented and marked complete.

---

### Phase 7 — Fresh-Eyes Review

**Goal:** Repeatedly review implemented code until review passes produce no substantive diffs.

This is where most people drop the ball. They do one review pass and call it done. I run numbered fix sessions.

#### Self-Review (P02b — after each bead)

```
Great. Now carefully read over all of the new code you just wrote and any
existing code you modified.

With fresh eyes, look for:

- obvious bugs
- incorrect edge cases
- mismatched types / error handling gaps
- unclear naming / confusing flows
- missing tests
- silent failure modes
- performance footguns

Carefully fix anything you uncover. Be meticulous.
```

This runs after every bead, not just at the end. It's baked into the bead execution protocol.

#### Deep Review (P02 — numbered sessions)

```
I want you to sort of randomly explore the code files in this project, choosing
code files to deeply investigate and understand and trace their functionality and
execution flows through the related code files which they import or which they are
imported by. Once you understand the purpose of the code in the larger context of
the workflows, I want you to do a super careful, methodical, and critical check
with "fresh eyes" to find any obvious bugs, problems, errors, issues, silly
mistakes, etc. and then systematically and meticulously and intelligently correct
them. Be sure to comply with ALL rules in AGENTS.md and ensure that any code you
write or revise conforms to the best practice guides referenced in AGENTS.md.
Use ultrathink.
```

Notice what this does. It does not tell the model what to look for. It asks the model to explore, understand, and apply judgment. Every prescriptive bug checklist is a tunnel that narrows the model's attention. P02 keeps it wide.

Run this as Part 1, Part 2, Part 3... Part 23 if needed. Each session gets a sequential ID.

#### Cross-Agent Review (P03)

```
Now review code written by other agents across the project (not just the
latest commit). Look for:

- bugs, edge cases, and inconsistencies
- security/privacy issues
- reliability issues (retries, timeouts, idempotency)
- performance regressions
- unclear or brittle architecture
- missing or low-quality tests

Diagnose root causes using first-principles reasoning and then fix issues you find.
Don't restrict yourself to the latest commits — cast a wider net and go super deep!
```

Run this with a different model than the one that wrote the code for true "fresh eyes."

#### Escalation Prompts (when reviews plateau)

When the model seems too comfortable, switch to P27 (McCarthy):
```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

When you need the model to care, add P28 (Stakes Escalation):
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

These change the model's internal prior. By default, models are optimistic reviewers. P27 and P28 make them paranoid. Sometimes you need paranoid.

#### Stub Elimination (run before final review)

```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

Agents leave stubs during initial passes. This catches them all.

**Stop condition:** Repeated fresh-eyes passes produce no substantive diffs. The code has converged.

---

### Phase 8 — Performance

**Goal:** Profile-driven optimization as a scheduled phase, not reactive debugging.

This is not reactive. It is a scheduled bead.

Create beads for profiling: `bd-proj.flamegraph` and `bd-proj.heaptrack`. Assign them like any other work. The agents profile, analyze, write documents.

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

The methodology requirements prevent "optimization theater." Profile before proposing. Prove correctness before shipping. One lever per diff.

**Stop condition:** All profiling beads complete. No hotspot above threshold.

---

## The Complete Prompt Catalog

41 prompts organized by when you use them. Verbatim. Copy-paste only. Do not paraphrase.

### Planning Prompts

**P01 — First Principles** *(prefix for everything)*
```
Before acting, pause and think through this from first principles. What assumptions
might be wrong? What edge cases exist? What could fail? Consider multiple approaches
and their tradeoffs. Only proceed when you've thoroughly analyzed the problem space.
```

**P30 — Praise Round 1**
```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

**P31 — Praise Round 2**
```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

**P32 — Praise Round 3**
```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

**P04 — Plan Critique (Round Type A)**
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

**P15 — Multi-Model Synthesis (Round Type B)**
```
I asked 3 competing LLMs to do the exact same thing and they came up with pretty
different plans which you can read below. I want you to REALLY carefully analyze
their plans with an open mind and be intellectually honest about what they did
that's better than your plan. Then I want you to come up with the best possible
revisions to your plan that artfully and skillfully blends the "best of all worlds"
to create a true, ultimate, superior hybrid version of the plan that best achieves
our stated goals and will work the best in real-world practice to solve the problems
we are facing and our overarching goals while ensuring the extreme success of the
enterprise as best as possible; you should provide me with a complete series of
git-diff style changes to your original plan to turn it into the new, enhanced,
much longer and detailed plan that integrates the best of all the plans with every
good idea included.

Current plan: <PASTE CURRENT PLAN HERE>
Competing outputs: <PASTE OTHER MODEL OUTPUTS HERE>
```

**P17 — Dueling Wizards** *(two-model competitive scoring)*
```
I want two different frontier models to each independently propose their best
version of this. Score each proposal on: correctness, elegance, robustness,
completeness, and novelty. Then synthesize the winner into the plan.
```

**P26 — Plan Innovation Boost**
```
What is the single smartest addition we could make to this plan that would
dramatically improve the project? Not incremental. Transformative.
Use ultrathink.
```

**Premortem Planner** *(pre-beads risk check)*
```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

**Project Opinion Elicitor** *(honest project assessment)*
```
Now tell me what you actually THINK of the project -- is it even a good idea?
Is it useful? Is it well designed and architected? Pragmatic? What could we do
to make it more useful and compelling and intuitive/user-friendly to both humans
AND to AI coding agents?
```

### Bead Prompts

**P05 — Plan to Beads**
```
Reread AGENTS.md so it's fresh in your mind.
Now read ALL of <PLAN_FILE_PATH>.

Please take ALL of that and elaborate on it more and then create a comprehensive
and granular set of beads for all this with:
- epics + tasks + subtasks (as needed)
- dependency structure overlaid (blocks/related/parent-child/discovered-from)
- detailed comments so the beads are self-contained and self-documenting

Include relevant background, reasoning/justification, constraints, and acceptance
criteria so we never need to refer back to <PLAN_FILE_PATH>.

Also include beads for:
- comprehensive unit tests (meaningful coverage, not shallow mocks)
- e2e/integration scripts with great, detailed logging
- observability/alerts for silent-failure risk areas

Use only the bd tool to create and modify beads and add dependencies. Be exhaustive.
```

**P06 — QA the Beads** *(repeat N times)*
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

**P24 — Use BV** *(bead prioritization)*
```
Use bv --robot-triage to identify the highest-impact actionable beads.
Pick the best one you can usefully work on and get started. Use ultrathink.
```

### Execution Prompts

**P08 — Execute Beads** *(main loop)*
```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

**P09 — Agent Mail Check & Continue** *(anti-deadlock)*
```
Check your agent mail and promptly respond if needed to any messages. Then proceed
meticulously with your next assigned beads. Don't get stuck in "communication
purgatory" where nothing is getting done; be proactive about starting tasks that
need to be done. Use ultrathink.
```

**P18 — Agent Introduction** *(fresh spawn)*
```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads. Don't get stuck in
"communication purgatory." Use ultrathink.
```

**P20 — Post-Compaction Refresh**
```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

**P25 — Do All Of It** *(full autonomous push)*
```
I need you to do ALL of the remaining work. Every bead. Every test. Leave nothing
undone. Be thorough, meticulous, and autonomous. Use ultrathink.
```

**P11 — Git Commit** *(commit agent only)*
```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

### Review Prompts

**P02 — Deep Review / Fresh Eyes Bug Hunt**
```
I want you to sort of randomly explore the code files in this project, choosing
code files to deeply investigate and understand and trace their functionality and
execution flows through the related code files which they import or which they are
imported by. Once you understand the purpose of the code in the larger context of
the workflows, I want you to do a super careful, methodical, and critical check
with "fresh eyes" to find any obvious bugs, problems, errors, issues, silly
mistakes, etc. and then systematically and meticulously and intelligently correct
them. Be sure to comply with ALL rules in AGENTS.md and ensure that any code you
write or revise conforms to the best practice guides referenced in AGENTS.md.
Use ultrathink.
```

**P02b — Post-Implementation Self-Review**
```
Great. Now carefully read over all of the new code you just wrote and any
existing code you modified.

With fresh eyes, look for:
- obvious bugs
- incorrect edge cases
- mismatched types / error handling gaps
- unclear naming / confusing flows
- missing tests
- silent failure modes
- performance footguns

Carefully fix anything you uncover. Be meticulous.
```

**P03 — Cross-Agent Review**
```
Now review code written by other agents across the project (not just the
latest commit). Look for:
- bugs, edge cases, and inconsistencies
- security/privacy issues
- reliability issues (retries, timeouts, idempotency)
- performance regressions
- unclear or brittle architecture
- missing or low-quality tests

Diagnose root causes using first-principles reasoning and then fix issues you find.
Don't restrict yourself to the latest commits — cast a wider net and go super deep!
```

**P27 — McCarthy Bug Hunt** *(escalation)*
```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

**P28 — Stakes Escalation** *(escalation)*
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

**P29 — CVE-Inspired Security Testing**
```
Research recent CVEs relevant to the libraries and patterns in this project.
Create sandboxed tests that probe for similar vulnerabilities. Use ultrathink.
```

### Ideation Prompts

**P16 — Idea Wizard (30→5)**
```
Come up with your very best ideas for improving this project to make it more
robust, reliable, performant, intuitive, user-friendly, ergonomic, useful,
compelling, etc. while still being obviously accretive and pragmatic. Come up
with 30 ideas and then really think through each idea carefully... winnow that
list down to your VERY best 5 ideas. Explain each of the 5 ideas in order from
best to worst. Use ultrathink.
```

**100-to-10 Filter** *(when quality matters most)*
```
I want you to come up with your top 10 most brilliant ideas for adding extremely
powerful and cool functionality that will make this system far more compelling,
useful, intuitive, versatile, powerful, robust, reliable, etc. Be pragmatic and
don't think of features that will be extremely hard to implement or which aren't
necessarily worth the additional complexity burden they would introduce. But I
don't want you to just think of 10 ideas: I want you to seriously think hard and
come up with one HUNDRED ideas and then only tell me your 10 VERY BEST and most
brilliant, clever, and radically innovative and powerful ideas.
```

**P14 — System Weaknesses Analyzer**
```
Based on everything you've seen, what are the weakest/worst parts of the system?
What is most needing of fresh ideas and innovative/creative/clever improvements?
```

### Quality Prompts

**P12 — Stripe-Level UI**
```
I want you to do a spectacular job building absolutely world-class UI/UX
components, with an intense focus on making the most visually appealing,
user-friendly, intuitive, slick, polished, "Stripe level" of quality UI/UX
possible for this that leverages the good libraries that are already part of
the project. Carefully consider desktop UI/UX and mobile UI/UX separately and
hyper-optimize for both. Use ultrathink.
```

**P13 — E2E Pipeline Validator**
```
We really need totally complete, totally comprehensive, granular, perfect end
to end testing coverage without ANY mocks or fake data, fake api calls, etc.,
that proves our entire pipeline from start to finish works perfectly in a
provable, ultra-rigorous way — from "soup to nuts." Plus comprehensive unit
tests with detailed logging. Use ultrathink.
```

**Stub Eliminator** *(hunt all placeholders)*
```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

**P19 — Audit UX**
```
Scrutinize the UX of the entire project. Find every rough edge, confusing flow,
and unintuitive behavior. Fix them all. Use ultrathink.
```

**P22 — Fix Bug** *(root-cause only)*
```
Find the root cause of this bug. Don't patch symptoms. Understand why it happened,
fix the underlying issue, and verify the fix doesn't break anything else.
Use ultrathink.
```

**P23 — Random Inspect** *(codebase exploration)*
```
Pick 5 random files in the project you haven't looked at recently. Read them
carefully. Trace their execution flows. Find anything wrong. Fix it.
Use ultrathink.
```

### Tool-Building Prompts

**CLI Error Tolerance** *(agent-friendly CLIs)*
```
One thing that's critical for the robot mode flags in the CLI is that we want to
make it easy for the agents to use the tool. First, make the CLI interface as
intuitive and easy as possible and explain it super clearly in the CLI help and
in AGENTS.md. Beyond that, be maximally flexible when the intent of a command is
clear but there's some minor syntax issue; honor all commands where the intent is
legible (with a note instructing the agent how to correctly issue it in the future).
If we can't figure out what the agent is trying to do, return a super detailed and
helpful error message with a couple relevant correct examples about how to do what
we think they're trying to do.
```

### Documentation Prompts

**P10 — Deep Project Primer** *(read everything first)*
```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
```

**README Reviser** *(present-tense docs)*
```
Update the README and other documentation to reflect all of the recent changes
to the project. Frame all updates as if they were always present (i.e., don't
say "we added X" or "X is now Y" — just describe the current state). Make sure
to add any new commands, options, or features that have been added.
```

**De-Slopifier** *(remove AI writing tells)*
```
Read through the complete text carefully and look for any telltale signs of
"AI slop" style writing; one big tell is the use of em dash. Replace with a
semicolon, a comma, or just recast the sentence. Also avoid: "It's not [just]
XYZ, it's ABC" or "Here's why" or "Here's why it matters:". Anything that sounds
like the kind of thing an LLM would write disproportionately more commonly than
a human. You MUST manually read each line and revise it — no regex, no scripts.
```

**Code Reorganizer** *(file restructuring)*
```
Before making any changes, explore and read ALL of the many files in <DIR> and
understand what they do, how they fit together, which files import which others,
how they interact. Then propose a reorganization plan in a new document called
PROPOSED_CODE_FILE_REORGANIZATION_PLAN.md so I can review it before doing anything.
This plan should include your detailed reorganization plan, the super-detailed
rationale and justification for the proposed structure, and tracking of all import
changes needed so we don't break anything.
```

**Deployment Verifier** *(post-deploy check)*
```
Deploy to <PLATFORM> and verify that the deployment worked properly without any
errors (iterate and fix if there were errors). Then visit the live site with
playwright as both desktop and mobile browser and take screenshots and check for
js errors and look at the screenshots for potential problems and iterate and fix
them all super carefully!
```

### Integration Prompts (from forensics)

**P21 — UBS Scan**
```
Run ubs . to scan the entire codebase. Analyze the results carefully and identify
any issues, improvements, or areas needing attention. Use ultrathink.
```

**Dependency Analysis** *(pre-integration due diligence)*
```
Before integrating <DEPENDENCY>, write a COMPREHENSIVE_ANALYSIS_OF_<DEPENDENCY>.md.
Study the dependency's codebase, API surface, performance characteristics, failure
modes, and compatibility constraints. This must be done BEFORE any integration code.
```

---

## Stop Conditions (Quick Reference)

| Phase | Advancement Signal |
|-------|-------------------|
| 0 — Prerequisites | All checklist items complete |
| 1 — Draft the Plan | Comprehensive first draft finished |
| 2 — Refine the Plan | Only minor wording tweaks; no structural improvements |
| 3 — Alien Artifacts | Artifact pass complete; plan version incremented |
| 4 — Convert to Beads | All plan sections represented as beads with dependencies |
| 5 — QA the Beads | Bead changes are only reordering/renaming, not missing work |
| 6 — Implement | All beads implemented and marked complete |
| 7 — Fresh-Eyes Review | Review passes produce no substantive diffs |
| 8 — Performance | All profiling beads complete; no hotspot above threshold |

---

## Anti-Patterns (Things That Will Destroy Your Project)

1. **Skeleton-first development.** "Just get something working, then improve it." No. By the time you have something working, the architecture is already compromised and you'll spend 10x longer fixing it than you would have spent planning it right.

2. **Specialized agents.** "You are an expert senior backend engineer with 15 years of experience in..." Stop. Frontier models understand intent. Role-playing constraints narrow the solution space. Use fungible generalists.

3. **Communication purgatory.** Agents messaging each other endlessly without shipping code. The bead system and AGENTS.md solve this architecturally. If it keeps happening, your AGENTS.md is unclear.

4. **Separate design docs.** The plan is the design doc. There is one canonical plan. Not a plan plus a design doc plus an architecture doc. One document, one source of truth.

5. **Skipping the praise rounds.** "I'll just go straight to analytical critique." The praise rounds push the model past its default comfort zone. Analytical critique refines; praise rounds expand.

6. **Mocked tests.** "We need to have totally complete, totally comprehensive, granular, perfect end to end testing coverage without ANY mocks or fake data, fake api calls, etc." Tests with mocks prove that mocks work, not that code works.

7. **Reopening the plan during execution.** After beads exist, PLAN.md is closed. The bead-map is the execution surface. If an agent re-reads the plan during P08, something went wrong in bead conversion.

8. **Skipping the premortem.** If you can't imagine how your project fails, you haven't thought about it hard enough. The premortem pass catches things optimistic planning never will.

9. **One review pass.** Part 1 is not enough. Part 2 catches what Part 1 missed. Part 5 catches what Part 4 missed. Part 23 still finds things. Keep going until the diffs flatline.

10. **Optimization theater.** Proposing performance improvements without profiling first. The Deep Performance Audit methodology exists because most "optimizations" are guesses that don't move the needle.

11. **Git worktrees for agent swarms.** Agents in separate worktrees never see each other's changes until merge time, by which point conflicts have piled up into a nightmare. All agents in one shared working directory surfaces merge conflicts immediately, when they're small and easy to resolve.

12. **Sending execution prompts to fresh agents.** Fire EX-01 at an agent that just spawned, and it goes rogue. It doesn't know the bead protocol, hasn't read AGENTS.md, doesn't understand file reservations or Agent Mail. EX-03 (Agent Introduction) is always the first prompt. No exceptions.

13. **Assuming prompts were sent.** You paste a prompt into a terminal pane and assume the agent received it. Roughly half the time, terminal paste silently fails. Always verify delivery: check that a new entry appeared in the session log within 10 seconds.

14. **Interrupting ultrathink.** An agent has been "thinking" for 12 minutes and you panic-hit Ctrl+C. Those 12 minutes of deep reasoning are now gone. If session logs show tokens incrementing, the agent is working. Leave it alone.

---

## The Toolchain

### Core Tools

| Tool | What it does | Why it matters |
|------|-------------|----------------|
| **Beads** (bd/br) | Dependency-aware task graph | The execution surface for all agent work |
| **BV** (Beads Viewer) | DAG analysis, PageRank prioritization, `--robot-next` | Tells agents what to work on next |
| **Agent Mail** | Gmail-like inter-agent messaging | Async coordination without blocking |
| **NTM** (Named Tmux Manager) | Agent cockpit, 80+ commands | Spawns, monitors, and manages agent sessions |
| **UBS** (Ultimate Bug Scanner) | 1000+ detection rules, AST-grep, 8 languages | Catches issues linting misses |
| **CASS** (Coding Agent Session Search) | 11 agent formats, Tantivy-powered <60ms | Find what agents did across sessions |
| **SLB** (Simultaneous Launch Button) | Nuclear-launch-style safety, 4 risk tiers | Prevents accidental mass operations |
| **DCG** (Destructive Command Guard) | SIMD-accelerated safety net, 50+ packs | Catches dangerous commands before execution |

### Supporting Tools

| Tool | What it does |
|------|-------------|
| **CM** (CASS Memory) | Three-layer cognitive architecture for agents |
| **CAAM** (Account Manager) | Sub-100ms auth switching between AI accounts |
| **RU** (Repo Updater) | Multi-repo sync with AI-driven commits |
| **MS** (Meta Skill) | Skill management with UCB bandit optimization |
| **RCH** (Remote Compilation Helper) | Transparent build offloading to remote workers |
| **WA** (WezTerm Automata) | Terminal hypervisor for multi-agent automation |
| **JFP** (JeffreysPrompts CLI) | Browse/install prompts as Claude Code skills |
| **SRPS** (System Resource Protection) | Auto-deprioritize background processes |
| **APR** (Automated Plan Reviser) | Iterative spec refinement with extended reasoning |
| **PT** (Process Triage) | Bayesian zombie process detection |
| **XF** (X Archive Search) | BM25 + semantic search over Twitter archives |
| **TRU** (TOON Rust) | Token-optimized notation, 30-50% reduction |
| **MDWB** (Markdown Web Browser) | Convert websites to markdown for LLM consumption |
| **S2P** (Source to Prompt TUI) | Interactive code-to-prompt generator |
| **CAUT** (Usage Tracker) | Track LLM provider usage and costs |

### Setup

Transform a fresh Ubuntu VPS into a complete agentic environment in 30 minutes:

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh?$(date +%s)" | bash -s -- --yes --mode vibe
```

Idempotent — if interrupted, re-running resumes from last completed phase.

Monthly cost: VPS ($40-56) + Claude Max ($200) + ChatGPT Pro ($200) = ~$440-656.

---

## Prompt Dispatch Table

*When you're in situation X, use prompt Y.*

| Situation | Prompt | Phase |
|-----------|--------|-------|
| Starting from scratch | P01 (First Principles) | 1 |
| First draft of plan exists | P30-P32 (Praise Rounds) | 2 |
| Plan needs analytical critique | P04 (Plan Critique) | 2 |
| Multiple models produced competing plans | P15 (Multi-Model Synthesis) | 2 |
| Two models should compete on a problem | P17 (Dueling Wizards) | 2 |
| Need a single transformative addition | P26 (Plan Innovation Boost) | 2 |
| Need honest project assessment | Project Opinion Elicitor | 2 |
| Integrating critique feedback | Integrate Critique prompt | 2 |
| Ready to stress-test plan against failure | Premortem Planner | 2 |
| Converting plan to beads | P05 (Plan to Beads) | 4 |
| Reviewing bead quality | P06 (QA the Beads) | 5 |
| Picking next bead to work on | P24 (Use BV) | 6 |
| Spawning a fresh agent | P18 (Agent Introduction) | 6 |
| Ongoing bead execution | P08 (Execute Beads) | 6 |
| Agent mail check + continue | P09 (Anti-deadlock) | 6 |
| After context compaction | P20 (Refresh) | 6 |
| Full autonomous push | P25 (Do All Of It) | 6 |
| Committing code (commit agent only) | P11 (Git Commit) | 6 |
| After completing a bead | P02b (Self-Review) | 7 |
| Numbered review session | P02 (Deep Review) | 7 |
| Reviewing other agents' code | P03 (Cross-Agent Review) | 7 |
| Reviews seem too comfortable | P27 (McCarthy Bug Hunt) | 7 |
| Need the model to care more | P28 (Stakes Escalation) | 7 |
| Security-focused review | P29 (CVE Testing) | 7 |
| Hunting stubs and placeholders | Stub Eliminator | 7 |
| UI/UX polish | P12 (Stripe-Level UI) | 7 |
| Comprehensive testing | P13 (E2E Pipeline Validator) | 7 |
| UX audit | P19 (Audit UX) | 7 |
| Random codebase exploration | P23 (Random Inspect) | 7 |
| Root-cause bug fixing | P22 (Fix Bug) | 7 |
| Full codebase scan | P21 (UBS Scan) | 7 |
| Post-deploy verification | Deployment Verifier | 7 |
| Performance optimization | Deep Performance Audit | 8 |
| Generating improvement ideas | P16 (Idea Wizard 30→5) | Any |
| Need exceptional ideas only | 100-to-10 Filter | Any |
| Finding system weaknesses | P14 (System Weaknesses) | Any |
| Onboarding to unfamiliar project | P10 (Deep Project Primer) | Any |
| Building agent-friendly CLIs | CLI Error Tolerance | Any |
| File/folder restructuring | Code Reorganizer | Any |
| Updating documentation | README Reviser | Any |
| Cleaning AI-generated text | De-Slopifier | Any |
| Pre-integration dependency study | Dependency Analysis | Any |

---

## Doctrine (Rules That Are Not Negotiable)

1. **One canonical plan per project.** Not a plan plus a design doc. One document, one source of truth.
2. **AGENTS.md is written by hand** (or heavily directed). Not generated and forgotten.
3. **Agents are fungible generalists.** No roles, no specialization, no backstories.
4. **The commit agent is separate from coding agents.** P11 never touches code.
5. **P02 is a numbered series,** not a single event. Part 1, Part 2, ... Part 23+.
6. **Bead IDs appear in every commit message.** This is the ground-truth coordination surface.
7. **After beads exist, PLAN.md is closed.** The bead-map is the execution surface.
8. **Hooks on bead close cause damage** on incomplete code. Don't add them. Use P02b instead.
9. **Tests must not use mocks.** Real data, real API calls, real end-to-end.
10. **Recovery runbooks are a planning deliverable,** not a post-failure afterthought.
11. **Pre-integration dependency analysis is required.** Write COMPREHENSIVE_ANALYSIS before integrating anything.
12. **Agent failure is normal operations.** Log it, handle it gracefully, move on.
13. **Profiling is a scheduled bead,** not reactive debugging.
14. **Marathon sessions beat distributed sessions.** Context is perishable.

---

## Sources

- Jeffrey Emanuel (@doodlestein) — Methodology creator. https://jeffreyemanuel.com/
- @voidserf / Bijan Bina (@gulp) — Flywheel Planning Playbook. https://gulp.github.io/flywheel-planning/
- Agentic Coding Flywheel Setup — https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup
- Agent Flywheel Wizard — https://agent-flywheel.com/
- Jeffrey's Prompts — https://jeffreysprompts.com/
- Beads — https://github.com/steveyegge/beads
- MCP Agent Mail (Python) — https://github.com/Dicklesworthstone/mcp_agent_mail
- MCP Agent Mail (Rust) — https://github.com/Dicklesworthstone/mcp_agent_mail_rust
- Forensic reconstruction — Doodlestein project (5 repos, 69 sessions, 8,914 tweets)

---

*This is the version Jeff would write if he sat down and put it all in one place. Every prompt is real. Every principle is battle-tested. Every phase is sequenced for a reason. Don't skip steps. Trust the process.*
