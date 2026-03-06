---
title: "3.2 Prompt Reference"
icon: lucide/message-square
---

The ACFS prompt library contains 47 named prompts organized into 6 categories. They are designed to be used verbatim -- copy-paste, not paraphrase. Jeff maps them to physical Stream Deck buttons for instant dispatch to agents via NTM.

| Prefix | Category | Count |
|--------|----------|-------|
| **PL-** | Planning | 13 |
| **BD-** | Beads | 3 |
| **EX-** | Execution | 6 |
| **RV-** | Review | 9 |
| **QA-** | Quality & Ideation | 8 |
| **MT-** | Meta | 8 |

Each prompt is self-documenting: prefix = category, number = order within category.

> *"Basically they attend to every word, so every word is critically important. You want prompts to be short so that they don't overly constrain the models but they need to force the model to work hard to go beyond their default posture."*
> -- Jeffrey Emanuel

> *"Good prompting might seem like a hand wavy thing, but there's a lot that goes into making a prompt that performs well in many situations. I've been doing this stuff so much for so long that I've basically internalized the theory of mind of these models and the gestalt psychology."*
> -- Jeffrey Emanuel

---

### 3.2.1 The Dispatch Table

When you are in situation X, use prompt Y. This is the quick-lookup version. For the full copy-paste prompt text, see the [Full Prompt Pack](../prompts/prompt-pack.md).

**Planning Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Starting from scratch | First Principles | PL-01 |
| First draft of plan needed | Plan Draft | PL-02 |
| First draft exists, push quality higher | Praise Push I / II / III | PL-03 / PL-04 / PL-05 |
| Plan needs analytical critique | Plan Critique | PL-06 |
| Multiple models produced competing plans | Multi-Model Synthesis | PL-07 |
| Integrating critique feedback | Integrate Critique | PL-08 |
| Two models should compete on a problem | Dueling Wizards | PL-09 |
| Need a single transformative addition | Innovation Boost | PL-10 |
| Stress-test plan against failure | Premortem | PL-11 |
| Need honest project assessment | Project Opinion | PL-12 |
| Injecting alien artifacts | Alien Artifact Injection | PL-13 |

**Bead Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Converting plan to beads | Plan to Beads | BD-01 |
| Reviewing bead quality (repeat N times) | QA the Beads | BD-02 |
| Picking next bead to work on | BV Triage | BD-03 |

**Execution Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Ongoing bead execution | Execute Beads (main loop) | EX-01 |
| Agent mail check + continue | Mail Check & Continue | EX-02 |
| Spawning a fresh agent | Agent Introduction | EX-03 |
| After context compaction | Post-Compaction Refresh | EX-04 |
| Full autonomous push | Full Push | EX-05 |
| Committing code (commit agent only) | Git Commit | EX-06 |

**Review Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| After completing a bead | Self-Review | RV-01 |
| Numbered review session (Part 1, 2, ... 23+) | Deep Review | RV-02 |
| Reviewing other agents' code | Cross-Agent Review | RV-03 |
| Reviews seem too comfortable | McCarthy Hunt (escalation) | RV-04 |
| Need the model to care more | Stakes Escalation | RV-05 |
| Security-focused review | CVE Probe | RV-06 |
| Hunting stubs and placeholders | Stub Eliminator | RV-07 |
| Full codebase scan | UBS Scan | RV-08 |
| Random codebase exploration | Random Inspect | RV-09 |

**Quality & Ideation:**

| Situation | Prompt | ID |
|-----------|--------|----|
| UI/UX polish | Stripe-Level UI | QA-01 |
| Comprehensive testing | E2E Pipeline | QA-02 |
| UX audit | UX Audit | QA-03 |
| Root-cause bug fixing | Root-Cause Fix | QA-04 |
| Post-deploy verification | Deploy & Verify | QA-05 |
| Generating improvement ideas | Idea Wizard 30->5 | QA-06 |
| Need exceptional ideas only | 100-to-10 Filter | QA-07 |
| Performance optimization | Deep Performance Audit | QA-08 |

**Meta:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Onboarding to unfamiliar project | Deep Project Primer | MT-01 |
| Finding system weaknesses | System Weaknesses | MT-02 |
| Updating documentation | README Reviser | MT-03 |
| Cleaning AI-generated text | De-Slopifier | MT-04 |
| File/folder restructuring | Code Reorganizer | MT-05 |
| Building agent-friendly CLIs | CLI Error Tolerance | MT-06 |
| Pre-integration dependency study | Dependency Analysis | MT-07 |
| Collecting agent feedback on a tool | Agent Feedback | MT-08 |

