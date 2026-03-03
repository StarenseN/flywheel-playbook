---
icon: lucide/message-square
---

## 3.2 Prompt Reference

The ACFS prompt library contains 41 named prompts. They are designed to be used verbatim -- copy-paste, not paraphrase. Jeff maps them to physical Stream Deck buttons for instant dispatch to agents via NTM.

> *"Basically they attend to every word, so every word is critically important. You want prompts to be short so that they don't overly constrain the models but they need to force the model to work hard to go beyond their default posture."*
> -- Jeffrey Emanuel

> *"Good prompting might seem like a hand wavy thing, but there's a lot that goes into making a prompt that performs well in many situations. I've been doing this stuff so much for so long that I've basically internalized the theory of mind of these models and the gestalt psychology."*
> -- Jeffrey Emanuel

---

### 3.2.1 The Dispatch Table

When you are in situation X, use prompt Y. This is the quick-lookup version.

**Planning Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Starting from scratch | First Principles | P01 |
| First draft exists, push quality higher | Praise Round 1 / 2 / 3 | P30 / P31 / P32 |
| Plan needs analytical critique | Plan Critique (Type A) | P04 |
| Multiple models produced competing plans | Multi-Model Synthesis (Type B) | P15 |
| Two models should compete on a problem | Dueling Wizards | P17 |
| Need a single transformative addition | Plan Innovation Boost | P26 |
| Need honest project assessment | Project Opinion Elicitor | -- |
| Integrating critique feedback | Integrate Critique | -- |
| Stress-test plan against failure | Premortem Planner | -- |

**Bead Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Converting plan to beads | Plan to Beads | P05 |
| Reviewing bead quality (repeat N times) | QA the Beads | P06 |
| Picking next bead to work on | Use BV | P24 |

**Execution Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Spawning a fresh agent | Agent Introduction | P18 |
| Ongoing bead execution | Execute Beads (main loop) | P08 |
| Agent mail check + continue | Anti-deadlock | P09 |
| After context compaction | Post-Compaction Refresh | P20 |
| Full autonomous push | Do All Of It | P25 |
| Committing code (commit agent only) | Git Commit | P11 |
| Onboarding to unfamiliar project | Deep Project Primer | P10 |

**Review Phase:**

| Situation | Prompt | ID |
|-----------|--------|----|
| After completing a bead | Post-Implementation Self-Review | P02b |
| Numbered review session (Part 1, 2, ... 23+) | Deep Review / Fresh Eyes Bug Hunt | P02 |
| Reviewing other agents' code | Cross-Agent Review | P03 |
| Reviews seem too comfortable | McCarthy Bug Hunt (escalation) | P27 |
| Need the model to care more | Stakes Escalation | P28 |
| Security-focused review | CVE-Inspired Security Testing | P29 |
| Hunting stubs and placeholders | Stub Eliminator | -- |
| Full codebase scan | UBS Scan | P21 |
| Root-cause bug fixing | Fix Bug | P22 |
| Random codebase exploration | Random Inspect | P23 |
| UI/UX polish | Stripe-Level UI | P12 |
| Comprehensive testing | E2E Pipeline Validator | P13 |
| UX audit | Audit UX | P19 |
| Post-deploy verification | Deployment Verifier | -- |

**Ideation and Meta:**

| Situation | Prompt | ID |
|-----------|--------|----|
| Generating improvement ideas | Idea Wizard (30 -> 5) | P16 |
| Need exceptional ideas only | 100-to-10 Filter | -- |
| Finding system weaknesses | System Weaknesses Analyzer | P14 |
| Performance optimization | Deep Performance Audit | -- |
| Building agent-friendly CLIs | CLI Error Tolerance | -- |
| File/folder restructuring | Code Reorganizer | -- |
| Updating documentation | README Reviser | -- |
| Cleaning AI-generated text | De-Slopifier | -- |
| Pre-integration dependency study | Dependency Analysis | -- |

