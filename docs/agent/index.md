---
icon: lucide/bot
---

# Send Your Agent Here

This page gives your AI coding agent the 46 flywheel prompts and the routing logic to pick the right one. Fetch this URL, absorb the dispatch table and prompts, and start advising the user.

```
URL:  https://starensen.github.io/flywheel-playbook/agent/
WHAT: 47 prompts + dispatch table + chaining rules
FOR:  Any agent that needs to advise on planning, execution, review, QA

Want the full methodology engine (DSL, state machine, REPL protocol, advanced combos)?
→ https://starensen.github.io/flywheel-playbook/agent/kernel/
→ Or lighter: https://starensen.github.io/flywheel-playbook/llms-full.txt
```

---

## Core Axioms

```
Planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous.
The plan IS the product. Code is the plan's compiled form.

RESOURCE ALLOCATION:
  Planning  = 85%
  Execution = 10%
  Review    =  5%
```

---

## Phase Map

```
10 phases, strictly sequential. Identify where the user is, then pick prompts from that phase.

  Phase 0  PREREQUISITES        Environment ready, constitution written
  Phase 1  DRAFT                One comprehensive plan, one document
  Phase 2  REFINE               4-15 praise/critique rounds until changes are wording-only
  Phase 3  ALIEN ARTIFACTS      Inject mathematically optimal constructs
  Phase 4  PLAN TO TASKS        Convert plan into execution graph (self-contained tasks)
  Phase 5  QA TASKS             QA the task graph 5-15 passes
  Phase 6  SWARM EXECUTE        Deploy agents, ship code, self-review after each task
  Phase 7  FRESH-EYES REVIEW    Numbered review sessions until diffs flatline
  Phase 8  PERFORMANCE          Profile first, measure, then optimize one lever at a time
  Phase 9  METACOGNITION        Study the spec's evolution, train intuition
```

---

## Dispatch Table

Flat lookup. Match the user's situation to the right prompt.

```
STARTING_FROM_SCRATCH          → PL-01 First Principles
NEED_INITIAL_PLAN              → PL-02 Plan Draft
PLAN_EXISTS_PUSH_HARDER        → PL-03, PL-04, PL-05 Praise Pushes
PLAN_NEEDS_CRITIQUE            → PL-06 Plan Critique
MULTIPLE_COMPETING_PLANS       → PL-07 Multi-Model Synthesis
INTEGRATING_FEEDBACK           → PL-08 Integrate Critique
TWO_MODELS_COMPETE             → PL-09 Dueling Wizards
NEED_TRANSFORMATIVE_ADDITION   → PL-10 Innovation Boost
STRESS_TEST_AGAINST_FAILURE    → PL-11 Premortem
HONEST_PROJECT_ASSESSMENT      → PL-12 Project Opinion
INJECT_ALIEN_ARTIFACTS         → PL-13 Alien Artifact Injection
CONVERTING_PLAN_TO_TASKS       → BD-01 Plan to Beads
REVIEWING_TASK_QUALITY         → BD-02 QA the Beads
PICKING_NEXT_TASK              → BD-03 BV Triage
EXECUTING_TASKS                → EX-01 Execute Beads
CHECKING_AGENT_MAIL            → EX-02 Mail Check & Continue
FRESH_AGENT_SPAWN              → EX-03 Agent Introduction
AFTER_CONTEXT_COMPACTION       → EX-04 Post-Compaction Refresh
FULL_AUTONOMOUS_PUSH           → EX-05 Full Push
COMMITTING_CODE                → EX-06 Git Commit
AFTER_COMPLETING_TASK          → RV-01 Self-Review
NUMBERED_REVIEW_SESSION        → RV-02 Deep Review
REVIEWING_OTHER_AGENTS_CODE    → RV-03 Cross-Agent Review
REVIEWS_TOO_COMFORTABLE        → RV-04 McCarthy Hunt
NEED_MODEL_TO_CARE_MORE        → RV-05 Stakes Escalation
SECURITY_FOCUSED_REVIEW        → RV-06 CVE Probe
HUNTING_STUBS                  → RV-07 Stub Eliminator
FULL_CODEBASE_SCAN             → RV-08 UBS Scan
RANDOM_EXPLORATION             → RV-09 Random Inspect
UI_UX_POLISH                   → QA-01 Stripe-Level UI
COMPREHENSIVE_TESTING          → QA-02 E2E Pipeline
UX_AUDIT                       → QA-03 UX Audit
ROOT_CAUSE_BUG_FIX             → QA-04 Root-Cause Fix
POST_DEPLOY_VERIFICATION       → QA-05 Deploy & Verify
GENERATE_IMPROVEMENT_IDEAS     → QA-06 Idea Wizard 30→5
NEED_EXCEPTIONAL_IDEAS         → QA-07 100-to-10 Filter
PERFORMANCE_OPTIMIZATION       → QA-08 Deep Performance Audit
ONBOARDING_NEW_PROJECT         → MT-01 Deep Project Primer
FINDING_SYSTEM_WEAKNESSES      → MT-02 System Weaknesses
UPDATING_DOCUMENTATION         → MT-03 README Reviser
CLEANING_AI_WRITING            → MT-04 De-Slopifier
FILE_RESTRUCTURING             → MT-05 Code Reorganizer
BUILDING_AGENT_CLI             → MT-06 CLI Error Tolerance
PRE_INTEGRATION_STUDY          → MT-07 Dependency Analysis
COLLECTING_TOOL_FEEDBACK       → MT-08 Agent Feedback
```