---

### 3.2.2 Planning Prompts (PL-01 through PL-13)

These prompts drive Phases 1-3: plan creation, refinement, and alien artifacts.

#### PL-01 -- First Principles (prefix for everything)

```
Before acting, pause and think through this from first principles. What assumptions
might be wrong? What edge cases exist? What could fail? Consider multiple approaches
and their tradeoffs. Only proceed when you've thoroughly analyzed the problem space.
```

**Usage:** Prefix for any high-stakes decision. Appears in all 4 forensically analyzed repos (frankensqlite, rust_scriptbots, frankenterm, mcp_agent_mail_rust). Universal. No exceptions.

**Forensic frequency:** 10 confirmed uses across 5 repos. Appears in architecture decisions, plan bootstraps, and fix sessions.

---

#### PL-02 -- Plan Draft

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

**Usage:** Phase 1 kickoff. Use in a web app with extended thinking (ChatGPT Pro or Claude). The first output is always bad. That is the starting point, not the end. See [section 2.3](section-2-3.md) for the full planning workflow.

---

#### PL-03 / PL-04 / PL-05 -- Praise Pushes (push past default quality)

**PL-03 (Push I):**
```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

**PL-04 (Push II):**
```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

**PL-05 (Push III):**
```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

**Why they work:** The model is not a static function. It responds to the emotional register of the conversation. Analytical critique refines; praise pushes expand. Skipping them is [Anti-Pattern #5](../reference/anti-patterns.md) -- "I'll just go straight to analytical critique" narrows the solution space before it has been explored.

**Forensic evidence:** PL-03 through PL-05 sequence confirmed in frankensqlite (C1-C3), rust_scriptbots (H01-H02), frankenterm (F01). Always precedes analytical critique. Always runs in the same Claude conversation.

---

#### PL-06 -- Plan Critique (single-model)

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

**Usage:** After praise pushes. Run with a strong reasoning model (ChatGPT Pro with extended thinking, or Claude with extended thinking). The diff-based format forces concrete, integratable changes instead of vague advice. Can be automated with APR (`apr refine plan.md --rounds 10`).

**Forensic frequency:** 5 confirmed uses across all 4 repos. Universal planning pattern.

---

#### PL-07 -- Multi-Model Synthesis

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

**Usage:** Fire 3+ models in parallel on the same planning task. Then synthesize. This is a parallel compute job, not a conversation.

**Forensic evidence:** frankensqlite has both `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1.md` (Claude) and `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1_CODEX.md` (Codex). frankenterm has `PLAN.md` + `PLAN_CODEX.md`. frankentui has `PLAN_TO_CREATE_FRANKENTUI__CODEX.md` (v6.3, 502L) vs `PLAN_TO_CREATE_FRANKENTUI__OPUS.md` (v6.4, 4799L) -- competitive versioning with a documented winner (Opus by 9.5x depth).

---

#### PL-08 -- Integrate Critique

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

**Usage:** After PL-06 (Plan Critique) or PL-07 (Multi-Model Synthesis) produces feedback, this prompt folds it back into the plan without destroying coherence. The key discipline: reject-with-rationale is a valid response to feedback. Not all critique is correct.

---

#### Additional Planning Prompts

**PL-09 -- Dueling Wizards** (two-model competitive scoring):
```
I want two different frontier models to each independently propose their best
version of this. Score each proposal on: correctness, elegance, robustness,
completeness, and novelty. Then synthesize the winner into the plan.
```

**PL-10 -- Innovation Boost:**
```
What is the single smartest addition we could make to this plan that would
dramatically improve the project? Not incremental. Transformative.
Use ultrathink.
```

**PL-11 -- Premortem** (pre-beads risk check):
```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

**PL-12 -- Project Opinion** (honest assessment):
```
Now tell me what you actually THINK of the project -- is it even a good idea?
Is it useful? Is it well designed and architected? Pragmatic? What could we do
to make it more useful and compelling and intuitive/user-friendly to both humans
AND to AI coding agents?
```

**PL-13 -- Alien Artifact Injection:**
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