---

### 3.2.2 Planning Prompts (P01, P04, P05, P15, P30-P32)

These prompts drive Phases 1-5: plan creation, refinement, alien artifacts, bead conversion, and bead QA.

#### P01 -- First Principles (prefix for everything)

```
Before acting, pause and think through this from first principles. What assumptions
might be wrong? What edge cases exist? What could fail? Consider multiple approaches
and their tradeoffs. Only proceed when you've thoroughly analyzed the problem space.
```

**Usage:** Prefix for any high-stakes decision. Appears in all 4 forensically analyzed repos (frankensqlite, rust_scriptbots, frankenterm, mcp_agent_mail_rust). Universal. No exceptions.

**Forensic frequency:** 10 confirmed uses across 5 repos. Appears in architecture decisions, plan bootstraps, and fix sessions.

---

#### P30 / P31 / P32 -- Praise Rounds (push past default quality)

**P30 (Round 1):**
```
That's a decent start but it barely scratches the surface and is light years away
from being OPTIMAL. Please try again and revise your existing plan document in-place
to make it MUCH, MUCH, MUCH better in EVERY WAY. Use ultrathink.
```

**P31 (Round 2):**
```
That's a lot better than before but STILL is a far cry from being OPTIMAL. Please
try again and revise your existing plan document in-place to make it MUCH, MUCH,
MUCH better in EVERY WAY. I believe in you, you can do this! Show me how brilliant
you really are! Use ultrathink.
```

**P32 (Round 3):**
```
OK this is getting really good now but I KNOW you can do even better. Dig deep.
Give me your ABSOLUTE BEST work. This is your chance to show the world what
frontier AI can produce. Use ultrathink.
```

**Why they work:** The model is not a static function. It responds to the emotional register of the conversation. Analytical critique refines; praise rounds expand. Skipping them is Anti-Pattern #5 -- "I'll just go straight to analytical critique" narrows the solution space before it has been explored.

**Forensic evidence:** P30-P32 sequence confirmed in frankensqlite (C1-C3), rust_scriptbots (H01-H02), frankenterm (F01). Always precedes analytical critique. Always runs in the same Claude conversation.

---

#### P04 -- Plan Critique (Round Type A, single-model)

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

**Usage:** After praise rounds. Run with a strong reasoning model (ChatGPT Pro with extended thinking, or Claude with extended thinking). The diff-based format forces concrete, integratable changes instead of vague advice. Can be automated with APR (`apr refine plan.md --rounds 10`).

**Forensic frequency:** 5 confirmed uses across all 4 repos. Universal planning pattern.

---

#### P15 -- Multi-Model Synthesis (Round Type B)

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

#### P05 -- Plan to Beads

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

**Usage:** Phase 4. Converts the locked plan into the execution graph. After this runs, PLAN.md is closed. The bead-map is the execution surface. Reopening the plan during execution is Anti-Pattern #7.

---

#### P06 -- QA the Beads (repeat N times)

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

#### Additional Planning Prompts

**P17 -- Dueling Wizards** (two-model competitive scoring):
```
I want two different frontier models to each independently propose their best
version of this. Score each proposal on: correctness, elegance, robustness,
completeness, and novelty. Then synthesize the winner into the plan.
```

**P26 -- Plan Innovation Boost:**
```
What is the single smartest addition we could make to this plan that would
dramatically improve the project? Not incremental. Transformative.
Use ultrathink.
```

**Integrate Critique** (fold feedback back into plan):
```
Integrate the following review feedback into <PLAN_FILE_PATH> in-place. For each
point of feedback, either incorporate it (with a git-diff style edit) or explicitly
reject it with a rationale. Keep the plan cohesive, consistent, and remove any
contradictions introduced by the integration. Use ultrathink.

<PASTE REVIEW FEEDBACK HERE>
```