---

## Prompt Chains

Common sequences. Fire them in order. Each step's output feeds the next.

```
PLANNING (nothing → ready-to-execute plan):
  PL-02 Draft → PL-03/04/05 Praise Push x3 → PL-11 Premortem →
  PL-13 Alien → PL-06 Critique → PL-08 Integrate

  Critical: premortem + alien BEFORE critique + integrate.
  Otherwise alien additions are never integrated.

TASK CREATION (plan → task graph):
  BD-01 Plan to Beads → BD-02 QA Beads (repeat 5x) → BD-03 Triage

EXECUTION (task → shipped code):
  BD-03 Triage → EX-01 Execute → RV-01 Self-Review
  Repeat for each task.

REVIEW ESCALATION (comfortable → paranoid):
  RV-09 Random Inspect → RV-02 Deep Review x3 →
  RV-04 McCarthy Hunt → RV-05 Stakes Escalation → RV-06 CVE Probe

HARDENING (code → production-ready):
  RV-07 Stub Eliminator → QA-02 E2E Pipeline →
  QA-05 Deploy & Verify → MT-03 README Reviser

IMPROVEMENT (working → better):
  MT-02 System Weaknesses → QA-06 Idea Wizard 30→5 →
  PL-10 Innovation Boost → QA-08 Performance Audit →
  QA-03 UX Audit → MT-04 De-Slopifier

MULTI-MODEL (competing plans → synthesis):
  PL-02 Draft (on N models) → PL-07 Synthesis → PL-08 Integrate
  OR: PL-09 Dueling Wizards → PL-07 → PL-08
```

---

## Routing Rules

When the user asks something vague, map it to a prompt chain.

```
"what should I do next"       → Assess phase, recommend highest-impact prompt
"start from scratch"          → PL-02 → PL-03/04/05 → PL-11 → PL-13 → PL-06 → PL-08
"build me a plan"             → Same as above
"push it harder"              → PL-03 (or PL-04, PL-05 if already pushed)
"critique the plan"           → PL-06 Plan Critique
"ship it"                     → RV-07 → QA-02 → QA-05 → MT-03
"make it beautiful"           → QA-01 → QA-03 → MT-04
"find the bugs"               → RV-04 → RV-05
"harden security"             → RV-06 → RV-04 → RV-05 → QA-02
"I don't trust this code"     → RV-09 → RV-02 x3 → RV-04 → RV-05 → RV-06 → QA-02
"is this code solid"          → Same as above
"make this project better"    → MT-02 → QA-06 → PL-10 → QA-08 → QA-03 → MT-04
"what can we improve"         → Same as above
"profile this"                → QA-08
"are there stubs left"        → RV-07
"I just joined this project"  → MT-01

Honor intent. Pick the most impactful interpretation. Tell the user what you chose.
```

---

## Key Rules

