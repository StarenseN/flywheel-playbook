---
icon: lucide/copy
---

# Full Prompt Pack

47 prompts on one page for copy-paste. Organized by category. Each ID is self-documenting: the prefix tells you the category, the number gives you order within it.

| Prefix | Category | Count |
|:-------|:---------|:------|
| **PL-** | Planning | 13 |
| **BD-** | Beads | 3 |
| **EX-** | Execution | 6 |
| **RV-** | Review | 9 |
| **QA-** | Quality & Ideation | 8 |
| **MT-** | Meta | 7 |

---

## PL — Planning

### PL-01 First Principles (ex P01)
*Prefix for everything. Use before any significant decision.*

```
Before acting, pause and think through this from first principles. What assumptions
might be wrong? What edge cases exist? What could fail? Consider multiple approaches
and their tradeoffs. Only proceed when you've thoroughly analyzed the problem space.
```

### PL-02 Plan Draft
*The initial architect prompt. Produce the first comprehensive plan.*

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

### PL-03 Praise Push I (ex P30)

```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

### PL-04 Praise Push II (ex P31)

```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

### PL-05 Praise Push III (ex P32)

```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

### PL-06 Plan Critique — Round Type A (ex P04)

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

### PL-07 Multi-Model Synthesis — Round Type B (ex P15)

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

### PL-09 Dueling Wizards (ex P17)

```
I want two different frontier models to each independently propose their best
version of this. Score each proposal on: correctness, elegance, robustness,
completeness, and novelty. Then synthesize the winner into the plan.
```

### PL-10 Innovation Boost (ex P26)

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

---

## BD — Beads

### BD-01 Plan to Beads (ex P05)

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

### BD-02 QA the Beads (ex P06)
*Repeat N times until changes flatline.*

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

### BD-03 BV Triage (ex P24)

```
Use bv --robot-triage to identify the highest-impact actionable beads.
Pick the best one you can usefully work on and get started. Use ultrathink.
```

---

## EX — Execution

### EX-01 Execute Beads (ex P08)
*The main loop. Every agent runs this.*

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

### EX-02 Mail Check & Continue (ex P09)

```
Check your agent mail and promptly respond if needed to any messages. Then proceed
meticulously with your next assigned beads. Don't get stuck in "communication
purgatory" where nothing is getting done; be proactive about starting tasks that
need to be done. Use ultrathink.
```

### EX-03 Agent Introduction (ex P18)
*Fresh spawn or after context loss.*

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads. Don't get stuck in
"communication purgatory." Use ultrathink.
```

### EX-04 Post-Compaction Refresh (ex P20)

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

### EX-05 Full Push (ex P25)

```
I need you to do ALL of the remaining work. Every bead. Every test. Leave nothing
undone. Be thorough, meticulous, and autonomous. Use ultrathink.
```

### EX-06 Git Commit (ex P11)
*Commit agent only. Does not modify code.*

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

---

## RV — Review

### RV-01 Self-Review (ex P02b)
*Runs after every bead. Baked into the execution protocol.*

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

### RV-02 Deep Review (ex P02)
*Numbered sessions: Part 1, Part 2, Part 3...*

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

### RV-03 Cross-Agent Review (ex P03)

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

### RV-04 McCarthy Hunt (ex P27)
*Escalation. When reviews seem too comfortable.*

```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

### RV-05 Stakes Escalation (ex P28)

```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

### RV-06 CVE Probe (ex P29)

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

### RV-08 UBS Scan (ex P21)

```
Run ubs . to scan the entire codebase. Analyze the results carefully and identify
any issues, improvements, or areas needing attention. Use ultrathink.
```

### RV-09 Random Inspect (ex P23)

```
Pick 5 random files in the project you haven't looked at recently. Read them
carefully. Trace their execution flows. Find anything wrong. Fix it. Use ultrathink.
```

---

## QA — Quality & Ideation

### QA-01 Stripe-Level UI (ex P12)

```
I want you to do a spectacular job building absolutely world-class UI/UX
components, with an intense focus on making the most visually appealing,
user-friendly, intuitive, slick, polished, "Stripe level" of quality UI/UX
possible for this that leverages the good libraries that are already part of
the project. Carefully consider desktop UI/UX and mobile UI/UX separately and
hyper-optimize for both. Use ultrathink.
```

### QA-02 E2E Pipeline (ex P13)

```
We really need totally complete, totally comprehensive, granular, perfect end
to end testing coverage without ANY mocks or fake data, fake api calls, etc.,
that proves our entire pipeline from start to finish works perfectly in a
provable, ultra-rigorous way -- from "soup to nuts." Plus comprehensive unit
tests with detailed logging. Use ultrathink.
```

### QA-03 UX Audit (ex P19)

```
Scrutinize the UX of the entire project. Find every rough edge, confusing flow,
and unintuitive behavior. Fix them all. Use ultrathink.
```

### QA-04 Root-Cause Fix (ex P22)

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

### QA-06 Idea Wizard 30→5 (ex P16)

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

---

## MT — Meta

### MT-01 Deep Project Primer (ex P10)

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
```

### MT-02 System Weaknesses (ex P14)

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

*After an agent has used a tool or workflow for a session, collect structured feedback to improve it.*

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