**Usage:** After P04 (Plan Critique) or P15 (Multi-Model Synthesis) produces feedback, this prompt folds it back into the plan without destroying coherence. The key discipline: reject-with-rationale is a valid response to feedback. Not all critique is correct.

**Premortem Planner** (pre-beads risk check):
```
Before we proceed, I want you to do a "premortem" on this plan. Imagine we're
6 months in the future and this approach has completely failed. What went wrong?
What assumptions did we make that turned out to be false? What edge cases did we
miss? What integration issues did we overlook? What would users hate about it?
Now, with that pessimistic scenario fresh in your mind, revise the plan to address
the most likely failure modes.
```

**Project Opinion Elicitor** (honest assessment):
```
Now tell me what you actually THINK of the project -- is it even a good idea?
Is it useful? Is it well designed and architected? Pragmatic? What could we do
to make it more useful and compelling and intuitive/user-friendly to both humans
AND to AI coding agents?
```

---

### 3.2.3 Execution Prompts (P08, P09, P11, P18, P20, P25)

These prompts drive Phase 6: deploying coordinated agents to execute beads.

#### P08 -- Execute Beads (main loop)

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

**Usage:** The most-used prompt across all repos. 11 confirmed uses across 4 repos. This is the execution workhorse. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication.

---

#### P09 -- Agent Mail Check & Continue (anti-deadlock)

```
Check your agent mail and promptly respond if needed to any messages. Then proceed
meticulously with your next assigned beads. Don't get stuck in "communication
purgatory" where nothing is getting done; be proactive about starting tasks that
need to be done. Use ultrathink.
```

**Usage:** Jeff sends this via NTM when he sees an agent stalling. The key phrase is "communication purgatory" -- agents can endlessly discuss coordination without shipping code.

---

#### P18 -- Agent Introduction (fresh spawn)

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

**Usage:** Every new agent's first prompt. Handles self-onboarding: read AGENTS.md, understand the codebase, register with Agent Mail, check for messages, and start working.

---

#### P20 -- Post-Compaction Refresh

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

**Usage:** When an agent hits context compaction, it loses roughly half its context window. This prompt gives it the constitution back before it makes any decisions. Simple by design -- the agent just lost context, do not overload it.

---

#### P11 -- Git Commit (commit agent only)

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

**Usage:** The commit agent is a dedicated Claude Code instance that runs P11 continuously. It does not modify code. It reads the diff, groups changes into logical commits, writes detailed messages with bead IDs, and pushes. Doctrine #4: The commit agent is separate from coding agents.

> *"Look at my exact prompt. I explicitly tell the committer not to modify the code. That's why. It's a purely logistical thing."*
> -- Jeffrey Emanuel

---

#### P25 -- Do All Of It (full autonomous push)

```
I need you to do ALL of the remaining work. Every bead. Every test. Leave nothing
undone. Be thorough, meticulous, and autonomous. Use ultrathink.
```

**Usage:** When the bead count is low enough for one agent to finish. Confirmed in 5 uses across 4 repos. Jeff uses this for final pushes and overnight autonomous runs. In Codex (which supports message queuing), Jeff chains this as a multi-step pipeline entered up front: scrutinize the codebase, generate beads, QA beads in plan space, execute beads, fresh-eyes review, commit. Codex processes these one at a time. Claude Code does not support this workflow because follow-up messages interrupt the running agent.

> *"These are all entered up front and go into a queue of messages which Codex processes one at a time. Then you can come back 3+ hours later to see the incredible amount of work done autonomously for you."*
> — Jeffrey Emanuel

---