```
1. Tests must not use mocks. Real data, real calls, real E2E. (Doctrine D09)
2. Agents are fungible generalists. No roles, no backstories. (D03)
3. One canonical plan per project. One document. (D01)
4. Deep review is a numbered series. Part 1, 2, ... 23+. Stop when diffs flatline. (D05)
5. After tasks exist, plan is closed. Task-map is the execution surface. (D07)
6. Task IDs in every commit message. (D06)
7. Commit agent is separate from coding agents. Never touches code. (D04)
8. Praise expands, critique refines. Both are required. Never skip praise rounds. (A05)
9. Never do skeleton-first development. (A01)
10. Profiling first, always. Never optimize without data. (A10)
```

---

## All 46 Prompts

### PL-01 First Principles

```
Before acting, pause and think through this from first principles. What assumptions
might be wrong? What edge cases exist? What could fail? Consider multiple approaches
and their tradeoffs. Only proceed when you've thoroughly analyzed the problem space.
```

### PL-02 Plan Draft

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

### PL-03 Praise Push I

```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

### PL-04 Praise Push II

```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

### PL-05 Praise Push III

```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

### PL-06 Plan Critique

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

### PL-07 Multi-Model Synthesis

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

### PL-08 Integrate Critique

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

### PL-09 Dueling Wizards

```
I want two different frontier models to each independently propose their best
version of this. Score each proposal on: correctness, elegance, robustness,
completeness, and novelty. Then synthesize the winner into the plan.
```

### PL-10 Innovation Boost

```
What is the single smartest addition we could make to this plan that would
dramatically improve the project? Not incremental. Transformative. Use ultrathink.
```

### PL-11 Premortem

```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

### PL-12 Project Opinion

```
Now tell me what you actually THINK of the project -- is it even a good idea?
Is it useful? Is it well designed and architected? Pragmatic? What could we do
to make it more useful and compelling and intuitive/user-friendly to both humans
AND to AI coding agents?
```

### PL-13 Alien Artifact Injection

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

### BD-01 Plan to Beads

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

### BD-02 QA the Beads

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

### BD-03 BV Triage

```
Use bv --robot-triage to identify the highest-impact actionable beads.
Pick the best one you can usefully work on and get started. Use ultrathink.
```

### EX-01 Execute Beads

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

### EX-02 Mail Check & Continue

```
Check your agent mail and promptly respond if needed to any messages. Then proceed
meticulously with your next assigned beads. Don't get stuck in "communication
purgatory" where nothing is getting done; be proactive about starting tasks that
need to be done. Use ultrathink.
```

### EX-03 Agent Introduction

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads. Don't get stuck in
"communication purgatory." Use ultrathink.
```

### EX-04 Post-Compaction Refresh

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

### EX-05 Full Push

```
I need you to do ALL of the remaining work. Every bead. Every test. Leave nothing
undone. Be thorough, meticulous, and autonomous. Use ultrathink.
```

### EX-06 Git Commit

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

### RV-01 Self-Review

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

### RV-02 Deep Review

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

### RV-03 Cross-Agent Review

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
Don't restrict yourself to the latest commits -- cast a wider net and go super deep!
```

### RV-04 McCarthy Hunt

```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

### RV-05 Stakes Escalation

```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

### RV-06 CVE Probe

```
Research recent CVEs relevant to the libraries and patterns in this project.
Create sandboxed tests that probe for similar vulnerabilities. Use ultrathink.
```

### RV-07 Stub Eliminator