**Usage:** Phase 3 dedicated prompt. Run after the plan is converged but before converting to beads. Use a model strong in formal reasoning (o3 or equivalent). See [section 2.5.4](section-2-5.md#254-alien-artifacts) for the full alien artifacts workflow.

---

### 3.2.3 Bead Prompts (BD-01 through BD-03)

These prompts drive Phases 4-5: converting the locked plan into an execution graph and QA-ing it.

#### BD-01 -- Plan to Beads

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

**Usage:** Phase 4. Converts the locked plan into the execution graph. After this runs, PLAN.md is closed. The bead-map is the execution surface. Reopening the plan during execution is [Anti-Pattern #7](../reference/anti-patterns.md).

---

#### BD-02 -- QA the Beads (repeat N times)

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

**Usage:** Phase 5. Run 5-15 passes. The most common failure mode is agents under-scoping beads during QA. The stop condition is when changes become reordering and renaming instead of finding missing work.

| Pass | Focus |
|------|-------|
| 1 | Completeness -- are all plan items represented? |
| 2 | Dependencies -- are orderings correct? Missing links? |
| 3 | Scope -- are beads too large or too small? |
| 4 | Tests -- is coverage comprehensive with detailed logging? |
| 5 | Security/ops -- are there beads for threats, alerts, monitoring? |

---

#### BD-03 -- BV Triage (bead prioritization)

```
Use bv --robot-triage to identify the highest-impact actionable beads.
Pick the best one you can usefully work on and get started. Use ultrathink.
```

**Usage:** Directs the agent to use graph-theory prioritization instead of picking beads by intuition.

---

### 3.2.4 Execution Prompts (EX-01 through EX-06)

These prompts drive Phase 6: deploying coordinated agents to execute beads.

#### EX-01 -- Execute Beads (main loop)

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

**Usage:** The most-used prompt across all repos. 11 confirmed uses across 4 repos. This is the execution workhorse. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication.

---

#### EX-02 -- Mail Check & Continue (anti-deadlock)

```
Check your agent mail and promptly respond if needed to any messages. Then proceed
meticulously with your next assigned beads. Don't get stuck in "communication
purgatory" where nothing is getting done; be proactive about starting tasks that
need to be done. Use ultrathink.
```

**Usage:** Jeff sends this via NTM when he sees an agent stalling. The key phrase is "communication purgatory" -- agents can endlessly discuss coordination without shipping code.

---

#### EX-03 -- Agent Introduction (fresh spawn)

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads. Don't get stuck in
"communication purgatory." Use ultrathink.
```

**Usage:** Every new agent's first prompt. Handles self-onboarding: read AGENTS.md, understand the codebase, register with Agent Mail, check for messages, and start working.

---

#### EX-04 -- Post-Compaction Refresh

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

**Usage:** When an agent hits context compaction, it loses roughly half its context window. This prompt gives it the constitution back before it makes any decisions. Simple by design -- the agent just lost context, do not overload it.

---

#### EX-05 -- Full Push (full autonomous push)

```
I need you to do ALL of the remaining work. Every bead. Every test. Leave nothing
undone. Be thorough, meticulous, and autonomous. Use ultrathink.
```

**Usage:** When the bead count is low enough for one agent to finish. Confirmed in 5 uses across 4 repos. Jeff uses this for final pushes and overnight autonomous runs. In Codex (which supports message queuing), Jeff chains this as a multi-step pipeline entered up front: scrutinize the codebase, generate beads, QA beads in plan space, execute beads, fresh-eyes review, commit. Codex processes these one at a time. Claude Code does not support this workflow because follow-up messages interrupt the running agent.

> *"These are all entered up front and go into a queue of messages which Codex processes one at a time. Then you can come back 3+ hours later to see the incredible amount of work done autonomously for you."*
> -- Jeffrey Emanuel

---

#### EX-06 -- Git Commit (commit agent only)

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

**Usage:** The commit agent is a dedicated Claude Code instance that runs EX-06 continuously. It does not modify code. It reads the diff, groups changes into logical commits, writes detailed messages with bead IDs, and pushes. [Doctrine #4](../reference/doctrine.md): The commit agent is separate from coding agents.

> *"Look at my exact prompt. I explicitly tell the committer not to modify the code. That's why. It's a purely logistical thing."*
> -- Jeffrey Emanuel

---

### 3.2.5 Review Prompts (RV-01 through RV-09)

These prompts drive Phase 7: fresh-eyes review until diffs flatline.

#### RV-01 -- Self-Review (post-implementation)

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

**Usage:** Runs after every bead, not just at the end. Baked into the Bead Execution Protocol. This is the inline quality gate.

---

#### RV-02 -- Deep Review / Fresh Eyes Bug Hunt

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

**Usage:** This is a numbered series, not a single event ([Doctrine #5](../reference/doctrine.md)). Run as Part 1, Part 2, Part 3... Part 23 if needed. Each session gets a sequential ID. The prompt deliberately does NOT tell the model what to look for -- it asks the model to explore, understand, and apply judgment. Every prescriptive bug checklist is a tunnel that narrows the model's attention. RV-02 keeps it wide.

**Forensic frequency:** 9 confirmed uses across 4 repos. frankensqlite alone had 4 consecutive RV-02 commands (C11-C14). frankentui documents "Part 23" in FIXES_SUMMARY.md.

---

#### RV-03 -- Cross-Agent Review

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

**Usage:** Run with a different model than the one that wrote the code for true "fresh eyes."

---

#### Escalation Prompts (when reviews plateau)

**RV-04 -- McCarthy Hunt:**
```
I know for a fact that there are at least 87 serious bugs throughout this
project impacting every facet of its operation. The question is whether you
can find and diagnose and fix all of them autonomously. I believe in you.
```

**RV-05 -- Stakes Escalation:**
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

These change the model's internal prior. By default, models are optimistic reviewers. RV-04 and RV-05 make them paranoid. Confirmed in frankensqlite (C12-C13).

---

#### RV-06 -- CVE Probe

```
Research recent CVEs relevant to the libraries and patterns in this project.
Create sandboxed tests that probe for similar vulnerabilities. Use ultrathink.
```

**Usage:** Run after dependency updates or when using libraries with known CVE history. Best with Gemini or o3 (strong at formal reasoning about vulnerability patterns). Frequency: once per project, plus after any major dependency upgrade. The sandboxed tests become part of the permanent test suite, catching future regressions.

---

#### RV-07 -- Stub Eliminator

```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

**Usage:** Run after every fix session and before closing any project phase. This targets the [Placeholder Regression anti-pattern](section-3-6.md#367-placeholder-regression) -- agents that "simplify" working code into stubs while fixing nearby bugs. Best run with a fresh agent that has no memory of the original implementation. Frequency: every 2-3 fix rounds, and always before final review.

---

#### RV-08 -- UBS Scan

```
Run ubs . to scan the entire codebase. Analyze the results carefully and identify
any issues, improvements, or areas needing attention. Use ultrathink.
```

**Usage:** Automated codebase health scan using the [UBS tool](section-3-1.md). Run periodically during execution (every 20-30 beads) and always before entering Phase 7 (review). The agent runs the `ubs` CLI, then analyzes the structured output for patterns that warrant manual review. Best with Claude or Codex (needs shell access to run the tool).

---

#### RV-09 -- Random Inspect

```
Pick 5 random files in the project you haven't looked at recently. Read them
carefully. Trace their execution flows. Find anything wrong. Fix it.
Use ultrathink.
```

**Usage:** The "spot check" prompt. Catches issues in files that no other review specifically targets -- utility modules, configuration files, helper functions that every bead assumes work correctly. Run with fresh agents (the "recently" constraint ensures they explore unfamiliar territory). Frequency: once per review cycle. Particularly effective with Gemini, which brings an orthogonal perspective to files Claude or Codex wrote.

---

### 3.2.6 Quality & Ideation Prompts (QA-01 through QA-08)

#### QA-01 -- Stripe-Level UI

```
I want you to do a spectacular job building absolutely world-class UI/UX
components, with an intense focus on making the most visually appealing,
user-friendly, intuitive, slick, polished, "Stripe level" of quality UI/UX
possible for this that leverages the good libraries that are already part of
the project. Carefully consider desktop UI/UX and mobile UI/UX separately and
hyper-optimize for both. Use ultrathink.
```

---

#### QA-02 -- E2E Pipeline

```
We really need totally complete, totally comprehensive, granular, perfect end
to end testing coverage without ANY mocks or fake data, fake api calls, etc.,
that proves our entire pipeline from start to finish works perfectly in a
provable, ultra-rigorous way -- from "soup to nuts." Plus comprehensive unit
tests with detailed logging. Use ultrathink.
```

---

#### QA-03 -- UX Audit

```
Scrutinize the UX of the entire project. Find every rough edge, confusing flow,
and unintuitive behavior. Fix them all. Use ultrathink.
```

**Usage:** Run after the primary implementation is complete and QA-01 (Stripe-Level UI) has been applied. QA-01 builds the UI; QA-03 stress-tests it from a user's perspective. Best with a fresh agent that did not build the UI (avoids authorial bias). Check both desktop and mobile flows. Pairs naturally with QA-05 (Deploy & Verify) for end-to-end validation.

---

#### QA-04 -- Root-Cause Fix (root-cause only)

```
Find the root cause of this bug. Don't patch symptoms. Understand why it happened,
fix the underlying issue, and verify the fix doesn't break anything else.
Use ultrathink.
```

**Usage:** For specific, identified bugs -- not for general review (use RV-02 for that). The key difference from a standard fix prompt is the explicit instruction to trace causation, not just symptoms. Send this when an agent's previous fix attempt patched the symptom but the bug recurred. Best with Opus or Codex, which tend to follow the causal chain more reliably than models that jump to the nearest plausible fix.

---

#### QA-05 -- Deploy & Verify

```
Deploy to <PLATFORM> and verify that the deployment worked properly without any
errors (iterate and fix if there were errors). Then visit the live site with
playwright as both desktop and mobile browser and take screenshots and check for
js errors and look at the screenshots for potential problems and iterate and fix
them all super carefully!
```

**Usage:** Replace `<PLATFORM>` with your deployment target (e.g., `Vercel`, `Railway`, `fly.io`, `AWS`). Requires Playwright installed in the agent's environment. The prompt covers the full deploy-verify-fix loop: deploy, visit as desktop browser, visit as mobile browser, screenshot both, check for JS console errors, and iterate. Run after QA-02 (E2E tests pass) and before marking the project complete. Best with Claude Code (needs full shell access for deployment commands and Playwright).

---

#### QA-06 -- Idea Wizard (30 -> 5)

```
Come up with your very best ideas for improving this project to make it more
robust, reliable, performant, intuitive, user-friendly, ergonomic, useful,
compelling, etc. while still being obviously accretive and pragmatic. Come up
with 30 ideas and then really think through each idea carefully... winnow that
list down to your VERY best 5 ideas. Explain each of the 5 ideas in order from
best to worst. Use ultrathink.
```

**Usage:** When you need improvement ideas but want quality over quantity. The 30->5 winnowing forces the model to self-critique.

---

#### QA-07 -- 100-to-10 Filter (when quality matters most)

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

---

#### QA-08 -- Deep Performance Audit (Phase 8)

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

**Forensic evidence:** frankentui T07: flamegraph (bd-3jlw5.7) + heaptrack (bd-3jlw5.8) as scheduled beads. 43 screens x 2 sizes x 100 cycles = 8,600 renders benchmarked.

---

### 3.2.7 Meta Prompts (MT-01 through MT-08)

#### MT-01 -- Deep Project Primer (read-only onboarding)

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
```

**Usage:** Read-only onboarding. Unlike EX-03, this does not include Agent Mail registration or bead execution. Used when you want the agent to understand the codebase before giving it a specific task.

---

#### MT-02 -- System Weaknesses

```
Based on everything you've seen, what are the weakest/worst parts of the system?
What is most needing of fresh ideas and innovative/creative/clever improvements?
```

**Usage:** Strategic assessment prompt. Run with a fresh agent that has just completed MT-01 (Deep Project Primer) -- it has full context but no authorial investment. The open-ended framing avoids leading the model toward specific areas. Useful mid-project (after 50% of beads complete) to identify structural issues before they compound, and at project end to generate improvement backlog for the next iteration.

---

#### MT-03 -- README Reviser (present-tense docs)

```
Update the README and other documentation to reflect all of the recent changes
to the project. Frame all updates as if they were always present (i.e., don't
say "we added X" or "X is now Y" -- just describe the current state). Make sure
to add any new commands, options, or features that have been added.
```

**Usage:** Run after every major implementation milestone (every 15-20 beads, or at epic boundaries). The "present tense" instruction is critical: it prevents the documentation from accumulating changelog-style entries that confuse future agents and users. The agent should read the existing README, compare it to the current codebase, and rewrite outdated sections as if the new state was always the design.

---

#### MT-04 -- De-Slopifier (remove AI writing tells)

```
Read through the complete text carefully and look for any telltale signs of
"AI slop" style writing; one big tell is the use of em dash. Replace with a
semicolon, a comma, or just recast the sentence. Also avoid: "It's not [just]
XYZ, it's ABC" or "Here's why" or "Here's why it matters:". Anything that sounds
like the kind of thing an LLM would write disproportionately more commonly than
a human. You MUST manually read each line and revise it -- no regex, no scripts.
```

**Usage:** Run on any user-facing text (README, docs, UI copy) before release. The "no regex, no scripts" instruction forces the model to read each sentence individually rather than pattern-matching. Common AI tells to watch for: em dashes used as dramatic pauses, "It's worth noting that," "Here's the thing," "Let's dive in," "In conclusion," and the "Not X, but Y" rhetorical structure. Run with Claude (strong prose quality) or Gemini (catches different patterns than Claude).

---

#### MT-05 -- Code Reorganizer (file restructuring)

```
Before making any changes, explore and read ALL of the many files in <DIR> and
understand what they do, how they fit together, which files import which others,
how they interact. Then propose a reorganization plan in a new document called
PROPOSED_CODE_FILE_REORGANIZATION_PLAN.md so I can review it before doing anything.
This plan should include your detailed reorganization plan, the super-detailed
rationale and justification for the proposed structure, and tracking of all import
changes needed so we don't break anything.
```

**Usage:** Replace `<DIR>` with the directory to reorganize (e.g., `src/`, `lib/`). This is a two-phase prompt: first the agent explores and proposes (producing the plan document), then the human reviews and approves before the agent executes. Never let this run in a single shot -- the proposal step is a mandatory gate. Run when file count in a directory exceeds ~30 or when multiple agents report difficulty navigating the project structure.

---

#### MT-06 -- CLI Error Tolerance (FCBC -- For Clankers, By Clankers)

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

---

#### MT-07 -- Dependency Analysis (pre-integration due diligence)

```
Before integrating <DEPENDENCY>, write a COMPREHENSIVE_ANALYSIS_OF_<DEPENDENCY>.md.
Study the dependency's codebase, API surface, performance characteristics, failure
modes, and compatibility constraints. This must be done BEFORE any integration code.
```

---

#### MT-08 -- Agent Feedback (tool quality loop)

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

**Usage:** Jeff runs this after agents have used a new tool for a full session. The output feeds directly into the next iteration of the tool. This is the FCBC feedback loop in practice: agents are the primary users of agent tools, so agent feedback is the primary signal for tool improvement.

---

### 3.2.8 Prompt Chains

Pre-composed sequences for common workflows. Each chain is a series of prompts fired in order.

**PLANNING chain** (Phase 1-3):
PL-02 -> PL-03 -> PL-04 -> PL-05 -> PL-11 -> PL-13 -> PL-06 -> PL-08

**TASK CREATION chain** (Phase 4-5):
BD-01 -> BD-02 (x5) -> BD-03

**REVIEW ESCALATION chain** (Phase 7):
RV-09 -> RV-02 (x3) -> RV-04 -> RV-05 -> RV-06

**HARDENING chain** (pre-release):
RV-07 -> QA-02 -> QA-05 -> MT-03

---

### 3.2.9 Adapting Prompts

The prompts are designed to be used verbatim. However, there are three valid adaptation patterns:

**1. Filling in placeholders.** Prompts containing `<PLAN_FILE_PATH>`, `<DEPENDENCY>`, `<PROJECT_NAME>`, etc. require substitution. This is not adaptation; it is parameterization.

**2. Chaining prompts.** Jeff chains PL-01 as a prefix before other prompts when the decision is high-stakes. PL-01 + EX-01 = "think from first principles, then execute the highest-priority bead." PL-01 + PL-06 = "think from first principles, then critique this plan."

**3. The emotional register.** The praise pushes (PL-03 through PL-05) work because they shift the model's internal prior. You can adjust the intensity but should preserve the core mechanic: each round acknowledges improvement while insisting on more.

**What not to do:**
- Do not add role-play preambles ("You are an expert senior backend engineer"). Frontier models understand intent. Role-playing constraints narrow the solution space ([Anti-Pattern #2](../reference/anti-patterns.md) from the Doctrine).
- Do not enumerate everything the model should think about. Short prompts that force the model beyond its default posture beat long prompts that catalog concerns.
- Do not paraphrase. The prompts are calibrated. "Carefully fix anything you uncover" is not the same as "fix bugs." The word "carefully" changes the model's behavior.

---

!!! tip "Related pages on this site"

    - [Full Prompt Pack](../prompts/prompt-pack.md) -- All 47 prompts in copy-paste format
    - [Prompt Index](../prompts/overview.md) -- Quick-lookup table
    - [Dispatch Table](../reference/dispatch.md) -- Situation-to-prompt mapping