#### P10 -- Deep Project Primer (read-only onboarding)

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
```

**Usage:** Read-only onboarding. Unlike P18, this does not include Agent Mail registration or bead execution. Used when you want the agent to understand the codebase before giving it a specific task.

---

#### P24 -- Use BV (bead prioritization)

```
Use bv --robot-triage to identify the highest-impact actionable beads.
Pick the best one you can usefully work on and get started. Use ultrathink.
```

**Usage:** Directs the agent to use graph-theory prioritization instead of picking beads by intuition.

---

### 3.2.4 Review Prompts (P02, P02b, P03, P12, P13, P14, P19, P21-P23, P27-P29)

These prompts drive Phase 7: fresh-eyes review until diffs flatline.

#### P02 -- Deep Review / Fresh Eyes Bug Hunt

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

**Usage:** This is a numbered series, not a single event (Doctrine #5). Run as Part 1, Part 2, Part 3... Part 23 if needed. Each session gets a sequential ID. The prompt deliberately does NOT tell the model what to look for -- it asks the model to explore, understand, and apply judgment. Every prescriptive bug checklist is a tunnel that narrows the model's attention. P02 keeps it wide.

**Forensic frequency:** 9 confirmed uses across 4 repos. frankensqlite alone had 4 consecutive P02 commands (C11-C14). frankentui documents "Part 23" in FIXES_SUMMARY.md.

---

#### P02b -- Post-Implementation Self-Review

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

#### P03 -- Cross-Agent Review

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

**P27 -- McCarthy Bug Hunt:**
```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

**P28 -- Stakes Escalation:**
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

These change the model's internal prior. By default, models are optimistic reviewers. P27 and P28 make them paranoid. Confirmed in frankensqlite (C12-C13).

---

#### P29 -- CVE-Inspired Security Testing

```
Research recent CVEs relevant to the libraries and patterns in this project.
Create sandboxed tests that probe for similar vulnerabilities. Use ultrathink.
```

---

#### Quality and Testing Prompts

**P12 -- Stripe-Level UI:**
```
I want you to do a spectacular job building absolutely world-class UI/UX
components, with an intense focus on making the most visually appealing,
user-friendly, intuitive, slick, polished, "Stripe level" of quality UI/UX
possible for this that leverages the good libraries that are already part of
the project. Carefully consider desktop UI/UX and mobile UI/UX separately and
hyper-optimize for both. Use ultrathink.
```

**P13 -- E2E Pipeline Validator:**
```
We really need totally complete, totally comprehensive, granular, perfect end
to end testing coverage without ANY mocks or fake data, fake api calls, etc.,
that proves our entire pipeline from start to finish works perfectly in a
provable, ultra-rigorous way -- from "soup to nuts." Plus comprehensive unit
tests with detailed logging. Use ultrathink.
```

**P14 -- System Weaknesses Analyzer:**
```
Based on everything you've seen, what are the weakest/worst parts of the system?
What is most needing of fresh ideas and innovative/creative/clever improvements?
```

**P19 -- Audit UX:**
```
Scrutinize the UX of the entire project. Find every rough edge, confusing flow,
and unintuitive behavior. Fix them all. Use ultrathink.
```

**P21 -- UBS Scan:**
```
Run ubs . to scan the entire codebase. Analyze the results carefully and identify
any issues, improvements, or areas needing attention. Use ultrathink.
```

**P22 -- Fix Bug** (root-cause only):
```
Find the root cause of this bug. Don't patch symptoms. Understand why it happened,
fix the underlying issue, and verify the fix doesn't break anything else.
Use ultrathink.
```

**P23 -- Random Inspect:**
```
Pick 5 random files in the project you haven't looked at recently. Read them
carefully. Trace their execution flows. Find anything wrong. Fix it.
Use ultrathink.
```