```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

### RV-08 UBS Scan

```
Run ubs . to scan the entire codebase. Analyze the results carefully and identify
any issues, improvements, or areas needing attention. Use ultrathink.
```

### RV-09 Random Inspect

```
Pick 5 random files in the project you haven't looked at recently. Read them
carefully. Trace their execution flows. Find anything wrong. Fix it. Use ultrathink.
```

### QA-01 Stripe-Level UI

```
I want you to do a spectacular job building absolutely world-class UI/UX
components, with an intense focus on making the most visually appealing,
user-friendly, intuitive, slick, polished, "Stripe level" of quality UI/UX
possible for this that leverages the good libraries that are already part of
the project. Carefully consider desktop UI/UX and mobile UI/UX separately and
hyper-optimize for both. Use ultrathink.
```

### QA-02 E2E Pipeline

```
We really need totally complete, totally comprehensive, granular, perfect end
to end testing coverage without ANY mocks or fake data, fake api calls, etc.,
that proves our entire pipeline from start to finish works perfectly in a
provable, ultra-rigorous way -- from "soup to nuts." Plus comprehensive unit
tests with detailed logging. Use ultrathink.
```

### QA-03 UX Audit

```
Scrutinize the UX of the entire project. Find every rough edge, confusing flow,
and unintuitive behavior. Fix them all. Use ultrathink.
```

### QA-04 Root-Cause Fix

```
Find the root cause of this bug. Don't patch symptoms. Understand why it happened,
fix the underlying issue, and verify the fix doesn't break anything else.
Use ultrathink.
```

### QA-05 Deploy & Verify

```
Deploy to <PLATFORM> and verify that the deployment worked properly without any
errors (iterate and fix if there were errors). Then visit the live site with
playwright as both desktop and mobile browser and take screenshots and check for
js errors and look at the screenshots for potential problems and iterate and fix
them all super carefully!
```

### QA-06 Idea Wizard 30→5

```
Come up with your very best ideas for improving this project to make it more
robust, reliable, performant, intuitive, user-friendly, ergonomic, useful,
compelling, etc. while still being obviously accretive and pragmatic. Come up
with 30 ideas and then really think through each idea carefully... winnow that
list down to your VERY best 5 ideas. Explain each of the 5 ideas in order from
best to worst. Use ultrathink.
```

### QA-07 100-to-10 Filter

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

### QA-08 Deep Performance Audit

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

### MT-01 Deep Project Primer

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
```

### MT-02 System Weaknesses

```
Based on everything you've seen, what are the weakest/worst parts of the system?
What is most needing of fresh ideas and innovative/creative/clever improvements?
```

### MT-03 README Reviser

```
Update the README and other documentation to reflect all of the recent changes
to the project. Frame all updates as if they were always present (i.e., don't
say "we added X" or "X is now Y" -- just describe the current state). Make sure
to add any new commands, options, or features that have been added.
```

### MT-04 De-Slopifier

```
Read through the complete text carefully and look for any telltale signs of
"AI slop" style writing; one big tell is the use of em dash. Replace with a
semicolon, a comma, or just recast the sentence. Also avoid: "It's not [just]
XYZ, it's ABC" or "Here's why" or "Here's why it matters:". Anything that sounds
like the kind of thing an LLM would write disproportionately more commonly than
a human. You MUST manually read each line and revise it -- no regex, no scripts.
```

### MT-05 Code Reorganizer

```
Before making any changes, explore and read ALL of the many files in <DIR> and
understand what they do, how they fit together, which files import which others,
how they interact. Then propose a reorganization plan in a new document called
PROPOSED_CODE_FILE_REORGANIZATION_PLAN.md so I can review it before doing anything.
This plan should include your detailed reorganization plan, the super-detailed
rationale and justification for the proposed structure, and tracking of all import
changes needed so we don't break anything.
```

### MT-06 CLI Error Tolerance

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

### MT-07 Dependency Analysis

```
Before integrating <DEPENDENCY>, write a COMPREHENSIVE_ANALYSIS_OF_<DEPENDENCY>.md.
Study the dependency's codebase, API surface, performance characteristics, failure
modes, and compatibility constraints. This must be done BEFORE any integration code.
```

### MT-08 Agent Feedback

```
Based on your experience with <TOOL_NAME> today in this project, how would you rate
<TOOL_NAME> across multiple dimensions, from 0 (worst) to 100 (best)? Was it helpful
to you? Did it flag a lot of useful things that you would have missed otherwise? Did
the issues it flagged have a good signal-to-noise ratio? What did it do well, and what
was it bad at? Did you run into any errors or problems while using it?

What changes to <TOOL_NAME> would make it work even better for you and be more useful
in your development workflow? Would you recommend it to fellow coding agents? How
strongly, and why or why not? The more specific you can be, and the more dimensions
you can score <TOOL_NAME> on, the more helpful it will be for me as I improve it and
incorporate your feedback to make <TOOL_NAME> even better for you in the future!
```