**Stub Eliminator:**
```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

---

### 3.2.5 Recovery and Ideation Prompts (P16-P17, 100-to-10)

#### P16 -- Idea Wizard (30 -> 5)

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

#### 100-to-10 Filter (when quality matters most)

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

#### Deep Performance Audit (Phase 8)

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

### 3.2.6 Documentation and Tooling Prompts

**README Reviser** (present-tense docs):
```
Update the README and other documentation to reflect all of the recent changes
to the project. Frame all updates as if they were always present (i.e., don't
say "we added X" or "X is now Y" -- just describe the current state). Make sure
to add any new commands, options, or features that have been added.
```

**De-Slopifier** (remove AI writing tells):
```
Read through the complete text carefully and look for any telltale signs of
"AI slop" style writing; one big tell is the use of em dash. Replace with a
semicolon, a comma, or just recast the sentence. Also avoid: "It's not [just]
XYZ, it's ABC" or "Here's why" or "Here's why it matters:". Anything that sounds
like the kind of thing an LLM would write disproportionately more commonly than
a human. You MUST manually read each line and revise it -- no regex, no scripts.
```

**Code Reorganizer** (file restructuring):
```
Before making any changes, explore and read ALL of the many files in <DIR> and
understand what they do, how they fit together, which files import which others,
how they interact. Then propose a reorganization plan in a new document called
PROPOSED_CODE_FILE_REORGANIZATION_PLAN.md so I can review it before doing anything.
This plan should include your detailed reorganization plan, the super-detailed
rationale and justification for the proposed structure, and tracking of all import
changes needed so we don't break anything.
```

**CLI Error Tolerance** (FCBC -- For Clankers, By Clankers):
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

**Agent Feedback** (tool quality loop):
```
Rate this tool on a scale of 0-100 across these dimensions:
- Helpfulness (does it solve a real problem?)
- Signal-to-noise ratio (useful output vs. clutter)
- Strengths (what does it do well?)
- Weaknesses (what is frustrating or broken?)
- Errors encountered (with reproduction steps)
- Specific improvement suggestions (actionable, not vague)
- Recommendation strength (would you tell another agent to use it?)
```

**Usage:** Jeff runs this after agents have used a new tool for a full session. The output feeds directly into the next iteration of the tool. This is the FCBC feedback loop in practice: agents are the primary users of agent tools, so agent feedback is the primary signal for tool improvement.

**Dependency Analysis** (pre-integration due diligence):
```
Before integrating <DEPENDENCY>, write a COMPREHENSIVE_ANALYSIS_OF_<DEPENDENCY>.md.
Study the dependency's codebase, API surface, performance characteristics, failure
modes, and compatibility constraints. This must be done BEFORE any integration code.
```

**Deployment Verifier** (post-deploy check):
```
Deploy to <PLATFORM> and verify that the deployment worked properly without any
errors (iterate and fix if there were errors). Then visit the live site with
playwright as both desktop and mobile browser and take screenshots and check for
js errors and look at the screenshots for potential problems and iterate and fix
them all super carefully!
```

---

### 3.2.7 Adapting Prompts

The prompts are designed to be used verbatim. However, there are three valid adaptation patterns:

**1. Filling in placeholders.** Prompts containing `<PLAN_FILE_PATH>`, `<DEPENDENCY>`, `<PROJECT_NAME>`, etc. require substitution. This is not adaptation; it is parameterization.

**2. Chaining prompts.** Jeff chains P01 as a prefix before other prompts when the decision is high-stakes. P01 + P08 = "think from first principles, then execute the highest-priority bead." P01 + P04 = "think from first principles, then critique this plan."

**3. The emotional register.** The praise rounds (P30-P32) work because they shift the model's internal prior. You can adjust the intensity but should preserve the core mechanic: each round acknowledges improvement while insisting on more.

**What not to do:**
- Do not add role-play preambles ("You are an expert senior backend engineer"). Frontier models understand intent. Role-playing constraints narrow the solution space (Anti-Pattern #2 from the Doctrine).
- Do not enumerate everything the model should think about. Short prompts that force the model beyond its default posture beat long prompts that catalog concerns.
- Do not paraphrase. The prompts are calibrated. "Carefully fix anything you uncover" is not the same as "fix bugs." The word "carefully" changes the model's behavior.

---


---

!!! tip "Related pages on this site"

    - [Full Prompt Pack](../prompts/prompt-pack.md)
    - [Prompt Overview](../prompts/overview.md)

