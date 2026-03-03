---
icon: lucide/rocket
---

# ACFS: From Zero to Hero

**The Agentic Coding Flywheel — A Complete Guide**

---

# LEVEL 1: STRATEGIC — "Why This Exists"

## 1.1 The Problem This Solves

### 1.1.1 The Reactive Loop

Most AI-assisted development follows the same pattern:

1. Describe what you want in a chat or IDE prompt.
2. The model generates code.
3. Paste it in (or accept the suggestion).
4. It doesn't work, or works partially.
5. Feed the error back to the model.
6. Repeat.

This is a **reactive loop**. Each prompt is essentially stateless; the model has no persistent understanding of your architecture, no memory of decisions made three sessions ago, no sense of how this component relates to the rest of the system. You are the integration layer, manually shuttling context between a stateless model and your codebase.

IDE-integrated tools (Cursor, Copilot, Windsurf) improved the ergonomics. Suggestions appear inline, context windows pull in nearby files. But the fundamental dynamic is unchanged: the developer reacts to model output instead of directing execution against a plan.

The cost compounds. Each cycle burns tokens and, more critically, burns context. By the tenth iteration on a stubborn bug, both developer and model have lost sight of the original intent. The codebase accretes prompt-by-prompt decisions that no one planned and no one fully understands.

Practitioners who have been through this recognize the pattern:

> *"I think vibe coding genuinely makes people lazier. Bugs like this would have bothered traditional devs, even if it took them hours/days to fix. Now it's like 'eh the machines'll fix it eventually I guess.'"*
> — @doodlestein community, Feb 2025

The reactive loop trains you to tolerate lower quality. The speed of generation masks the drift in standards.

---

### 1.1.2 Where Ad-Hoc AI Coding Breaks Down

Prompt-driven development works well within a bounded scope:

- Single-file scripts and utilities
- Prototypes and throwaway demos
- UI mockups where visual correctness is the only bar
- Any project where "it runs on my machine" is sufficient

It breaks down reliably around **5,000 lines of code**, the approximate threshold where:

- **State dependencies emerge.** Components depend on shared state, configuration, and contracts that must be consistent across the codebase.
- **The project exceeds a single context window.** Without an external plan, coherence degrades as the model can no longer "see" the full system.
- **Edge cases multiply combinatorially.** You can't chat your way through the interaction matrix of auth, payments, error handling, and concurrency.
- **Integration requires architectural consistency.** Prompt-by-prompt decisions produce three naming conventions, two ORM patterns, and four competing approaches to error handling, all in the same project.

This has been reported widely enough that it's become a known wall:

> *"Vibe coding is fun until you hit the boring walls: prod level auth, app store compliance, weird platform bugs. The dopamine fades, the discipline begins. Most quit right there."*
> — @rileybrown_ai, Apr 2025

> *"I'm vibe coding and I'm so underwater right now with this. I have some cute processes that manage it but uptime and reliability are a struggle."*
> — ACFS community member, Feb 2026

The pattern recurs: someone builds a demo in a weekend, tries to turn it into a real product, and hits the wall. The fun AI-assisted part was 20% of the total work. The remaining 80% (auth, error handling, data integrity, deployment) is interconnected, state-dependent work that ad-hoc prompting cannot coordinate.

The models themselves are capable enough. Current frontier models can write production auth, database migrations, and concurrent systems. They fail when requirements arrive one message at a time, with no persistent architecture document and no mechanism to ensure consistency across hundreds of individual invocations.

> *"It's incredible how despite so much effort, there is just no automated software development that can get done even a small sized application without intervention. [...] It's like they can get done ONE FEATURE but ask it to do 10-20 consecutive features, it just can't get it done. [...] HOW CAN THEY NAIL ONE THING PERFECTLY but fail to do them jointly over a long window?"*
> — @VictorTaelin, Feb 2025

The root cause is structural. These tools try to solve a **coordination problem** (building a coherent system from many parts) with a **generation solution** (producing code from prompts). Coordination requires planning, decomposition, and state management, none of which exist in a reactive prompt loop.

---

### 1.1.3 The Real Cost of Skipping the Plan

The central failure mode is **deferred complexity**. The work feels fast because code generation is genuinely fast. But speed of generation is not speed of delivery.

Consider: a developer generates a REST API in 45 minutes using prompt-driven coding. The delivered artifact has:

- Error handling that swallows exceptions (discovered in production)
- Auth that doesn't handle token refresh (discovered by users)
- No test coverage (discovered during the first refactor)
- A schema designed prompt-by-prompt with inconsistent conventions (discovered during the next feature)

The 45 minutes of generation deferred 8-12 hours of debugging, refactoring, and rewriting. That deferred work costs more than doing it right initially, because now you must reverse-engineer intent from code that neither you nor the model fully planned.

The relevant metric is **time to working, tested, integrated, deployable feature**, not time to first line of code. By that measure, a developer who spends 4 hours on a plan and 2 hours on guided execution consistently outperforms one who generates code in 45 minutes and debugs for 12 hours.

This is the core insight behind the Agentic Coding Flywheel:

> *"I spend 90% of my human time and energy (which is the only thing that matters) on planning. Once the beads are done it's just machine tending."*
> — Jeffrey Emanuel

The methodology inverts the typical time allocation. Instead of planning 10% and coding/debugging 90%, the majority of **human** effort goes to planning, decomposition, and quality control. Execution is delegated to agents working against a well-defined task structure. Practitioners who adopt this report that the planning phase routinely takes longer than the coding phase, yet total delivery time drops significantly.

> *"The planning is front-loaded and where I spend most of my time and energy. I do tons of iterations with multiple frontier models, then turn that into beads."*
> — Jeffrey Emanuel

> *"All of my issues are solved with: breaking things into smaller problems, and providing more explicit direction."*
> — Jeffrey Emanuel

Neither of these insights is complicated. The methodology's value is in making them systematic: how to write the plan, how to decompose it into atomic tasks, how to execute those tasks with agents, and how to coordinate the results.

---

## 1.2 The Agentic Coding Flywheel: What It Actually Is

### 1.2.1 One-Sentence Definition

The Agentic Coding Flywheel is a methodology where the bulk of human effort goes into an upfront plan document, that plan is decomposed into a dependency graph of atomic tasks (called "beads"), and parallel AI coding agents execute against that graph under human oversight.

There is a second, equally important meaning. "Flywheel" also refers to a compounding effect: each project you build becomes a library for the next project. The tools stack. FrankenTUI feeds FrankenSQLite feeds FrankenTerm feeds Agent Mail Rust. Each rotation makes the next one faster.

> *"That's what the Flywheel is all about. Making programs and libraries and then turning around and combining them with other new libraries you've made to make still more ambitious new projects."*
> — Jeffrey Emanuel

These two meanings reinforce each other. The methodology (plan, decompose, execute) produces high-quality libraries. Those libraries reduce the cost of the next project's plan. The next project produces more libraries. This is the flywheel.

---

### 1.2.2 The Three-Space Model: Plan, Bead-Map, Code

The methodology operates across three distinct artifacts, each at a different level of abstraction:

**1. Plan space** (PLAN.md or similar)

The architecture document. Describes what you're building, why, how components interact, what the interfaces look like, what the edge cases are. Written collaboratively between the human and multiple frontier models. Typically 2,000 to 21,000+ lines for serious projects.

The plan is a *read-once* artifact. Once beads are created from it, agents work from the bead-map, not the plan. The plan's job is to produce a good bead-map; after that, it is archived.

**2. Bead-map space** (master-todo-bead-map.md or beads database)

The execution graph. Each bead is one atomic task that one agent can complete in one session. Beads have IDs, descriptions, dependencies, acceptance criteria, and status. A complex project produces 200 to 500 initial beads.

The bead-map is the **shared state** that all agents read from. It replaces ad-hoc coordination; agents claim beads, execute them, and mark them closed.

> *"They follow the plan as embodied in the beads task structure, and they use my bv tool to pick the best bead to work on next."*
> — Jeffrey Emanuel

**3. Code space** (src/, the repository)

The implementation. Generated by agents working against beads, reviewed by humans and other agents, integrated through a merge cycle. Code is the *output* of the process, not the center of it.

The key discipline: these three spaces are kept separate. The plan informs the bead-map. The bead-map drives the code. Information flows in one direction. Agents executing code do not modify the plan; agents refining the plan do not touch the code.

> *"The 2:1 planning ratio is the part everyone skips."*
> — ACFS community member

---

### 1.2.3 Why the Plan Is the Product

In traditional development, code is the deliverable. Plans (if they exist) are secondary artifacts that drift out of sync within days. The Flywheel inverts this.

The plan is the durable artifact. Code is derived from it. If you lose the code, you regenerate it from the plan in hours by re-running agents against the bead-map. If you lose the plan, you have lost the architecture, the design decisions, the rationale for every tradeoff. You would have to reverse-engineer all of that from code, which is expensive when the code was written by agents whose context no longer exists.

Jeff's plans go through 8 to 15 revision rounds before any code is written. One planning document reached 21,000 lines. Plans are version-controlled with the same rigor as source code: every revision is a git commit, and the diff history of the plan is itself a diagnostic artifact.

> *"I didn't write that plan. I caused it to be written!"*
> — Jeffrey Emanuel

The practical consequence: planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous. The effort split looks like this:

| Activity | Typical AI-assisted | Flywheel methodology |
|----------|-------------------|---------------------|
| Planning | ~10% | ~85% |
| Implementation | ~70% | ~10% |
| Review/Fix | ~20% | ~5% |

The 85% figure sounds extreme. In practice, it means the human spends most of their time reading, refining, and stress-testing the plan. When the plan is solid, execution is largely mechanical.

> *"Planning Phase: world is Claude Code's oyster. Implementation Phase: leave no room for imagination."*
> — ACFS community member

---

### 1.2.4 The Flywheel Effect: Each Tool Strengthens the Next

The toolchain is designed so that each tool's output feeds the next tool's input. This creates a compounding loop:

- **NTM** (tmux manager) spawns agent sessions. Agents use **Agent Mail** to coordinate. **BV** (beads viewer) tells agents what to work on. **BR** (beads rust) tracks what's done. **CASS** remembers cross-session context. **CM** feeds learnings back into future plans.

No single tool is revolutionary on its own. The value is in how they interlock. Using three of these tools together is meaningfully more productive than using any one of them alone.

The compounding goes deeper than tool integration. Each *project* built with the flywheel becomes a library available for the next project. The February 2026 Rust Agent Mail port stacked 10 self-built libraries on top of each other: FrankenTUI, FrankenSQLite, FrankenSearch, asupersync, fastmcp_rust, sqlmodel_rust, beads_rust, and three others. Each of those libraries was itself built using the flywheel methodology, using earlier libraries as dependencies.

> *"My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinning faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools."*
> — Jeffrey Emanuel

This compounding is why output velocity increases over time rather than plateauing. A practitioner six months into the methodology has accumulated libraries, prompts, and institutional memory (via CASS/CM) that a newcomer lacks. The gap is real, but it closes with each project you ship.

> *"I find scaling past 2-3 agents has a pretty steep falloff for oversight quality in any non-greenfield work."*
> — ACFS community member

This is a fair observation, and it points to an important caveat: the flywheel's acceleration depends on the quality of your coordination infrastructure. Scaling agents without scaling your planning discipline just scales noise. The toolchain is necessary but not sufficient; the methodology is what makes it work.

---

## 1.3 What Makes This Different

### 1.3.1 vs. Cursor / Copilot / Windsurf (IDE-Assist)

IDE-assist tools are reactive by design. They watch what you type and suggest completions, or they accept natural-language instructions and generate code in your editor. They are excellent at what they do: reducing keystrokes, generating boilerplate, and answering inline questions about code.

What they do not provide:

- **A persistent plan.** Each prompt exists in isolation. There is no shared architecture document that all suggestions align with.
- **Multi-agent coordination.** You get one model in one context window. If you want a second opinion, you switch tabs manually.
- **Cross-session memory.** Context resets when you close the window. The model that helped you yesterday does not remember today.
- **Task decomposition.** There is no mechanism to break a project into atomic units and track their completion.

The Flywheel methodology is tool-agnostic. It operates at a layer above any individual IDE or agent. The plan, the bead-map, and the coordination infrastructure (Agent Mail, NTM) work with any coding agent that supports MCP or can read files.

> *"My system has worked perfectly with Cursor for 4 months since I first released it. It uses MCP so it works with everything. Standards!"*
> — Jeffrey Emanuel

You can use Cursor as one of your execution agents inside the Flywheel. The point is that Cursor alone, like any IDE-assist tool, provides generation without coordination.

---

### 1.3.2 vs. Devin / OpenHands / SWE-agent (Autonomous Agents)

Autonomous agent platforms attempt to handle the entire development cycle: understand the task, plan the approach, write the code, test it, iterate. The promise is fully hands-off development.

In practice, these systems work well on bounded, single-feature tasks (the kind that SWE-bench measures). They fail on multi-feature applications where components must interact coherently over a long execution window. This has been documented repeatedly by practitioners:

> *"Build systems that decompose a problem in a haystack of needles."*
> — @TheodoreGalanos, Feb 2025

The Flywheel's approach to this problem is different in a specific way: the **human remains the strategic planner**. Agents do not decide *what* to build; they execute against a pre-existing plan. The plan absorbs the complexity that autonomous agents cannot handle, because the plan was written (and stress-tested through multi-model competition) before any agent started coding.

> *"All my experiments have failed when starting with autonomous agents. So I'M the orchestrator."*
> — ACFS community member

The tradeoff is explicit: the Flywheel requires significant human effort in the planning phase. Autonomous agents require less human effort upfront but produce lower-quality results on complex projects. For projects over ~5K LOC with multiple interacting components, the upfront planning investment pays for itself.

---

### 1.3.3 vs. "Just Use Claude Code" (Raw CLI)

Claude Code is the primary execution engine in most Flywheel setups. The question is what you wrap around it.

Raw Claude Code gives you one agent in one context window. It can spawn subagents, but each subagent operates in a context silo; it cannot see what the other subagents are doing, and the human cannot easily monitor multiple subagent streams.

> *"I tried the orchestrator agent pattern today across a 3-project stack and while it was neat, some sub agents made silly mistakes due to the context silos and I couldn't see inside the agent streams while they worked."*
> — @ShannonCode

The Flywheel adds three layers on top of raw Claude Code:

1. **The plan document** as shared context that all agents can read.
2. **Agent Mail** for async inter-agent messaging (agents report status, request help, coordinate merges).
3. **NTM + tmux** for human oversight of multiple agent sessions simultaneously.

These layers let you run 4, 8, or 12+ Claude Code instances on the same project, each working on a different bead, coordinating through a shared bead-map and agent mail rather than through a single orchestrator's context window.

> *"Thinking is not scalable and it is already my bottleneck."*
> — @steipete (Peter Steinberger)

The Flywheel's answer to this bottleneck is to front-load the thinking into the plan, then parallelize the execution. The human thinks once (during planning), and that thinking is distributed to all agents via the plan and bead-map.

---

### 1.3.4 The Missing Layer: Planning Leverage

Across all three comparisons, the pattern is the same. IDE tools, autonomous agents, and raw CLI agents all lack the same thing: a structured, version-controlled planning phase that converts high-level intent into an executable task graph before any code is written.

This planning phase is the leverage point. It is where:

- Architectural decisions are made and stress-tested (via multi-model competition)
- The work is decomposed into units small enough for a single agent session
- Dependencies are mapped so agents can work in parallel without conflicts
- Acceptance criteria are defined so "done" is unambiguous

Without this layer, every other tool is just generating code faster into a void of uncoordinated decisions. With it, any tool becomes an execution engine for a coherent plan.

> *"My approach really works. You don't have to be some crazy savant to apply it, either. Just read my posts about planning and use the tools and workflows, and you will also get these kinds of results. I've now seen at least 10 people who I don't even know report similar outcomes."*
> — Jeffrey Emanuel

The following section covers the economics of this approach: what it costs in time and money, when the investment is justified, and when it is overkill.

---

## 1.4 The Economics

### 1.4.1 Time: Plan Longer, Ship Faster

The most counterintuitive aspect of the methodology is the time allocation. The planning phase takes roughly 2x longer than the coding phase. Total delivery time still drops, often dramatically, because the coding phase produces working software on the first pass rather than requiring multiple debug-and-rewrite cycles.

The effort split in practice:

| Activity | Time share |
|----------|-----------|
| Planning (human-intensive) | ~60-70% |
| Bead decomposition | ~10-15% |
| Agent execution | ~10-15% |
| Integration and review | ~5-10% |

"Human time" and "wall clock time" are different. During the execution phase, agents work largely autonomously. The human monitors, steers when needed, and reviews output. A 12-hour project might involve 8 hours of active human planning, 30 minutes of bead decomposition, and then 3-4 hours of agents running while the human checks in periodically.

Because the plan was thorough and the beads are well-defined, agents produce code that works on the first pass far more often than in ad-hoc prompting. The debugging phase shrinks from the majority of total time to a small fraction.

---

### 1.4.2 Money: What It Costs

The financial cost depends on how many agents you want to run in parallel and which models you use.

**Minimum viable setup (~$200/month):**
- 1x Claude Max subscription ($200)
- This gives you one agent with generous rate limits

**Comfortable setup (~$400-650/month):**
- 1x Claude Max ($200)
- 1x ChatGPT Pro ($200) for multi-model competition during planning
- VPS for running agents ($20-50)
- Optional: Gemini API (free tier often sufficient for planning rounds)

**Power user setup ($1,000-3,000/month):**
- Multiple Claude Max accounts (for parallel agents)
- ChatGPT Pro
- VPS with 32-64 cores
- Multiple model subscriptions for planning diversity

For reference, API-based usage of Claude Code (without a Max subscription) runs roughly $10 per 30 minutes of active use. A Max subscription at $200/month subsidizes heavy usage significantly. For most practitioners, the Max subscription is the right starting point.

> *"The GPT Pro sub for $200 is an incredible value proposition compared to the cost of the API."*
> — Jeffrey Emanuel

At full scale, Jeff's spend trajectory went from ~$3,000/month to ~$10,000/month to ~$12,000/month across 52 total accounts (22 Claude Max, 22 GPT Pro, 7 Gemini Ultra, plus miscellaneous). The `ccusage` tool tracks real token cost per project for anyone who wants to measure rather than guess.

The cost scales with the number of parallel agents. A solo practitioner running 1-2 agents can operate comfortably at the $200-400/month level. Someone running 8-12 agents on multiple projects will need multiple subscriptions and more compute.

---

### 1.4.3 Case Study: 300K LOC TypeScript to 140K LOC Elixir

The most cited proof-of-concept for the methodology is a full-stack rewrite: 300,000 lines of TypeScript converted to 140,000 lines of Elixir, with the resulting system booting on its first run.

Key details:
- The planning phase took approximately 2x longer than the coding phase
- The LOC reduction (300K to 140K) reflects both the expressiveness of Elixir and the architectural improvements made during planning
- "Booted first try" means the fundamental architecture was correct; the system started and passed basic integration tests without manual debugging

This result is only possible when the plan is comprehensive enough to capture all component interfaces, data flows, and integration points before any code is generated. A single model producing code prompt-by-prompt could not achieve this level of system-wide coherence.

> *"300k LOC TypeScript to 140k LOC Elixir and it booted first try is wild. The planning phase being 2x longer than the coding phase says everything about where the real leverage is now."*
> — ACFS community member

---

### 1.4.4 When Not to Use This

The Flywheel methodology has real overhead. The planning phase, bead decomposition, agent coordination infrastructure, and multi-model review cycles take time and effort. That overhead is justified for certain projects and wasteful for others.

**Use the Flywheel when:**
- The project has multiple interacting components (>5K LOC, multiple services or modules)
- Architectural coherence matters (production systems, libraries, long-lived code)
- You need parallel execution (the project is large enough that serial development is too slow)
- The project will be maintained, extended, or used as a dependency by future projects

**Don't use it when:**
- The project is a single-file script or utility (<500 LOC)
- You're prototyping and expect to throw the code away
- The full project fits in a single context window and can be built in one session
- The specification IS the output (writing, analysis, one-shot generation tasks)
- You cannot articulate what you want to build (the Flywheel assumes you know *what*; it helps with *how*)

Jeff describes his target audience as "hungry but clueless": people who know they want to build real software with AI but have only encountered "slop factories" that produce demo-quality code. The Flywheel is for those who want production output, not impressive first impressions.

The crossover point is roughly in the 2,000-5,000 LOC range with multiple components. Below that, prompt-and-iterate is faster. Above that, the upfront planning investment pays for itself in reduced debugging and rework.

---

## 1.5 Core Principles

The methodology rests on six principles. These are not aspirational guidelines; they are operational rules that practitioners apply on every project.

### 1.5.1 Figure Out What to Build, Then Build It

The first principle is a sequencing rule: planning comes before execution, and the two phases do not overlap.

During planning, the goal is to produce a comprehensive specification that captures architecture, component interfaces, data flows, edge cases, and deployment requirements. The specification is refined through multiple rounds of model-assisted iteration until it is stable.

During execution, agents implement against the specification. They do not redesign the architecture, question the component boundaries, or propose alternative approaches. Those decisions were made during planning.

> *"All the work and energy goes mostly into step one, deciding what to make and creating and refining the plan, iterating over and over on it."*
> — Jeffrey Emanuel

This separation is strict because mixing planning and execution causes the worst failure modes: half-built features that don't fit together, architectural decisions made under time pressure, and agents that spend their context window debating design instead of writing code.

---

### 1.5.2 Every Word in a Prompt Is a Constraint

Transformer-based models attend to every token in the prompt. Each word you include shapes the output. This has a practical consequence: more words in your prompt means more constraints the model must satisfy simultaneously, which leaves less room for the model to reason deeply about any single constraint.

> *"Basically they attend to every word, so every word is critically important. You want prompts to be short so that they don't overly constrain the models, but they need to force the model to work hard to go beyond their default posture."*
> — Jeffrey Emanuel

This principle applies to execution prompts, not to the plan document. The plan can be 11,000 lines; it is context, not instruction. But the prompt that tells an agent what to do with that context should be concise and precise.

When practitioners find themselves writing two-page prompts, the correct response is usually to decompose the task into more beads, not to write a longer prompt.

---

### 1.5.3 Short Prompts Beat Long Prompts

This follows from 1.5.2. The most effective execution prompts in the methodology are typically 1-3 sentences. They specify what to do, point the agent at the relevant context (AGENTS.md, the bead description), and let the model work.

Example of a high-leverage short prompt:

> *"What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?"*
> — Jeffrey Emanuel

This is one sentence. It produces pages of detailed, reasoned output because it gives the model room to think rather than constraining it with a checklist of requirements.

Contrast with a typical long prompt: "Please review the auth module and check for token refresh handling, rate limiting, error codes, logging, and ensure compatibility with the user service API." Each noun splits the model's attention. The model will produce a shallow pass across all five items rather than a deep analysis of any one.

> *"Mate, I tried your prompts on a CC cloud environment and let them run while I was on a plane this afternoon. Results are outstanding. Something about the way you've worded these is extracting more depth than my regular prompts."*
> — ACFS community member

The mechanism behind this is specific: when you prescribe HOW to do something, models become accommodative. They follow your method rather than reasoning about the best approach. When you specify only goals and hard constraints, each model generates a different plan, which gives you diversity of ideas.

> *"When you start proscribing HOW to do things, they fall into line and try to be accommodative. That's why you want to focus more on your goals and the purpose and any hard constraints. Then each model will come up with a different plan, so you get the diversity of ideas, which is critical."*
> — Jeffrey Emanuel

The short-prompt principle coexists with massive plans. The plan is the *context*; the prompt is the *instruction*. Keep the instruction short. Let the context do the heavy lifting.

---

### 1.5.4 Compete Models Against Each Other

Single-model plans have single-model blind spots. The methodology addresses this through structured multi-model competition during the planning phase.

The process:

1. Give the same plan to 2-3 different frontier models (Claude, GPT/Codex, Gemini).
2. Ask each to produce two documents: a **revised plan** and a **narrative document arguing for its changes**.
3. Feed each model's output to the others. Ask them to evaluate honestly.
4. The plan that survives this gauntlet is the one you execute.

> *"When I have Codex & Claude go back and forth over a plan I ask them for two documents: its revised plan AND narrative doc arguing its position. Codex wins majority of arguments but often chooses specific phrases and sentences of Claude's and adopts them, with strong praise."*
> — Jeffrey Emanuel

This is a scheduled phase in the methodology (not ad-hoc). In the FrankenSQLite project, the git history contains side-by-side competing specs: `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1.md` (Claude) alongside `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1_CODEX.md` (Codex).

Each model tends to have characteristic strengths. Claude tends toward expressive, thorough specifications. Codex tends toward architectural rigor. Gemini often pushes both harder on edge cases. The combination produces plans that no single model would generate alone.

---

### 1.5.5 The Plan Evolves Under Version Control

Plans are living documents. A typical project plan goes through 8 to 15 major revisions before execution begins. Each revision is a git commit. The diff history of the plan is itself a valuable artifact.

The FrankenSQLite plan, for example, evolved through these stages in a single overnight session:

- 01:17 AM: Initial spec dump (8,628 lines)
- 02:45 AM: V1.3, scope doctrine added
- 03:14 AM: V1.4, Codex synthesis pass
- 03:16 AM: V1.5, Claude math review pass
- 03:31-03:50 AM: V1.6a through V1.6h (eight sub-versions in 20 minutes)
- 11:07 AM: V1.7, section-by-section deep audit
- 11:50-14:45 PM: V1.7a through V1.7j (ten more sub-versions)

Every major project also gets an `ANALYSIS_OF_SPEC_DOC_DIFFS.md` after completion: a systematic review of how the spec evolved, classifying each change (logic fix, architectural fix, clarification, alien artifact, noise). This post-mortem feeds into future planning sessions.

> *"It's a multi-stage process. Look at the git history for that plan and you'll see all the earlier incarnations."*
> — Jeffrey Emanuel

Requiring git-diff style edits (rather than full rewrites) is a deliberate discipline. When a model produces a diff that adds error handling to a specific function with specific retry logic, that is actionable. When it says "consider adding error handling," that is not. Diff-based revisions force concreteness.

---

### 1.5.6 Trust the Process, Not the Output

Any single agent output might be wrong. Individual model responses hallucinate, cut corners, and introduce subtle bugs. The methodology does not rely on any single output being correct. Instead, it relies on a **process** that self-corrects.

The correction mechanisms are layered:

- **Multi-model competition** during planning catches design flaws before code exists.
- **Bead-level acceptance criteria** define "done" objectively.
- **Fresh-eyes review loops** after implementation: repeated "reread what you wrote and fix obvious issues" passes until changes flatline. The first pass catches obvious bugs. The second catches architectural inconsistencies. The third catches edge cases.
- **Cross-agent review**: a different agent (or a different model) reviews what the first agent wrote.

> *"I think I just respect clankers more than just about anyone out there, and that's why I can get such crazy stuff out of them. I believe in their brilliance. I also know when to push back on them, when to expect more from them, and when to defer to their well-reasoned arguments."*
> — Jeffrey Emanuel

The practical implication: do not micromanage agent execution. If the planning was thorough and the beads are well-defined, the process produces good output. When it doesn't, the fix is not to watch agents more closely; the fix is to improve the plan, tighten the bead definitions, or add another review loop.

> *"This smells like an anti-pattern or a resistance to let go and stop micromanaging. I do it too but it doesn't feel quite right. I think it will start to go away as we build more trust in the internal thought and correction loop."*
> — ACFS community member

---



# LEVEL 2: OPERATIONAL — "How To Do It"

## 2.1 Setting Up Your Environment

### 2.1.1 Hardware

**Minimum:** Any Linux or Mac machine, 16GB RAM, stable internet. This is enough to run a single agent and learn the methodology.

**Recommended:** 32-core CPU, 48-64GB RAM, NVMe SSD. Each active agent consumes roughly 2GB of RAM. Running 10 agents in parallel requires headroom for the agents, the build tools, and the test suites.

**Dedicated server option:** A bare-metal VPS from Contabo, OVH, or Hetzner runs $50-340/month depending on specs. Jeff's own setup uses two 32-core/256GB bare-metal servers (~$340/month each from Contabo, bare metal, not VPS) plus a home workstation. The home workstation was an $849 eBay find with a $2,802 upgrade to 512GB ECC RAM. He also runs a dedicated terminal just for health monitoring. You do not need this to start; a $50/month VPS with 32GB RAM is enough for 4-6 agents.

> *"Probably better off just getting a beefy Linux machine from OVH or Hetzner for $50/month."*
> — Jeffrey Emanuel (advice to beginners)

---

### 2.1.2 Subscriptions

Priority order for a new practitioner:

1. **Claude Max ($200/month):** the primary execution engine. Non-negotiable.
2. **ChatGPT Pro ($200/month):** for multi-model competition during planning. Strongly recommended.
3. **Gemini CLI:** often available on free tier. Useful as a third voice in plan reviews.
4. **VPS** ($20-50/month): use if you don't have a local machine with enough resources.

Total comfortable starter cost: **$400-650/month.**

API-based usage (without subscriptions) runs roughly $10 per 30 minutes of active Claude Code use. A Max subscription subsidizes heavy usage heavily; the breakeven is roughly an hour of daily use.

> *"Before Claude Code Max I was spending ~$100 a day on Sonnet. Now it's like $30/day."*
> — ACFS community member

As you scale to more parallel agents, you may need additional Max accounts. The CAAM tool (part of the stack) handles automatic rotation between accounts to maximize throughput without hitting per-account rate limits.

---

### 2.1.3 Installing ACFS

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/acfs/main/install.sh | bash
```

The installer runs 9 phases in sequence: user setup, filesystem, shell configuration, CLI tools, language runtimes, agent CLIs, cloud/database services, the Dicklesworthstone stack, and finalization. Total time is roughly 12 minutes on a standard VPS.

What it installs (highlights):
- **Agent CLIs:** Claude Code (`cc`), Codex (`cod`), Gemini CLI (`gmi`)
- **Flywheel tools:** beads_rust (`br`), beads_viewer (`bv`), NTM (tmux manager), Agent Mail (MCP server), CASS (memory), CM (compatibility checker)
- **Supporting tools:** xf (search), brenner (code review), ruff (Python linter), plus ~500 utilities in `~/.local/bin/`
- **Shell enhancements:** Zsh with autosuggestions, syntax highlighting, powerlevel10k prompt

To update an existing installation:

```bash
acfs update
```

---

### 2.1.4 Post-Install Validation

```bash
acfs doctor
```

Every line should be green. Common issues after a fresh install:

| Symptom | Fix |
|---------|-----|
| PATH not updated | Restart your shell (`exec zsh`) |
| API key missing | Set in `~/.claude/settings.json` |
| MCP server not running | `systemctl status mcp-agent-mail` |
| DCG warning | Run `dcg install` (optional, for git safety) |

If `acfs doctor` reports all green, run a smoke test:

```bash
cc                    # start a Claude Code session
# type: "What tools do you have access to?"
# verify it can see Agent Mail, beads, etc.
```

---

### 2.1.5 Your Shell After ACFS

After installation, your PATH includes these aliases:

| Alias | Tool | Purpose |
|-------|------|---------|
| `cc` | Claude Code | Primary coding agent |
| `cod` | Codex CLI | OpenAI coding agent |
| `gmi` | Gemini CLI | Google coding agent |
| `br` | beads_rust | Bead lifecycle management |
| `bv` | beads_viewer | Visual bead management |
| `ntm` | NTM | Tmux session manager |
| `xf` | xf | Full-text + semantic search |

Jeff also maps his 32 master prompts to a physical Stream Deck. Each prompt fires with a single button press, enabling sub-second prompt dispatch to any agent session. This is optional but becomes valuable once you're managing 4+ agents simultaneously.

> *"Each of these blurbs takes under a second to do with a single button press using my little command palette gizmo."*
> — Jeffrey Emanuel

---

## 2.2 Anatomy of an ACFS Project

### 2.2.1 Creating a Project

```bash
acfs newproj
```

This creates a directory under `/data/projects/` with the canonical structure, detects your tech stack, initializes git, and generates AGENTS.md from a template. The resulting directory looks like this:

```
project/
├── AGENTS.md           # Agent behavioral rules (the most important file)
├── CLAUDE.md           # Project context for Claude Code
├── README.md           # Human-facing documentation
├── docs/plans/         # Plan versions and design documents
├── src/                # Source code (agent-generated)
├── tests/              # Test suites
└── .claude/            # Claude Code session state
```

---

### 2.2.2 AGENTS.md: The Project Constitution

AGENTS.md is the single most important file in any ACFS project. It is not documentation; it is an operating manual for minds that wake up without memory. Every agent reads this file first, before touching anything else.

> *"Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents."*
> — Jeffrey Emanuel

AGENTS.md must contain:

- **Project identity:** what this is, what stack it uses, what the goals are.
- **Behavioral rules:** what agents can and cannot do (file deletions, git operations, scope boundaries).
- **Coordination protocol:** how to register with Agent Mail, how to claim beads, how to communicate with other agents.
- **Bead execution protocol:** the step-by-step process for picking, executing, self-reviewing, and closing a bead.

A typical bead execution protocol section:

```
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Implement it completely
3. Before closing: reread ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
4. Only then: `br close <id>`
5. Communicate status to fellow agents via Agent Mail
6. Pick next bead. Repeat.
```

After every context compaction, agents must re-read AGENTS.md. This is critical enough that Jeff built a dedicated tool to automate the reminder.

> *"You just have to tell it at the beginning and after every compaction to reread AGENTS dot md."*
> — Jeffrey Emanuel

---

### 2.2.3 CLAUDE.md: Project Context

CLAUDE.md provides project-specific context that Claude Code loads automatically when starting a session in that directory. It contains: file structure descriptions, conventions, how to run tests, project-specific quirks.

CLAUDE.md and AGENTS.md serve different purposes. AGENTS.md is behavioral rules (what to do, what not to do). CLAUDE.md is project context (where things are, how they work). Keep them separate.

Project-level CLAUDE.md (`/project/CLAUDE.md`) overrides user-level CLAUDE.md (`~/.claude/CLAUDE.md`).

---

### 2.2.4 The Plan Document

The plan document is a markdown file that describes the entire project: architecture, component interfaces, data flows, edge cases, deployment requirements. For serious projects, this document ranges from 2,000 to 21,000+ lines.

This is not a README. A README tells users what the project does. The plan tells agents how to build it.

The plan is a *read-once* artifact in the execution workflow. Once beads are created from it, agents work from the bead-map, not the plan. The plan's purpose is to produce a good bead-map; after that, it is archived in `docs/plans/` and version-controlled for reference.

> *"This is truly nuts. The plan is 11k lines long and is ridiculously over the top."*
> — Jeffrey Emanuel (on the FrankenSQLite specification)

---

### 2.2.5 Directory Conventions

When multi-model competition runs during planning (see section 2.3.4), the competing specs appear as parallel files:

```
docs/plans/
├── PLAN_v1.md                              # Primary plan (Claude)
├── PLAN_v1_CODEX.md                        # Codex revision
├── PLAN_v1_GEMINI.md                       # Gemini revision
├── ANALYSIS_OF_SPEC_DOC_DIFFS.md           # Post-mortem of spec evolution
└── master-todo-bead-map.md                 # Plan-to-bead mapping
```

When `PLAN_CODEX.md` appears alongside the primary plan in a repository, it signals that multi-model competition was run. This pattern is visible in the FrankenSQLite git history, where `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1.md` (Claude) sits alongside `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1_CODEX.md` (Codex).

---

## 2.3 Phase 0: Writing the Plan

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

The competition prompt (P15 in the master prompt set):

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
- [ ] **Failure modes** define retries, timeouts, backoff, and idempotency rules
- [ ] **Performance** includes explicit SLOs with concrete numbers
- [ ] **Testing** covers unit, integration, and end-to-end, with fixture strategies
- [ ] **Rollout** includes feature flags, migration steps, and rollback procedures
- [ ] **Risk register** lists minimum 7 risks with severity and mitigation

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

## 2.4 Phase 1: Converting the Plan to Beads

### 2.4.1 What a Bead Is

A bead is an atomic unit of work: one task that one agent can complete in one session without needing external input. Each bead has an ID, a description, dependencies on other beads, acceptance criteria, and a status.

> *"Beads are extremely complementary to Agent Mail, and the combination lets you decompose a very big and complex plan into nice units."*
> — Jeffrey Emanuel

The bead system replaces ad-hoc task management. Agents do not choose what to work on based on conversation context or human instruction; they query the bead database for the highest-priority unblocked task, execute it, close it, and move on.

Beads are not limited to code. The bead set for a project includes implementation beads, test beads, observability beads, security review beads, and profiling beads.

---

### 2.4.2 Granularity: How Big Should a Bead Be?

The right size for a bead is 30-90 minutes of agent work, producing one meaningful commit. Rules of thumb:

**Too big:** "Implement the authentication system." This is 10-20 beads, not one. An agent will either lose coherence partway through or produce a monolithic commit that is hard to review.

**Too small:** "Add the import statement for jwt." This is a line of code. If the bead can be completed in under 5 minutes with trivial output, merge it into a larger bead.

**Right size:** "Implement JWT token validation middleware with expiry checking and refresh logic."

Complex plans decompose into 200 to 500 initial beads. The bead conversion prompt (P05) requests "epics + tasks + subtasks with appropriate level of granularity." A QA pass specifically checks granularity: are beads too large or too small?

> *"Starts out with just one agent [for bead creation]. Then I add more when it turns into 'make sure we didn't leave something important out' and polishing the beads."*
> — Jeffrey Emanuel

---

### 2.4.3 Dependency Ordering

Beads form a directed acyclic graph (DAG). Core infrastructure beads come first. Feature beads depend on infrastructure. Integration beads depend on features. The dependency structure is the primary coordination mechanism for multi-agent execution.

> *"If you have a large number of beads, you don't want agents just randomly choosing them. Because there's usually a 'right answer' for what each agent should do. And that right answer comes from the dependency structure of the tasks, and this can be mechanically computed using basic graph theory."*
> — Jeffrey Emanuel

The beads viewer (`bv`) computes this automatically: it identifies the critical path, ranks beads by priority (based on how many downstream beads they unblock), and recommends which bead each agent should take next.

A QA pass on the bead graph checks: are orderings correct? Are there missing dependency links? Are there circular dependencies?

---

### 2.4.4 Research Beads vs. Implementation Beads

Not all beads produce code. Research beads produce knowledge: API compatibility analysis, performance benchmarking of alternative approaches, or architecture spike investigations. Their output feeds back into plan revisions or informs implementation bead designs.

The transition from research to implementation is explicit: research beads produce findings; those findings may modify the plan; the modified plan generates new implementation beads. The two types do not blur.

New beads can be created at any point during a project's lifecycle. Discovering that you need a new bead during execution is normal operations, not a planning failure.

> *"Yes, I create new beads for in-progress projects all the time."*
> — Jeffrey Emanuel

---

### 2.4.5 Using Beads Viewer (bv)

The beads viewer (`bv`) is the coordination compass for multi-agent execution. It reads the bead database, computes the dependency graph, and answers the question: "What should each agent work on next?"

> *"[bv] is like a compass that each agent can use to tell them which direction will unlock the most work overall."*
> — Jeffrey Emanuel

In practice, agents invoke `bv --robot-triage` to get a prioritized list of ready beads. The human can also use `bv` interactively to visualize the full DAG, see the critical path, and identify bottlenecks.

The meta-prompt (where an agent uses bv to coordinate other agents):

> *"Re-read AGENTS dot md first. Then, can you try using bv to get some insights on what each agent should most usefully work on? Then share those insights with the other agents via agent mail."*
> — Jeffrey Emanuel

> *"Beads are insane. Slept on those until last week. Beads-viewer makes me happy."*
> — ACFS community member

---

### 2.4.6 Shared State vs. Worktrees

Jeff's approach to parallel agent execution is distinctive and sometimes controversial: all agents work in the same shared directory. No git worktrees.

> *"Working in one shared space surfaces conflicts immediately so you can deal with them, which is easy when agents can communicate."*
> — Jeffrey Emanuel

The coordination mechanisms that make this viable:
- **Advisory file reservations** via Agent Mail (agents "call dibs" on files temporarily)
- **Bead-level task decomposition** (agents work on different beads, minimizing file overlap)
- **Frequent commits** (changes are committed often, keeping the shared state clean)

The alternative (worktrees) defers conflict resolution to merge time, when the context about why a change was made has already been lost. The tradeoff: shared-state requires tighter coordination discipline but catches conflicts earlier.

> *"No worktrees with multiple agents working off same tasks list? They won't step on each other?"*
> — ACFS community member
>
> *"Doesn't seem to be a problem in practice. The beads keep them on track."*
> — Jeffrey Emanuel

Jeff tried worktrees and abandoned them:

> *"Friends don't let friends use git worktrees for agent swarms."*
> — Jeffrey Emanuel

Some practitioners successfully use jujutsu (jj) instead of git for agent isolation. The methodology does not require shared-state; it does require explicit coordination mechanisms regardless of the branching strategy.

---

## 2.5 Phase 2: Execution

### 2.5.1 Your First Bead

The execution prompt is short:

> *"Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any messages from other agents. Execute the highest-priority ready bead. Follow the bead execution protocol in AGENTS.md exactly. Use ultrathink."*

That is the entire prompt. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication. AGENTS.md defines the behavioral rules. The prompt simply says: "Go."

Start with a leaf node in the DAG (a bead with no unsatisfied dependencies). Watch the agent work. Review the output. Close the bead. This is the "hello world" of the methodology; once you've done it once, the loop for the rest of the project is the same.

---

### 2.5.2 Numbered Fix Sessions

After initial implementation, review happens in **numbered fix sessions**. Each session gets a sequential ID: Part 1, Part 2, Part 3. A long project might reach Part 23.

The deep review prompt (P02) does not prescribe what to look for. Instead, it asks the agent to explore the codebase with fresh eyes:

> *"Randomly explore the code files... deeply investigate and understand and trace their functionality... do a super careful, methodical, and critical check with 'fresh eyes' to find any obvious bugs, problems, errors, issues, silly mistakes."*

This is deliberate. A prescriptive checklist ("check for null pointer dereferences, SQL injection, race conditions") narrows the model's attention to those specific categories. An open-ended review prompt keeps the search space wide.

The recipe for review cycling:

1. Spin up ~5 fresh agents. Have each read AGENTS.md and explore the codebase.
2. Queue the deep review prompt. Let them find issues independently.
3. Have them review each other's code (cross-agent review).
4. Kill the agents when they come up clean. Start fresh ones with the same recipe.
5. Repeat until new agents find nothing to fix.

> *"Keep doing that round after round of that until they come up clean!"*
> — Jeffrey Emanuel

For aggressive bug hunting, Jeff uses the McCarthy prompt with an inflated bug count to change the model's prior:

> *"I know for a fact that there are at least 87 serious bugs remaining in this code. Your job is to find and fix as many as you can. This is Part 14 of an ongoing series..."*

The number is deliberately high. It forces the model to keep looking past the point where it would normally stop. Jeff also scales the search exponentially: start with 4 parallel agents running the McCarthy prompt, then 8, then 16, then 32. Gemini 3.1 has proven particularly effective for this role, sometimes running ~100 review passes on a single project.

The numbered sessions create a paper trail. If Part 14 introduced a regression, you can trace it. If Part 7 was the last clean session, you know where to look.

---

### 2.5.3 Praise Rounds in Execution

The praise round technique from planning (section 2.3.3) extends to execution. When reviewing code, ask the model what is *good* about it before asking what is wrong:

> *"Review this code. First, tell me what's excellent about it."*

This is not ego management. It prevents the model from reflexively rewriting working code. Models have a bias toward action; given code to review, they tend to suggest changes even when the code is correct. Anchoring on strengths first reduces unnecessary churn.

Jeff frames the underlying prompt mechanics in terms of cognitive science: "It's all based on theory of mind of the models and gestalt psychology concepts. This is the stuff that even the labs don't fully understand." The fresh-eyes technique works because it resets the model's attentional frame. A model that just wrote code is primed to defend it; a fresh model encountering the same code sees it without authorial bias.

For escalating review intensity, two prompts serve specific roles:

- **P27 (McCarthy):** *"I know for a fact that there are serious issues with this code. Find them."* This changes the model's prior from "optimistic reviewer" to "paranoid auditor."
- **P28 (Stakes):** *"Imagine your family's life depends on this code being correct. Find everything that could go wrong."* For security-critical or mission-critical code paths.

---

### 2.5.4 Alien Artifacts

Alien artifacts are a named, scheduled phase of the methodology. Every project includes a deliberate injection of theoretical constructs that standard engineering would not produce.

The process: take the plan (or the codebase, during execution) to a model with strong formal reasoning, and ask it to suggest mathematically optimal constructs. These might include BOCPD (Bayesian Online Change Point Detection), conformal prediction intervals, e-processes, information-theoretic bounds, or other techniques from recent research.

> *"This is why I call it an 'alien artifact.' It's not just some clever engineering, it's bringing to bear the last 50 years of math research."*
> — Jeffrey Emanuel

Alien artifacts are not optional or opportunistic. They are scheduled. Every N beads (or at specific plan milestones), you inject the artifact prompt:

> *"Review this plan/codebase and identify areas where mathematically sophisticated constructs would be superior to standard engineering approaches. Consider: BOCPD, conformal prediction, e-processes, information-theoretic bounds, Value of Information analysis, optimal stopping theory, or other techniques from recent research. For each proposal, explain the concrete improvement over the current approach and provide implementation-ready specifications."*

This is how you find improvements you didn't know to look for.

The FrankenSQLite plan, for example, gained a "Conformal Martingale Regime Shift Detector" during the alien artifact phase, replacing a more conventional change-point detection approach. The improvement provided distribution-free, finite-sample guarantees that the original approach lacked.

---

### 2.5.5 When to Stop

Every phase of the methodology has an explicit convergence criterion:

| Phase | Stop condition |
|-------|---------------|
| Planning | Only minor wording tweaks; no architectural or operational improvements |
| Bead conversion | All plan sections mapped to beads; no content unrepresented |
| Bead QA | Changes are mostly reordering or renaming, not missing work |
| Implementation | All beads implemented and closed |
| Review | Repeated fresh-eyes passes produce no substantive diffs |

The review convergence is the most important. Run fix sessions until new agents find nothing to fix. If Part 14 produces no changes, you're done.

Before closing a project phase, run one final check for stubs and placeholders:

> *"Look for stubs, placeholders, mocks, of ANY KIND. These ALL must be replaced with fully fleshed out, working, correct, performant, idiomatic code."*

A bead is done when its acceptance criteria are met and tests pass. Not when it is "perfect." Close it. Move on.

---

### 2.5.6 Commit Discipline

A dedicated commit agent handles all git operations. It does not write code. Its entire job is to read the diff, group changes into logically connected commits, write detailed commit messages with bead IDs, and push.

> *"Now, based on your knowledge of the project, commit all changed files in a series of logically connected groupings with super detailed commit messages for each and then push. Don't edit the code at all."*
> — Jeffrey Emanuel

Separating commits from implementation solves a common problem: agents that write code produce poor commit messages (short, vague, or inaccurate). A dedicated commit agent, given time and context, produces commit messages that accurately describe what changed and why.

Without this discipline, three failure modes recur: (1) cross-contaminated commits, where changes from two unrelated beads end up in one commit; (2) half-done commits, where an agent commits mid-bead because it "felt like a good stopping point"; (3) monolith commits, where 2,000 lines of changes across 40 files land in a single message saying "implement features." All three make rollbacks painful and bisection impossible.

The resulting git log becomes a readable narrative of the project's development, grouped by feature and annotated with bead IDs. This is valuable for review, debugging, and onboarding future agents to the project.

Commit rules:
- Group commits by bead or feature slice, not by time.
- Include bead IDs in commit messages: `feat(auth): implement JWT validation [BEAD-042]`
- The commit agent does not edit code, only commits. Jeff's `dcg` tool enforces this by preventing agents from running `git checkout`.

---

## 2.6 Scaling to Multi-Agent

### 2.6.1 Why and When to Scale

A single agent working through a bead queue produces roughly 5-10 beads per day. For a 50-bead project, that is a week of serial execution. For a 300-bead project, it is over a month.

The inflection point for scaling is when the serial execution time exceeds your patience or your deadline. In practice, this is around 80-100 beads.

Start with 3-4 agents. This is the range where coordination overhead is minimal and the throughput gain is significant.

> *"I've found that things work pretty well with 3-4 agents. But if you have a big and complex plan of work that can effectively be decomposed, you can handle 5 of them or even more."*
> — Jeffrey Emanuel

Scaling beyond 5 agents requires the full coordination infrastructure: Agent Mail for messaging, NTM for session management, advisory file reservations, and a dedicated commit agent. Without these, agents collide.

> *"I find scaling past 2-3 agents has a pretty steep falloff for oversight quality in any non-greenfield work."*
> — ACFS community member

This observation is accurate for setups without coordination infrastructure. With Agent Mail, bead-level task decomposition, and file reservations, the ceiling is much higher.

---

### 2.6.2 Agent Roles

Implementation agents are **fungible generalists**. Do not assign "the auth agent" or "the database agent." Each agent picks the next available bead, regardless of domain. Specialization creates bottlenecks; generalization allows any agent to take any ready bead.

The one exception: the **commit agent**. This is a single Claude Code instance running only the commit prompt (P11). It monitors the working directory, groups changes into logical commits, writes detailed messages with bead IDs, and pushes. It never modifies code.

A typical formation for a serious project:

| Agent type | Count | Role |
|---|---|---|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only |

Model mixing is deliberate. Different models have different strengths, and using multiple models increases the probability that at least one agent catches an issue that others miss.

---

### 2.6.3 Agent Mail

Agent Mail is the async messaging layer that enables multi-agent coordination. Agents register identities, send and receive messages, and declare file reservations.

The canonical onboarding prompt for every new agent session:

> *"Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks."*
> — Jeffrey Emanuel

Key capabilities:
- **Message routing:** agents send status updates, ask questions, report blockers.
- **Advisory file reservations:** agents "call dibs" on files they're working on. Reservations are not rigidly enforced and expire automatically.
- **Threading:** conversations between agents maintain context across multiple messages.
- **Persistent state:** messages persist in a SQLite database. A fresh agent can read the message history to reconstruct context.

> *"The advisory file reservation system is critical to how I get such crazy results from all these agents."*
> — Jeffrey Emanuel

The anti-pattern to avoid is "communication purgatory," where agents exchange messages endlessly without producing code. The coordination protocol in AGENTS.md should emphasize: be proactive about starting work, inform others when you do, and communicate blockers, not just status.

---

### 2.6.4 NTM + tmux

NTM (Named Tmux Manager) is the agent cockpit. It manages tmux sessions where agents run, providing 80+ commands for session management, prompt broadcasting, and agent monitoring.

Basic workflow:

```bash
ntm new coder1          # create a named session
ntm new coder2          # another
ntm new reviewer        # and another
ntm attach coder1       # jump into any session
```

For bulk spawning:

```bash
ntm --robot-spawn=myproject --spawn-cc=4 --spawn-cod=4 --spawn-gmi=2
```

This creates 10 agent sessions in one command. Each gets a randomly generated color+noun name (RedSnow, BrownLake, PurpleBear) that agents use to identify themselves in Agent Mail.

NTM enables a single human to monitor and steer many agents simultaneously. When the human sees a stall in one pane, they send a prompt to that pane. When they see a bug, they send the review prompt. All prompts are sent verbatim from the master prompt set.

---

### 2.6.5 Worktrees vs. Shared State

See section 2.4.6 for the full discussion. In brief: the methodology recommends shared-state by default, with advisory file reservations and bead-level decomposition preventing collisions. Worktrees are an alternative that some practitioners use successfully, particularly with jujutsu (jj) instead of git.

---

### 2.6.6 Coordination Patterns

The coordination flow for a running multi-agent session:

1. **Claim:** Agent claims a bead by marking it `in_progress`.
2. **Execute:** Agent implements the bead following AGENTS.md protocol.
3. **Self-review:** Agent rereads its own code with fresh eyes before closing.
4. **Report:** Agent sends a status message via Agent Mail.
5. **Close:** Agent marks the bead as complete via `br close <id>`.
6. **Next:** Agent picks the next highest-priority ready bead via `bv --robot-triage`.

The human steers reactively, not proactively:
- Sees stall → sends P09 (Agent Mail Check & Continue)
- Sees bug → sends P02 (Deep Review)
- Sees drift → sends P20 (Post-Compaction Refresh)

All steering prompts are verbatim from the master prompt set. The human does not improvise instructions; they select the appropriate prompt and fire it.

> *"Then they just keep cranking on their own for a really long time. And you don't need to supervise them much."*
> — Jeffrey Emanuel

The trust model is: wide autonomy with mutual oversight.

> *"I'm ultimately in charge, but I grant them very wide latitude. But with extreme oversight by means of mutual inspection and verification. That is, they mostly keep each other honest."*
> — Jeffrey Emanuel

---

### 2.6.7 The Integration Cycle

After a batch of beads is completed, integration happens:

1. The commit agent groups all changes into logical commits with bead IDs.
2. A fresh agent runs a cross-agent review (P03): examine code written by other agents, looking for inconsistencies, bugs, and integration issues.
3. The full test suite runs. Failures are filed as new beads.
4. Fix beads are executed in the normal way.

Integration issues are expected, not failures. When 6 agents implement 20 beads in parallel, some integration bugs are inevitable. The test suite catches them; the review cycle fixes them; the process continues.

> *"If it does then another one will fix it. And I already have thousands of unit tests and end to end tests to surface problems quickly."*
> — Jeffrey Emanuel

The safety net that makes all of this viable is test coverage. Projects with 10,000+ unit and end-to-end tests can tolerate aggressive parallelism because regressions surface immediately.

---

## 2.7 The Daily Workflow

### 2.7.1 Session Start

A typical session start sequence:

1. `acfs doctor` to verify everything is healthy.
2. `bv --robot-triage` to identify the highest-priority unblocked beads.
3. Review yesterday's Agent Mail messages and git log.
4. `ntm --robot-spawn=<project>` to spin up agents.
5. All agents: read AGENTS.md, register with Agent Mail, start executing.
6. One dedicated commit agent running P11 only.

The first message to every agent, every time:

> *"Before doing anything else, read ALL of AGENTS dot md."*

After every context compaction (when the model's conversation history is compressed), repeat this instruction. Agents that lose AGENTS.md from context start drifting from project conventions.

---

### 2.7.2 The Loop

The execution loop is:

1. Agents pick beads via `bv`, execute them, self-review, report via Agent Mail.
2. The human monitors via tmux panes, Agent Mail logs, and git log.
3. When the human sees a stall, they send the appropriate steering prompt.
4. When review rounds come up clean, the human moves to the next batch of beads.

Jeff cycles through 7+ projects daily, sending polishing and fixing prompts to agents that need forward progress. Each prompt is dispatched with a single button press.

> *"I like to make sure that I'm making some forward progress on every one of my active projects each day, even when I'm too busy to spend real mental bandwidth on all of them every single day."*
> — Jeffrey Emanuel

The human is not needed for every cycle. During the execution phase, the human's role is strategic steering, not line-by-line oversight.

---

### 2.7.3 Context Management

Context degradation is the primary failure mode of long sessions. After context compaction, the model loses details from earlier in the conversation. The symptoms: repeating failed approaches, ignoring project conventions, or drifting from the current bead's scope.

The response protocol:

| Symptom | Action |
|---------|--------|
| Agent ignores AGENTS.md conventions | Send P10 (re-read AGENTS.md) |
| Agent repeats the same failed approach | Kill session, start fresh |
| Agent is less effective after compaction | Send P20 (Post-Compaction Refresh) |
| Agent has been running 4+ hours on one bead | Kill session, narrow the bead scope, start fresh |

Fresh sessions are cheap. Debugging a context-polluted session is expensive. When in doubt, start over.

> *"If I notice it is being less effective I'll close it and reopen a new session."*
> — Jeffrey Emanuel

> *"Craft the prompts to be completely in single session context. You will love the output more the less context rot creeps in."*
> — ACFS community member

**On token conservation:** A common instinct is to pipe build output through wrapper scripts that truncate it, saving context tokens. Jeff rejects this: "Nah, I'd rather burn the tokens. The scripts never work right." His approach is to let full output stream into context and scale via multiple subscriptions rather than compressing context. Rich context produces better decisions than saved tokens.

**Experimental workaround:** A PreCompact hook can write a marker file, and a UserPromptSubmit hook can detect that marker and automatically re-inject critical context (AGENTS.md re-read, current bead state) after compaction. This is not yet reliable across all models but is the direction of travel for automated context recovery. A related anti-pattern: "ringleader agents" (a coordinator agent that manages other agents). Ringleader agents fail because when the coordinator's context compacts, the entire swarm loses coherence. Flat coordination via Agent Mail and bead priorities is more resilient.

---

### 2.7.4 End of Day

End of session runs:
1. P11 (final commit round): commit all outstanding changes.
2. P02 (final review): one last fresh-eyes pass.

The bead system, Agent Mail history, and git log serve as persistent state. A fresh agent tomorrow can reconstruct context from these three sources. Elaborate handoff documents are unnecessary; the infrastructure is the handoff.

> *"The messages themselves act like a kind of repository (as does the plan document), so you can resume again from a fresh context."*
> — Jeffrey Emanuel

---

### 2.7.5 Marathon Sessions vs. Structured Sprints

Agents running on well-structured projects with comprehensive test suites can operate autonomously for 8-17+ hours. This is documented and verified.

> *"I've seen a lot of skepticism recently from people about the claims around how long agents can go autonomously. People think it's BS because they haven't observed it. It's real. 17+ hours."*
> — Jeffrey Emanuel

The key factor is not the model's capability but the project's structure. An agent working against a well-defined bead in a project with good tests stays grounded: it executes a task, runs tests, handles failures, and moves on. The work itself provides the context that prevents drift.

> *"Biggest factor in long runs isn't intelligence, it's having a well-structured repo with good tests to anchor against."*
> — Agent self-report from the ACFS community

You do not need to run marathon sessions. Structured sprints (spin up agents, run review cycles until clean, kill, restart) work well and give the human more control. The marathon pattern is most useful for overnight or unattended execution, where the human reviews results the next morning.

---

# LEVEL 3: TACTICAL -- "The Reference"

Everything in Levels 1 and 2 explains why the methodology exists and how to execute it. Level 3 is the lookup table. When you need to know what a tool does, how a prompt works, or how to configure a project file, this is where you come.

---

## 3.1 Tool Reference

The flywheel is built from approximately 20 interconnected tools. No single tool is transformative on its own; the compounding effect comes from how they reinforce each other. NTM spawns agents that use Agent Mail to coordinate. BV tells agents what to work on. CASS and CM give agents memory across sessions. DCG and SLB prevent disasters, which is what allows "vibe mode" (dangerous flags enabled) to be safe.

> *"The magic isn't in any single tool. It's in how they work together. Using three tools is 10x better than using one."*
> -- Jeffrey Emanuel

The tools are organized into tiers based on how central they are to the workflow.

---

### 3.1.1 Tier 1 -- Core (Always Running)

These are the tools that define the flywheel. Every ACFS project uses all of them.

#### Claude Code (CC)

**Role:** Primary AI coding agent. The workhorse of the flywheel.

Claude Code is the CLI interface to Claude models. It runs inside tmux panes managed by NTM, reads AGENTS.md for project context, communicates with other agents via Agent Mail, and executes beads as directed by BV. In Jeff's typical formation, 5-6 Claude Code instances (running Opus) handle primary implementation work, plus one dedicated commit agent running P11 only.

Key characteristics:
- Runs in tmux (managed by NTM), persistent across SSH disconnects
- Reads `AGENTS.md` and `CLAUDE.md` for project-specific instructions
- Supports MCP tools (Agent Mail, CASS skills, etc.) via `settings.json` hooks
- PreToolUse hooks enable DCG (destructive command guard) and RCH (remote compilation)
- "Vibe mode" = passwordless sudo + dangerous flags enabled, guarded by DCG

**Forensic signal in Jeff's repos:** Named agents in commits (RedSnow, BrownLake, PurpleBear), multiple commits in the same 1-5 minute window confirming parallel execution.

---

#### Beads Rust (br / bd)

| | |
|---|---|
| **Stars** | 623 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/beads_rust |

**Role:** Local-first issue tracker. The execution graph for all agent work.

Beads is not a todo list. It is a dependency-aware task graph stored in SQLite (primary) with JSONL for git portability. Every task has: scope, acceptance criteria, context, rationale, dependencies (blocks/related/parent-child), constraints, and priority (P0-P4). Agents never refer back to the plan during execution; the beads are self-contained.

**Key commands:**
```
br create "title" --priority P1 --parent <epic-id>    # Create a bead
br list --status open --sort priority                   # List open beads
br close <id>                                           # Mark complete
br stats                                                # Velocity metrics
br dep add <id> --blocks <other-id>                     # Add dependency
bd ...                                                  # Alias (backward compat)
```

**Scale:** Jeff has hit 693 beads/day at peak velocity. A typical project has 500-2000 beads created during P05 (Plan to Beads).

**Forensic signal:** `beads.db` or `beads.jsonl` in repo root. Bead IDs in commit messages (`[bead-NNN]` or `bd-xxxxx`). High-impact beads completed before low-impact ones (PageRank ordering from BV).

---

#### Beads Viewer (bv)

| | |
|---|---|
| **Stars** | 891 |
| **Language** | Go |
| **GitHub** | github.com/Dicklesworthstone/beads_viewer |

**Role:** Graph-theory triage engine for beads. Tells agents what to work on.

BV treats the bead graph as a DAG and applies nine graph metrics including PageRank and betweenness centrality to determine which beads are highest-impact and ready for execution. Its robot protocol (`--robot-*` flags) produces JSON output that agents consume directly.

**Key commands:**
```
bv --robot-triage          # JSON output: highest-impact actionable beads, PageRank-sorted
bv --robot-insights        # Analysis of graph structure, bottlenecks
bv --robot-timeline        # Projected completion timeline
bv --robot-next            # Single highest-priority ready bead
bv -export-pages /tmp/bv   # Static HTML site of the bead graph
```

**How agents use it:** P08 (Execute Beads) and P24 (Use BV) both direct agents to call `bv --robot-triage` to pick their next work item. This is how coordination happens without a human assigning tasks.

> *"They follow the plan as embodied in the beads task structure, and they use my bv tool to pick the best bead to work on next."*
> -- Jeffrey Emanuel

**Forensic signal:** High-impact beads completed before low-impact ones. "bv" referenced in AGENTS.md. PageRank-consistent commit ordering.

---

#### NTM -- Named Tmux Manager

| | |
|---|---|
| **Stars** | 160 |
| **Language** | Go |
| **GitHub** | github.com/Dicklesworthstone/ntm |

**Role:** Agent cockpit. Spawns, monitors, and manages agent sessions.

NTM is the control plane for the agent swarm. It manages named tmux panes where Claude Code, Codex, and Gemini CLI instances run. 80+ commands cover session management, prompt broadcasting, file conflict detection, and context rotation. Sessions survive SSH disconnects.

**Key commands:**
```
ntm --robot-spawn=<project> --spawn-cc=4 --spawn-cod=4 --spawn-gmi=2
                                          # Spawn 10 agents: 4 CC, 4 Codex, 2 Gemini
ntm --robot-send=<session> --msg='<prompt>'
                                          # Send a prompt to a specific agent
ntm --robot-terse                         # Compact status of all sessions
ntm --robot-status --json                 # Full JSON status
```

**Naming convention:** NTM auto-generates color+noun names for agents: RedSnow, BrownLake, PurpleBear, PinkMountain, GreenRiver, BluePeak. These names appear in Agent Mail messages and sometimes in commit attribution.

**The human steering pattern:** Jeff monitors agents via NTM status and Agent Mail. When he sees a stall, he sends P09 (Anti-deadlock). When he sees a bug, P02 (Deep Review). When context compaction fires, P20 (Refresh). All prompts are sent verbatim via Stream Deck buttons.

> *"I don't even need to look at the button labels anymore."*
> -- Jeffrey Emanuel, on his Stream Deck + NTM workflow

**Forensic signal:** Named agents in commits. Multi-agent commits in the same minute window. "ntm" referenced in AGENTS.md.

---

#### CASS -- Coding Agent Session Search

| | |
|---|---|
| **Stars** | 524 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/coding_agent_session_search |

**Role:** Unified search across ALL agent sessions. The memory backbone.

CASS indexes 11 agent formats: Claude Code, Codex, Cursor, Gemini, ChatGPT, Cline, Aider, Pi-Agent, Factory, OpenCode, Amp. Powered by Tantivy for sub-60ms keyword queries, with optional MiniLM semantic search. When an agent needs to know "have we solved this before?" or "what did I do last session?", CASS is the answer.

**Key commands:**
```
cass search "query"                       # Keyword search across all sessions
cass search "query" --mode semantic       # Semantic search (MiniLM embeddings)
cass index ~/.claude/projects/            # Re-index session files
```

**How it fits:** CASS is the read-only prior-art layer. Before starting a new task, Jeff (or an agent with CASS as a skill) searches for prior work: "Have we handled this error pattern before?" "What approach did the Gemini agent use for the buffer pool?" CM (CASS Memory) builds on top of CASS to create persistent procedural memory.

> *"I do have a cass skill which the agents can use to find past conversation excerpts easily that are relevant."*
> -- Jeffrey Emanuel

**Forensic signal:** No direct git signal (it is a personal context recall tool). Its value shows up indirectly: agents making decisions consistent with prior sessions, patterns reappearing across projects.

---

### 3.1.2 Tier 2 -- Safety, Quality, and Coordination

These tools extend the core with safety nets, quality gates, and cross-session memory.

#### MCP Agent Mail

| | |
|---|---|
| **Stars** | 1,742 |
| **Language** | Python (Rust variant: 22 stars) |
| **GitHub** | github.com/Dicklesworthstone/mcp_agent_mail |

**Role:** Inter-agent coordination. The messaging backbone.

Agents register identities, send/receive messages, and declare file reservations to prevent edit conflicts. HTTP FastMCP server with Web UI and static export. Agents use it through MCP tools in Claude Code, Codex, or Gemini CLI.

**Key operations (via MCP tools, not CLI):**
- Register agent identity on spawn (P18)
- Send messages to specific agents by name
- Declare file reservations (prevents two agents editing the same file)
- Check inbox for blockers, discoveries, and coordination messages
- Agent Mail SQLite is a monitoring surface for the human operator

**The anti-deadlock pattern:** P09 exists specifically because agents can fall into "communication purgatory" -- messaging each other without shipping code. P09 forces them back to work: "Don't get stuck in communication purgatory where nothing is getting done; be proactive about starting tasks."

**Forensic signal:** "check agent mail" in commits. Cross-agent coordination messages. AGENTS.md with "Agent Mail" coordination section. `mcp_agent_mail_rust` sync commits confirm parallel agents.

---

#### UBS -- Ultimate Bug Scanner

| | |
|---|---|
| **Stars** | 132 |
| **Language** | Bash |
| **GitHub** | github.com/Dicklesworthstone/ultimate_bug_scanner |

**Role:** AST-grep patterns, 1000+ detection rules, 8 languages, 18 categories. The quality gate.

UBS is the standard scan that runs as part of P21 (UBS Scan prompt). Sub-5s scans. Auto-wires into Claude Code, Codex, Gemini, and Cursor.

**Key commands:**
```
ubs .                                     # Scan entire project
ubs file.ts file.py                       # Scan specific files (<1s)
ubs $(git diff --name-only --cached)      # Scan staged files only
```

**Forensic signal:** "ubs scan" in commit notes. Defensive patterns added post-scan. Maps to P21 (UBS Scan prompt).

---

#### DCG -- Destructive Command Guard

| | |
|---|---|
| **Stars** | 89 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/destructive_command_guard |

**Role:** Claude Code PreToolUse hook. Blocks dangerous commands BEFORE execution.

DCG is what makes "vibe mode" (passwordless sudo, dangerous flags enabled) safe. It intercepts every tool use attempt and checks against 50+ rule packs across 17 categories: git, filesystem, DB, k8s, cloud, CI/CD. Fail-open design means it never blocks legitimate work.

**What it catches:**
- `rm -rf` (outside /tmp)
- `git reset --hard`, `git push --force`, `git branch -D`
- `git clean -f`, `git stash drop/clear`
- Database drops, k8s deletes, cloud resource destruction

**Configuration:** Lives in `~/.claude/settings.json` as a PreToolUse hook.

> *"Agent safety should be baked into the runtime, not bolted on after. Validation gates that catch destructive ops before they execute is such an obvious pattern yet almost nobody ships with it by default."*
> -- @doodlestein community member, Feb 2026

> *"[DCG] gets called automatically by the agent on every attempted tool use. [...] The agent will learn from dcg and stop trying to do dumb stuff. Definitely that happens within the individual session level via in-context learning, and might even persist now that Claude Code has built in native memory by default."*
> -- Jeffrey Emanuel

**Forensic signal:** N/A for Jeff's repos (personal safety tool, no git trace).

---

#### Brenner Bot

| | |
|---|---|
| **Stars** | 28 |
| **Language** | TypeScript |
| **GitHub** | github.com/Dicklesworthstone/brenner_bot |

**Role:** Research orchestration inspired by Nobel laureate Sydney Brenner.

Not a code review bot. Brenner Bot is an "iterated knowledge distillation" system that converts unstructured research corpora into refined "algebras of ideas" with operators. Hypothesis lifecycle tracking, discriminative test design, anomaly management, evidence pack integration.

Jeff describes the broader value beyond molecular biology:

> *"Although the system itself is very cool and powerful, and Brenner's approach and worldview are breathtakingly brilliant and fruitful, perhaps the more relevant thing is the process of iterated knowledge distillation that was used to convert a massive amount of unstructured, meandering stories about his life and work into a refined 'algebra of ideas' complete with operators. [...] What else could it be used for? Perhaps something as far afield as philosophy or the classics?"*
> -- Jeffrey Emanuel

**Forensic signal:** Deep research docs, hypothesis files in repos. Very rare. Jeff's most complex research projects only.

---

#### CM -- CASS Memory System

| | |
|---|---|
| **Stars** | 253 |
| **Language** | TypeScript |
| **GitHub** | github.com/Dicklesworthstone/cass_memory_system |

**Role:** Procedural memory for agents. Three-layer cognitive architecture: Episodic -> Working -> Procedural.

CM transforms scattered sessions into persistent cross-agent memory. Patterns discovered in one Cursor session automatically help Claude Code in the next session. It builds on top of CASS (session search) to create durable, actionable memory.

**Key commands:**
```
cm context "<task>" --json     # Inject relevant procedural rules into agent prompts
cm mark <id> --helpful         # Positive feedback loop
cm mark <id> --harmful         # Negative feedback loop
cm reflect                     # Trigger memory consolidation
cm stats                       # Memory health metrics
cm stale                       # Find outdated memories
```

**How it fits:** Pre-session, Jeff runs `cm context "<task>"` to load relevant procedural rules for the upcoming work. This context gets injected into agent prompts, giving fresh agents the benefit of prior sessions' discoveries.

**Forensic signal:** `cm context` blocks in project AGENTS.md. Consistent patterns across Jeff's many projects.

---

#### RCH -- Remote Compilation Helper

| | |
|---|---|
| **Stars** | 35 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/remote_compilation_helper |

**Role:** Claude Code PreToolUse hook. Offloads cargo builds to remote workers.

Intercepts build commands, syncs source via rsync + zstd compression, builds on the remote machine, streams artifacts back. Jeff has a 64-core machine dedicated to compilation; RCH makes it transparent to agents running on VPS instances.

**Forensic signal:** Fast build times on VPS-class hardware. No direct git trace.

---

#### SLB -- Simultaneous Launch Button

| | |
|---|---|
| **Stars** | 59 |
| **Language** | Go |
| **GitHub** | github.com/Dicklesworthstone/simultaneous_launch_button |

**Role:** Two-person rule for dangerous commands. Nuclear-launch-style safety.

Four risk tiers: CRITICAL (2+ approvals required), DANGEROUS, CAUTION, SAFE. Cryptographic signing, rollback support. For security-critical projects where a single misconfigured agent could cause real damage.

**Forensic signal:** Security-critical projects only.

---

### 3.1.3 Tier 3 -- Acceleration and Infrastructure

These tools enable Jeff's specific scale (50+ agents across 3 machines) but are optional for most practitioners.

#### CAAM -- Coding Agent Account Manager

| | |
|---|---|
| **Stars** | 59 |
| **Language** | TypeScript |
| **GitHub** | github.com/Dicklesworthstone/coding_agent_account_manager |

**Role:** Sub-100ms auth switching across multiple AI provider accounts.

Jeff runs 2 Claude Max accounts ($400/month), ChatGPT Pro ($200/month), and Gemini. With 10+ agents across 3 machines, CAAM handles smart rotation, cooldown tracking, health scoring, and vault isolation. When one account hits rate limits, CAAM transparently rotates to the next.

**Key commands:**
```
caam switch      # Rotate to next healthy account
caam status      # Health and cooldown state for all accounts
caam rotate      # Force rotation
```

> *"Nah, I'd rather burn the tokens. The scripts never work right. I do have a lot of pro/max accounts, though."*
> -- Jeffrey Emanuel

**When you need it:** Only relevant when running more agents than a single subscription supports (roughly >5 concurrent Claude Code instances on Max).

---

#### S2P -- Source to Prompt TUI

**Role:** Interactive code-to-prompt generator.

A TUI tool for assembling code context into prompts. Useful when you need to extract specific code sections for review or multi-model comparison, particularly during P04/P15 planning rounds where you paste code into competing model conversations.

---

#### Meta Skill (ms)

| | |
|---|---|
| **Stars** | 126 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/meta_skill |

**Role:** Skill management with MCP integration.

Dual persistence (SQLite + Git), hybrid search (BM25 + semantic + RRF), UCB bandit optimization for skill selection. Manages the growing library of agent skills and prompts.

**Key commands:**
```
ms search "query"    # Find relevant skills
ms install <id>      # Install a skill
ms list              # List available skills
```

---

#### RANO

| | |
|---|---|
| **Stars** | 23 |
| **Language** | Rust |
| **GitHub** | github.com/Dicklesworthstone/rano |

**Role:** Network observer. Tracks outbound connections from AI CLI processes.

Monitors what Claude/Codex/Gemini is connecting to. Useful for auditing agent behavior and understanding which external services agents contact during execution.

---

#### Supporting Tools (Quick Reference)

| Tool | Role |
|------|------|
| **APR** (Automated Plan Reviser Pro) | Automates multi-round P04 spec review using extended reasoning. `apr refine plan.md --rounds 10` |
| **JFP** (JeffreysPrompts CLI) | Browse and install battle-tested prompts. `jfp list / jfp search / jfp install`. This IS the MASTER_PROMPTS.md -- the CLI version. |
| **RU** (Repo Updater) | Sync 100+ GitHub repos in one command. `ru sync` |
| **SRPS** (System Resource Protection) | ananicy-cpp + curated rules. Auto-deprioritize background processes. Essential at 10+ agent scale. |
| **MDWB** (Markdown Web Browser) | Convert websites to markdown for LLM consumption |
| **TRU** (TOON Rust) | Token-optimized notation, 30-50% context reduction |
| **PT** (Process Triage) | Bayesian zombie process detection |
| **CAUT** (Usage Tracker) | Track LLM provider usage and costs |
| **WA** (WezTerm Automata) | Terminal hypervisor for multi-agent automation (superseded by FrankenTerm) |

---

### 3.1.4 Decision Matrix: Which Tools Do You Need?

Not everyone needs the full toolchain. Here is a decision matrix based on project scale:

| Scale | Agents | Essential Tools | Recommended Additions |
|-------|--------|----------------|----------------------|
| **Solo** | 1 CC | Beads (br), CLAUDE.md | UBS, DCG |
| **Small team** | 2-3 agents | + NTM, Agent Mail, BV | + CASS, SLB |
| **Standard** | 4-6 agents | + CM, UBS, DCG | + RCH (if Rust), SRPS |
| **Heavy** | 7-10 agents | + CAAM (if multi-account), SRPS | + APR, Meta Skill |
| **Jeff-scale** | 11-50+ agents | All of the above | + S2P, RANO, SLB, RU |

**The minimum viable flywheel** is: Beads + BV + AGENTS.md + one Claude Code instance. You can run the entire methodology with just these. Everything else is acceleration.

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

## 3.3 Configuration Reference

Three configuration surfaces define an ACFS project: AGENTS.md (the project constitution), CLAUDE.md (the model-specific configuration), and the MCP/hooks config in `settings.json`. Together they form the complete operating environment for agents.

---

### 3.3.1 AGENTS.md -- The Full Specification

AGENTS.md is the single most important file in an ACFS project. It is not documentation. It is an operating manual for minds that will wake up without memory of any prior conversation. Jeff writes this himself or heavily directs its creation.

> *"You need to have a whole section in AGENTS dot md telling it to never do stuff like that."*
> -- Jeffrey Emanuel

> *"Try replacing your whole AGENTS dot md with one based on [the reference] but adapted to fit your project/stack (Claude can do this for you). And you have to tell it to read it carefully as the very first message."*
> -- Jeffrey Emanuel

> *"ALWAYS read the project's AGENTS.md file before starting a task. This is always essential, but especially when testing, since it will often outline testing guidance."*
> -- Jeffrey Emanuel (added to his own ~/.codex/AGENTS after an agent ignored testing instructions)

#### Mandatory Sections

**1. Project Identity**
```markdown
## Project: <NAME>
<1-2 paragraph description: what this is, who it's for, why it exists>
Tech stack: <languages, frameworks, key libraries>
Repo structure: <brief map of src/, tests/, docs/>
```

**2. Tool Rules**
```markdown
## Tools
- Beads: `br` (or `bd`). Use `br create`, `br close`, `br list`.
- Beads Viewer: `bv --robot-triage`, `bv --robot-insights`.
  NEVER use `bv` without `--robot-*` flags.
- Agent Mail: Register on spawn. Check inbox before each bead. Declare file reservations.
- UBS: Run `ubs .` after major changes.
- CM: Run `cm context "<task>"` before starting new task areas.
```

**3. Bead Execution Protocol (MANDATORY)**
```markdown
## Bead Execution Protocol
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Implement it completely
3. Before closing: read over ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
4. Only then: `br close <id>`
5. Communicate to fellow agents via Agent Mail
6. Pick next bead. Repeat.
Do NOT commit (the commit agent handles this).
Do NOT stop between beads. Keep the loop going.
```

**4. Coordination Rules**
```markdown
## Coordination
- Claim beads before starting (mark `in_progress`)
- Communicate blockers and discoveries, not just status
- Group commits by bead/feature slice, not by time
- Bead IDs in commit messages are the coordination surface
- Agent failure is normal operations -- log it, handle it, move on
- Do NOT get stuck in "communication purgatory"
```

**5. Constraints and Best Practices**
```markdown
## Constraints
- Never use mocks in tests. Real data, real API calls, real e2e.
- Never commit ephemeral files or secrets.
- Never reopen PLAN.md during execution. The bead-map is the execution surface.
- Follow <LANGUAGE> best practices: <link to style guide or inline rules>
- All error handling must be explicit. No silent failures.
```

**6. Model Selection (if multi-model)**
```markdown
## Models
- Claude Code (Opus): primary implementation
- Codex: parallel implementation, deterministic work
- Gemini CLI: review duties, shell-restricted tasks
- Commit agent: 1x CC with P11 only. Never edits code.
```

#### Optional Sections

- **Best Practice Guides:** Inline or referenced. Language-specific coding standards.
- **Recovery Runbook:** What to do when the system is in a bad state.
- **Risk Register:** Top risks with severity and mitigation status.
- **Architecture Decision Records (ADRs):** Key decisions locked as reference. frankentui uses beads for ADRs (bd-10i.10.1 through bd-10i.10.6).
- **CM Context Blocks:** Pre-loaded procedural memory for agents.

#### Anti-Patterns

- AGENTS.md generated by an AI and never reviewed. The constitution must be human-authored or human-directed.
- AGENTS.md that is too long. Agents read it at the start of every session and after every context compaction. If it exceeds ~500 lines, agents lose attention on the critical rules.
- AGENTS.md without the Bead Execution Protocol. This section is what makes the loop work.

---

### 3.3.2 CLAUDE.md Inheritance

Claude Code supports a three-level configuration hierarchy:

| Level | File | Scope |
|-------|------|-------|
| **User** | `~/.claude/CLAUDE.md` | All projects on this machine |
| **Project** | `<repo>/CLAUDE.md` | This repository |
| **Directory** | `<repo>/subdir/CLAUDE.md` | This subdirectory (additive) |

All three are loaded and merged. More-specific files override or extend less-specific ones.

**What goes where:**

| File | Content |
|------|---------|
| `~/.claude/CLAUDE.md` | Global defaults: preferred language style, safety rules, personal conventions. Applies to all repos. |
| `<repo>/CLAUDE.md` | Project-specific instructions: tech stack, testing framework, linting rules, custom commands. Checked into git so all developers share it. |
| `<subdir>/CLAUDE.md` | Directory-specific rules: "files in this directory are auto-generated, do not edit" or framework-specific conventions. |

**Relationship to AGENTS.md:** CLAUDE.md is a Claude Code-specific feature. AGENTS.md is tool-agnostic -- it works with Codex, Gemini, and any future agent. In practice, CLAUDE.md contains Claude Code-specific configuration (MCP tool hints, hook behavior), while AGENTS.md contains the universal project constitution that all agents read.

Jeff's pattern: AGENTS.md is the primary document. CLAUDE.md supplements it with Claude Code-specific configuration and occasionally duplicates critical rules from AGENTS.md for emphasis.

> *"It's such a bummer trying to maintain separate CLAUDE dot md files. [...] I should probably modify it for codex, though."*
> -- Jeffrey Emanuel

---

### 3.3.3 MCP Configuration

MCP (Model Context Protocol) tools are configured in `~/.claude/settings.json`. This is where Agent Mail, CASS skills, and other MCP servers are wired into Claude Code.

**Example `settings.json` structure:**

```json
{
  "mcpServers": {
    "agent-mail": {
      "command": "python3.13",
      "args": ["-m", "mcp_agent_mail.cli", "serve-http", "--host", "0.0.0.0", "--port", "8765"]
    }
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash|Computer",
        "command": "dcg check \"$INPUT\""
      }
    ]
  }
}
```

**Key MCP tools in a typical ACFS setup:**

| MCP Server | Port | Purpose |
|------------|------|---------|
| Agent Mail | 8765 | Inter-agent messaging and file reservations |
| CASS (as skill) | -- | Session search as an agent-accessible tool |
| CM (as skill) | -- | Procedural memory injection |
| Meta Skill | -- | Skill search and management |

**Hooks:**

| Hook Type | Tool | Purpose |
|-----------|------|---------|
| PreToolUse | DCG | Blocks destructive commands before execution |
| PreToolUse | RCH | Intercepts cargo builds, offloads to remote |

> *"It helps if you structure the MCP tools a certain way and document it well. There's a blurb I also provide to add to your agents dot md file that helps a lot."*
> -- Jeffrey Emanuel

**Common issue:** Agents ignoring MCP tools despite configuration. Jeff's solution is explicit instruction in AGENTS.md, not just settings.json configuration:

> *"Getting claude code to use each of the tools consistently. The blurbs are often ignored: they use bd ready rather than bv, or ignore cm altogether. Should I add to the system prompt? Use hooks?"*
> -- ACFS community member

Document tool usage in AGENTS.md with explicit examples, and reference the tools in the Bead Execution Protocol.

---

### 3.3.4 NTM Layouts

NTM session layouts define the agent formation for a project. The key command is `ntm --robot-spawn` with agent count flags.

**Typical formations:**

| Formation | Command | Use Case |
|-----------|---------|----------|
| Solo | `ntm --robot-spawn=proj --spawn-cc=1` | Single-agent work, small projects |
| Small | `ntm --robot-spawn=proj --spawn-cc=2 --spawn-cod=1` | 2-3 agent projects |
| Standard | `ntm --robot-spawn=proj --spawn-cc=4 --spawn-cod=2 --spawn-gmi=1` | Jeff's typical (4-6 agents) |
| Heavy | `ntm --robot-spawn=proj --spawn-cc=6 --spawn-cod=3 --spawn-gmi=1` | 7-10 agents, heavy projects |
| Jeff-scale | 3 machines, 8 CC + 8 Codex per project | 50+ agents total |

**Formation composition:**

| Agent Type | Count | Role |
|------------|-------|------|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only (P11) |

**Important:** All implementation agents are fungible generalists. Do not specialize. The single exception is the commit agent, which runs P11 only and never modifies code.

**Scaling rule:** Scale based on bead graph size and budget. A project with 200 beads does not need 15 agents. A project with 2,000 beads might.

---

### 3.3.5 Git Conventions

Git is a first-class coordination surface in ACFS. Commit messages, branch structure, and commit grouping all follow specific conventions.

**Commit messages must include bead IDs:**
```
feat(buffer-pool): implement write-back cache [bead-142]

Adds the write-back cache with dirty-page tracking and LRU eviction.
Configurable flush interval (default 5s). Includes unit tests for
eviction under memory pressure.

Co-authored-by: RedSnow <agent@ntm>
```

Bead IDs in commit messages are the ground-truth coordination surface (Doctrine #6). Agents read git messages to know what is done. Agent Mail is for higher-level coordination.

**The commit agent pattern:**
- One dedicated agent runs P11 continuously
- It reads the diff, groups changes into logical commits by bead/feature slice
- It writes detailed messages with bead IDs
- It never modifies code
- It pushes after each commit round

**Commit grouping:** By bead or feature slice, not by time. If three agents worked on three different beads in the same hour, that produces three separate commits (or commit groups), not one time-based merge.

**Branch structure:** ACFS projects typically work on a single branch (main) with the commit agent handling all pushes. Multi-branch workflows are avoided because they add coordination overhead that the bead system already handles.

**What never gets committed:**
- Ephemeral files (build artifacts, cache files)
- Secrets (API keys, credentials, .env files)
- Agent session data (JSONL thoughts, Agent Mail SQLite -- these are operational, not code)

---

### 3.3.6 The Stream Deck Configuration

Jeff maps all 32+ prompts to physical Stream Deck buttons for instant dispatch. Each button sends a specific prompt verbatim to the active NTM session via `ntm --robot-send`.

> *"It's already a button on my Stream Deck. What I need is for it to be fully automated."*
> -- Jeffrey Emanuel

> *"And yeah, basically manually, but it's quick because each prompt is a button on my stream deck."*
> -- Jeffrey Emanuel

The Stream Deck is not required. The prompts work identically when typed or pasted. The Stream Deck eliminates the friction of finding the right prompt and ensures it is sent verbatim without modification. For practitioners who do not have a Stream Deck, a text expander or clipboard manager achieves the same effect.

---

## 3.4 Scaling Patterns

### 3.4.1 Formation Taxonomy

The number of agents you run is not a dial you turn to "fast." It is a formation decision that depends on project size, bead graph shape, budget, and hardware. Jeff uses a six-tier formation taxonomy:

| Formation | Agents | Typical Use | Model Mix (example) |
|-----------|--------|-------------|---------------------|
| **A** | 1 | Single-bead work, prototyping, learning the methodology | 1 CC (Opus) |
| **B** | 2--3 | Small projects, 30-50 beads | 2 CC + 1 Codex |
| **C** | 4--6 | Standard projects, 50-100 beads (Jeff's typical) | 3 CC + 2 Codex + 1 Gemini |
| **D** | 7--10 | Heavy projects, 100-200 beads | 5 CC + 3 Codex + 1 Gemini + 1 commit |
| **E** | 11--15 | Major systems (frankensqlite scale) | 6 CC + 4 Codex + 2 Gemini + 1 commit + 1-2 review |
| **F+** | 15+ | Full-scale swarms (frankenglibc) | 8+ CC + 8+ Codex + Gemini, across 3 machines |

Source: `/data/projects/Doodlestein/acfs_doc/FLYWHEEL.md` lines 287-294.

The commit agent is always a singleton. Review agents scale with the coder count: 1 reviewer per 4-5 coders. The "reality checker" role (an agent that validates against real data rather than tests) is optional, needed only for data-intensive projects.

> *"I have 16 GPT Pro accounts now. But they're both great and highly complementary. Claude has more of a 'can do' attitude. I also use Gemini for code review. More brains is better than one."*
> -- Jeffrey Emanuel

By February 2026, Jeff's operational formation was 3 Opus agents (Claude Code) + 3 Codex agents + 3 Gemini agents per project, sometimes running across 3 machines simultaneously:

> *"8 Codex + 8 Claude Code per project across 3 machines = ~50+ agents"*
> -- FLYWHEEL.md formation scale table (reconstructed from tweets)

### 3.4.2 Model Mix Ratios

Jeff's observed model allocation as of late February 2026, per his own statements:

| Model | Share of Work | Strengths |
|-------|--------------|-----------|
| **Codex** | ~50-60% | Follows instructions precisely, thorough, catches things Claude misses |
| **Claude (Opus)** | ~10-30% | "Can do" attitude, strong architecture reasoning, only model that reliably handles git hooks |
| **Gemini** | ~20% | Review duties, fresh-eyes perspective that makes both Codex and Claude look incomplete |

> *"Gemini earns 20% of mix right now, for my workload. Claude between 10-30% depending on what I am doing. Codex the rest."*
> -- Jeffrey Emanuel

> *"I thought codex was a thorough, judicious, genius engineer, and claude was competent but a little behind. After bringing gemini into the fold, codex looks lazy and claude seems totally inept."*
> -- Jeffrey Emanuel

**Behavioral differences:** Claude self-corrects iteratively (fix, test, fix, test in rapid cycles). Codex front-loads its thinking (long pause, then dumps a large coherent change). During multi-model competition (P15), Jeff asks each model for two documents: a revised plan AND an argumentative narrative against the other's approach. This "two-document debate protocol" surfaces disagreements that a simple review would miss.

**Model-role matrix (operational rules):**

| Task | Recommended Models | Avoid |
|------|-------------------|-------|
| Coding (P08) | Codex, Claude | Gemini (shell restriction issues) |
| Review (P02) | Gemini, Claude | -- |
| Commit (P11) | Claude ONLY | Codex, Gemini (git issues) |
| QA / Reality Check | Codex, Gemini | -- |

Source: `/data/projects/Doodlestein/acfs_doc/ACFS_WIZARD.md` lines 202-207.

### 3.4.3 Cost Modeling

**Subscription-based cost (Jeff's actual stack, Feb 2026):**

| Item | Monthly Cost |
|------|-------------|
| Claude Max x2 (2 accounts) | $400 |
| ChatGPT Pro (16 accounts at scale) | $3,200+ |
| Gemini (included in Google account) | $0 |
| VPS (Contabo, 64GB RAM) | $40-56 |
| Dedicated bare metal x2 (32-core AMD, 256GB RAM each) | $680 |
| **Total (full scale)** | **~$4,300+/month** |
| **Total (starter, 1 of each)** | **~$440-650/month** |

The starter cost for a solo practitioner: VPS ($40-56) + Claude Max ($200) + ChatGPT Pro ($200) = ~$440-656/month.

> *"Before claude code max i was spending ~$100 a day on sonnet"*
> -- Jeffrey Emanuel

> *"Opus basically compresses a whole engineering team into a context window. When velocity 10x's, the $200 feels less like cost and more like leverage."*
> -- Jeffrey Emanuel

> *"My token usage cost on some of these tools far exceeds the subscription cost. Makes me think the cost of tokens are subsidized by the model providers to a significant degree."*
> -- Jeffrey Emanuel

**Cost optimization levers:**

1. **Use flat-rate subscriptions (Max/Pro) instead of API billing.** Jeff's entire methodology is built on subscription tiers. At 50+ agents, per-token pricing would be ruinous.
2. **Use Sonnet/Codex for bulk execution, Opus/Gemini for review.** The cheaper, faster models handle the volume; the expensive models handle the judgment.
3. **Kill stalled agents early.** An agent burning tokens in a loop produces no value. Monitor via `git log --since="45 min ago"` and kill anything with 0 commits in that window.
4. **Context compaction is not free.** Every compaction event degrades quality. For long sessions, terminate and restart rather than accumulating compactions.

### 3.4.4 Hardware Scaling

**Single-machine ceiling:** Jeff routinely runs 50+ agents on one machine.

> *"I am often running 50+ agents on a single machine, and if you don't have something like that set up, the machine quickly becomes an unresponsive mess."*
> -- Jeffrey Emanuel

**Hardware progression (from Jeff's tweets):**

| Stage | Hardware | Agent Ceiling |
|-------|----------|---------------|
| Starter | VPS, 64GB RAM, 8-16 cores | 5-10 agents |
| Intermediate | Workstation, AMD 5975wx (32 cores) | 20-30 agents |
| Advanced | 64-core workstation + VPS fleet | 40-50 agents |
| Jeff's Feb 2026 | 3 machines (64-core primary + 2x bare metal Contabo) | 50-80+ agents |

> *"I have two dedicated bare metal servers for around $340/month each from Contabo, hosted in the US. They each have a 32-core AMD CPU and 256gb of RAM and NVME SSD."*
> -- Jeffrey Emanuel

**Scaling infrastructure (tools Jeff built to cope):**

| Problem | Tool | Purpose |
|---------|------|---------|
| Rust builds crush the primary machine | **RCH** (Remote Compilation Helper) | Offloads `cargo build` to a fleet of dedicated VPS machines |
| Disk fills up from build artifacts | **SBH** (Storage Ballast Helper) | Intelligently clears junk so RCH keeps working |
| Zombie/runaway processes | **PT** (Process Triage) | Bayesian zombie process detection |
| RAM/CPU exhaustion from swarm | **SRPS** (System Resource Protection) | Auto-deprioritize background processes |
| Temp files and stuck tests | Dedicated maintenance agent | Has SSH to all machines, clears junk and kills runaways |

> *"Running big agent swarms can be hazardous to your machine's health. It has become such a problem for me that I've been forced to create a suite of programs to help me cope with this, including: (1) remote_compilation_helper for offloading builds to a fleet of dedicated VPS machines (this is the single most critical one). (2) storage_ballast_helper for intelligently clearing out junk so you avoid filling up your primary machine. Without this, rch stops working after a few hours. (3) process_triage for helping to diagnose messed up runaway or zombie processes."*
> -- Jeffrey Emanuel

> *"It's so incredibly useful to have a dedicated agent with knowledge of and ssh access to all your machines, that is highly skilled in the art of maintaining machines that are used for big agent swarms. Basically, clearing out junk temp files and killing stuck tests and runaways."*
> -- Jeffrey Emanuel

---

## 3.5 Troubleshooting

### 3.5.1 Post-Install Issues

**Problem: `acfs doctor` reports missing tools.**

The install script is idempotent. Re-run it:

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh?$(date +%s)" | bash -s -- --yes --mode vibe
```

If individual tools are missing, check that Cargo, Node, Python, and Bun are all on PATH. ACFS tools install to `~/.cargo/bin/`, `~/.local/bin/`, and `~/node_modules/.bin/`.

**Problem: Agent Mail not starting.**

Verify the port before any agent spawn:

```bash
lsof -i :8765 || echo "START AGENT MAIL FIRST"
```

Agent Mail must be up before agents are spawned. If agents start without it, they cannot register, cannot coordinate, and will collide. This is the single most common first-session failure.

**Problem: Agent cannot find `bd`, `br`, `bv`, or other tools.**

Check AGENTS.md. The tools must be referenced with full paths or be on the agent's PATH. Agents inherit the shell environment of the tmux session they're spawned in. Verify with:

```bash
ntm --robot-send=SESSION --msg="which bd && which br && which bv"
```

### 3.5.2 Plan Quality Problems

**Symptom: Agents produce inconsistent architecture across beads.**

Root cause: the plan is ambiguous or contains contradictions. Two different agents read the same ambiguous sentence and reach opposite conclusions.

Fix: Return to Phase 2 (plan refinement). Run P04 (Plan Critique) specifically looking for ambiguous requirements. Run the Multi-Model Synthesis (P15) to surface divergent interpretations.

**Symptom: Beads keep getting added during execution.**

Root cause: the plan missed significant work. Some bead additions during execution are normal (discovered integration issues, edge cases). If more than 20% of final beads were added post-Phase 5 (QA the Beads), the plan was incomplete.

Fix: For the current project, accept the additions. For the next project, run more praise rounds (P30-P32) and ensure the premortem pass was done before converting to beads.

**Symptom: Plan quality score (SA-10) below 10/16.**

This is a hard gate. Do not proceed to beads with a score below 10. The quality score rubric covers 16 dimensions of plan quality; a low score means the plan has structural deficiencies that agents will inherit and amplify. Fix the plan first.

### 3.5.3 Bead Granularity Problems

**Symptom: Agents take 4+ hours on a single bead without completing it.**

Root cause: the bead is too large. A single bead should represent 1-4 hours of agent work. If an agent cannot complete a bead in one context window's worth of work, the bead needs to be split.

Fix: Kill the agent. Split the bead into 2-3 smaller beads with explicit dependencies. Restart.

**Symptom: Agents complete beads in under 10 minutes and the diffs are trivial.**

Root cause: the beads are too small. Micro-beads create coordination overhead (claim, execute, self-review, report, close, next) that exceeds the implementation time.

Fix: Merge adjacent trivial beads. The sweet spot is 30-120 minutes of agent execution time per bead.

**Symptom: QA rounds (P06) keep finding missing beads on every pass.**

This is normal for the first 3-5 QA rounds. Run 5-15 passes (the Playbook specifies "however many it takes"). The stop condition is when changes become reordering and renaming, not finding missing work. If you are still finding missing work after 10 passes, the plan itself is incomplete.

### 3.5.4 Agent Collisions

**Symptom: Two agents edit the same file simultaneously, causing merge conflicts.**

Root cause: advisory file reservations are not being checked, or beads that touch the same files are both marked `ready` with no dependency between them.

Fix:
1. Ensure AGENTS.md instructs agents to check file reservations before editing.
2. Add dependencies between beads that touch the same core files.
3. In extreme cases, use the commit agent more frequently (every 10-15 min instead of 20) to surface conflicts early.

> *"Agents can also observe if reserved files haven't been touched recently and reclaim them. This is critical because you want a system that's robust to agents suddenly dying or getting their memory wiped, because that happens all the time! That's why you also don't want ringleaders."*
> -- Jeffrey Emanuel

**Symptom: Agents dead-lock waiting on each other.**

Root cause: circular dependencies in the bead graph, or agents waiting for Agent Mail responses that never come ("communication purgatory").

Fix: Run `bv --robot-triage` to check for circular dependencies. Send P09 (Agent Mail Check & Continue) to stuck agents with the explicit instruction: "Don't get stuck in 'communication purgatory' where nothing is getting done; be proactive about starting tasks that need to be done."

### 3.5.5 Rogue Agents

**Symptom: Agent ignores AGENTS.md conventions, deletes files, rewrites unrelated code, or goes on a tangent.**

> *"They have a tendency to go rogue and act like wild animals (fortunately, they're at least muzzled wild animals since I'm also using my dcg tool)."*
> -- Jeffrey Emanuel

Root causes:
- **Context compaction stripped AGENTS.md from memory.** The agent lost its "constitution" and is operating on base-model instincts.
- **The bead description was vague.** The agent filled in the ambiguity with its own interpretation, which diverged from project intent.
- **The session has been running too long without a refresh.** Quality degrades over extended sessions.

Fix protocol:
1. If the agent is salvageable: send P20 (Post-Compaction Refresh): "Reread AGENTS.md so it's still fresh in your mind. Use ultrathink."
2. If the agent has already done damage: kill the session, `git checkout` the affected files, start a fresh agent with P18.
3. Preventively: install the post-compaction hook that auto-triggers AGENTS.md re-read after every compaction event.

> *"You need to have a whole section in AGENTS dot md telling it to never do stuff like that."*
> -- Jeffrey Emanuel

DCG alone has prevented catastrophic actions over 100 times across Jeff's projects. Agents also learn from DCG blocks via in-context learning: after being blocked once, they stop attempting the blocked pattern for the rest of the session.

**Defensive tools against rogue agents:**

| Tool | Protection |
|------|-----------|
| **DCG** (Destructive Command Guard) | Intercepts dangerous commands (`rm -rf`, `git push --force`, etc.) before execution. SIMD-accelerated, 50+ rule packs. |
| **SLB** (Simultaneous Launch Button) | Nuclear-launch-style safety requiring two-person confirmation for high-risk operations. |
| **SRPS** (System Resource Protection) | Prevents a single rogue agent from consuming all CPU/RAM. |
| Advisory file reservations | Limits blast radius by making agents aware of each other's working files. |

> *"Resource monitoring + hard limits is the answer. Run each agent with capped cpu/memory + auto-kill on threshold breach. Keeps one rogue agent from nuking the whole system."*
> -- ACFS community member

### 3.5.6 Cost Spiraling

**Symptom: Token spend is growing but output (commits, closed beads) is flat.**

Root causes:
- Agents stuck in retry loops on broken tests.
- Agents re-reading large files repeatedly without making progress.
- Too many review agents relative to coding agents (all review, no implementation).
- Agents in communication purgatory.

Fix protocol:
1. Check ground truth: `git log --oneline --since="1 hour ago" | wc -l`. If commits are near zero, agents are churning.
2. Check bead velocity: `br stats`. Compare beads closed in the last hour vs. the previous hour.
3. Kill agents that show no git output in 45+ minutes.
4. Rebalance the formation: more coders, fewer reviewers.
5. For chronic cost issues, move to cheaper models for bulk work (Sonnet for implementation, Opus only for review).

### 3.5.7 Context Exhaustion

**Symptom: Agent repeats failed approaches, hallucinates function signatures, ignores recent changes, or produces output that contradicts its own earlier work.**

Root cause: context window is saturated or compacted. The model has lost access to critical earlier context.

> *"Yeah, I let it auto compact a few times but always tell it to reread AGENTS dot md right after. If I notice it is being less effective I'll close it and reopen a new session."*
> -- Jeffrey Emanuel

Fix protocol:
1. **After compaction:** Send P20 immediately. This is non-negotiable: "Reread AGENTS.md so it's still fresh in your mind. Use ultrathink."
2. **If quality continues to degrade:** Kill the session. Start a fresh agent with P18. Fresh context is cheap; debugging a confused agent is expensive.
3. **For very long beads:** Break them up. If a bead exceeds one context window's worth of work, it is too large.

**Prevention:** The post-compaction hook (when working) automatically sends the AGENTS.md re-read instruction. Until the hook is reliable, manual monitoring for the compaction indicator (`C:>0` in the session status) is required.

### 3.5.8 Broken Builds

**Symptom: Tests pass for individual beads but the integrated build is broken.**

Root cause: agents implemented beads against different snapshots of the codebase. Agent A's changes assume Agent B's old code; Agent B's changes assume Agent A's old code.

Fix protocol:
1. Run the commit agent (P11) more frequently during high-parallelism phases.
2. After every commit round, all coding agents should pull the latest state.
3. Run cross-agent review (P03) specifically looking for integration seams.
4. Add integration test beads that explicitly test the interactions between components built by different agents.

> *"I already have thousands of unit tests and end to end tests to surface problems quickly."*
> -- Jeffrey Emanuel

The safety net is test coverage. Projects with comprehensive test suites catch integration failures at build time, not in production. Projects without them discover failures via user reports. The methodology demands no mocks, real data, real API calls, real end-to-end coverage precisely because mocked tests cannot detect integration failures.

### 3.5.9 The `acfs doctor` Command

`acfs doctor` is the first command of every session. It runs a preflight checklist:

| Check | What it verifies |
|-------|-----------------|
| Tool availability | `bd`, `br`, `bv`, `ntm`, `ubs`, `cass` on PATH |
| Agent Mail status | Port 8765 is listening |
| Git health | Clean working tree, no unresolved merges |
| Bead state | No orphaned `in_progress` beads (from crashed agents) |
| Disk space | Sufficient space for builds and agent temp files |
| SSH connectivity | Remote compilation hosts reachable (if configured) |

If `acfs doctor` reports issues, fix them before spawning agents. Starting a swarm with broken infrastructure compounds the problem.

---

## 3.6 Anti-Patterns and War Stories

### 3.6.1 The Mega-Prompt Trap

**Anti-pattern:** Writing enormous, multi-page prompts that attempt to specify every constraint, exception, and edge case in a single instruction.

**Why it fails:** Frontier models attend to every word (see section 1.5.2). A two-page prompt does not provide twice the guidance; it provides twice the constraints, many of which conflict or force the model into narrow solution paths that preclude better alternatives.

> *"Yeah, resist the temptation early on. Bloated prompts just confuse the model more than they guide it."*
> -- @glennsc (ACFS community)

**What to do instead:** Jeff's operational prompts (P01-P32) are deliberately short. P08 (the main execution loop) is two sentences. P20 (post-compaction refresh) is one sentence. The detail lives in AGENTS.md and the bead descriptions, not in the runtime prompt. The prompt sets intent and posture; the project infrastructure provides context.

> *"All done with 5 word prompts."*
> -- Jeffrey Emanuel (demonstrating with a screenshot)

**The rule:** If your prompt exceeds 100 words for a routine operation, you are over-specifying. Move the detail into AGENTS.md or the bead description.

### 3.6.2 Skipping the Plan

**Anti-pattern:** Jumping directly to code because "we know what we want" or "the plan is in my head."

**Why it fails:** This is the most expensive anti-pattern. By the time you discover that your architecture is wrong, you have hundreds of committed files that need to be refactored. The refactoring itself introduces bugs. The bugs need debugging. The debugging takes longer than the original planning would have.

From the FLYWHEEL_PLAYBOOK_DEFINITIVE.md:

> *"Skeleton-first development: 'Just get something working, then improve it.' No. By the time you have something working, the architecture is already compromised and you'll spend 10x longer fixing it than you would have spent planning it right."*
> -- Playbook anti-pattern #1

> *"Planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous."*
> -- The Playbook's foundational premise

**The economics:** A 6-hour planning session prevents 60 hours of agent chaos. A bad plan wastes multi-agent hours across parallel workers. The 85/10/5 split (planning/implementation/review) is not aspirational; it is the empirically observed ratio from projects that complete successfully.

**What to do instead:** Follow the phases in order. Start with a first-principles prompt (P01). Run praise rounds (P30-P32). Get multi-model feedback (P15). Score the plan (SA-10, minimum 10/16). Run the premortem. Only then convert to beads.

### 3.6.3 Over-Beading

**Anti-pattern:** Decomposing the plan into hundreds of micro-beads where each bead represents 5-15 minutes of work.

**Why it fails:** The coordination overhead per bead (claim it, execute it, self-review, report via Agent Mail, close it, pick next) is roughly constant regardless of bead size. When beads are trivially small, this overhead dominates. An agent spends more time in protocol than in implementation.

The bead QA process (P06) has a specific check for this: "is it optimally scoped?" If QA keeps finding beads that are too small, merge adjacent beads.

**The right granularity:** 30-120 minutes of agent execution time per bead. This gives enough substance for meaningful self-review (P02b) while keeping beads small enough to complete in a single context window.

### 3.6.4 Under-Reviewing

**Anti-pattern:** Running one review pass (P02 Part 1) and calling it done.

**Why it fails:** From the Playbook:

> *"One review pass: Part 1 is not enough. Part 2 catches what Part 1 missed. Part 5 catches what Part 4 missed. Part 23 still finds things. Keep going until the diffs flatline."*
> -- Playbook anti-pattern #9

Each review pass operates from a fresh vantage point. P02 Part 1 catches the obvious issues. Part 2 catches the issues that Part 1's fixes introduced. Part 5 catches the subtle architectural inconsistencies that only become visible after the obvious bugs are fixed. The stop condition is convergence: when review passes produce no substantive diffs.

Jeff runs escalation prompts when reviews plateau:
- **P27 (McCarthy Bug Hunt):** "I know for a fact that there are serious issues with this code. Think like Joe McCarthy: assume there's a spy. The bugs are there. Find them."
- **P28 (Stakes Escalation):** "Imagine your family's life depends on this code being correct. Not metaphorically. Literally."

These prompts change the model's internal prior from optimistic to paranoid. Sometimes you need paranoid.

### 3.6.5 Single-Model Monoculture

**Anti-pattern:** Using only one model (e.g., only Claude, only Codex) for all work.

**Why it fails:** Every model has systematic blind spots. Claude is strong on architecture reasoning but can miss implementation details. Codex follows instructions precisely but can be unimaginative. Gemini brings a perspective that makes the other two look incomplete. Using a single model means its blind spots become your blind spots.

> *"I thought codex was a thorough, judicious, genius engineer, and claude was competent but a little behind. After bringing gemini into the fold, codex looks lazy and claude seems totally inept."*
> -- Jeffrey Emanuel

**The multi-model advantage surfaces in three specific places:**

1. **Planning (Phase 2):** P15 (Multi-Model Synthesis) has models review each other's competing plans. The synthesis produces a plan stronger than any single model's output.
2. **Review (Phase 7):** P03 (Cross-Agent Review) instructs: "Run this with a different model than the one that wrote the code for true 'fresh eyes.'"
3. **Execution (Phase 6):** Formation D+ mixes 3 model families. Different models catch different bugs, produce different edge-case handling, and surface different architectural concerns.

**The rule:** For any project above Formation B (3+ agents), use at least 2 model families. For serious projects (Formation D+), use all 3.

### 3.6.6 Gold-Plating

**Anti-pattern:** Agents spending excessive time polishing code that works correctly, adding unnecessary abstractions, or over-engineering edge cases that will never occur in practice.

**Why it fails:** Agent time is not free. Every minute an agent spends gold-plating a working component is a minute not spent on the next bead. Over-abstraction also introduces complexity that makes future agents' work harder: they must understand the abstraction before modifying the code.

**Symptoms:**
- Agent creates elaborate type hierarchies for a module with 2 implementations.
- Agent adds caching, retry logic, and circuit breakers to a function called once at startup.
- Agent refactors working code for "elegance" during an implementation phase.

**The countermeasure:** The bead system is the countermeasure. Each bead has acceptance criteria. When the criteria are met, the bead is closed. The agent moves on. There is no "make it prettier" bead (unless you explicitly create one). The self-review prompt (P02b) looks for bugs, not beauty.

**Optimization is a scheduled phase (Phase 8), not a continuous activity.** Profiling beads (`bd-proj.flamegraph`, `bd-proj.heaptrack`) are assigned like any other work. The Playbook rule: "Profile before proposing. Prove correctness before shipping. One lever per diff."

> *"Optimization theater: Proposing performance improvements without profiling first. The Deep Performance Audit methodology exists because most 'optimizations' are guesses that don't move the needle."*
> -- Playbook anti-pattern #10

### 3.6.7 Placeholder Regression

**Anti-pattern:** An agent, while fixing a bug in one function, replaces a nearby working function with a placeholder stub ("TODO: implement this later").

**Why it happens:** The model's context includes the fix intent but not the full implementation of the adjacent function. It "simplifies" the surrounding code to focus on the fix, silently destroying working logic. This is Jeff's most hated failure mode.

**How to catch it:** The stub eliminator prompt (in section 3.2.4) specifically targets this. Run it after every fix session, not just at project end. Code review (P03) by a different model also catches it, because the reviewing model has no memory of the "simplification" that seemed reasonable to the fixing model.

### 3.6.8 Abstraction Sprawl

**Anti-pattern:** Agents create elaborate test infrastructure, utility libraries, or abstraction layers that look impressive but test nothing real or add no value.

> *"The test suite is largely performative."*
> — Jeff's assessment of an agent-generated test suite

**Why it happens:** Models are trained on codebases with extensive test infrastructure. Given freedom, they reproduce what they've seen: mock factories, test helpers, assertion utilities, base classes. The result passes all checks ("100% test coverage!") while testing nothing of substance.

**The countermeasure:** P13 (E2E Pipeline Validator) explicitly demands "without ANY mocks or fake data, fake API calls." The no-mocks rule is not about testing philosophy; it is about preventing agents from building a parallel universe where everything passes and nothing works.

### 3.6.9 The 800 LOC Review Threshold

**Anti-pattern:** Asking an agent to review a diff larger than ~800 lines of code in a single pass.

**Why it fails:** Model review quality degrades sharply past this boundary. The model starts skimming, misses interactions between early and late changes, and produces generic feedback ("looks good, consider adding error handling") instead of specific, actionable findings.

**What to do instead:** Split large reviews at the 800 LOC boundary. For a 2,400-line diff, run three separate review passes, each focused on a specific third. The overhead of three passes is lower than the cost of a single low-quality review that misses critical issues.

### 3.6.10 Ignoring Alien Artifacts

**Anti-pattern:** Treating unexpected model contributions as bugs or noise instead of evaluating them on merit.

**What alien artifacts are:** When a frontier model produces something beyond what was specified in the plan -- a novel algorithm, an unexpected optimization, an architectural pattern you didn't envision -- Jeff calls these "alien artifacts" (see section 2.5.4). They are one of the methodology's most valuable outputs.

> *"The 'alien artifact' stuff has specific meaning in my case; I have these 'skills' that explain what these things mean."*
> -- Jeffrey Emanuel

**Why ignoring them fails:** Alien artifacts represent the model's ability to exceed your own engineering knowledge. Phase 3 (Alien Artifacts) exists specifically to harvest these contributions before the plan is frozen into beads. If you skip this phase, you get only what you could have thought of yourself, which defeats half the point of using frontier AI.

**The spec evolution analysis (Phase 9) explicitly tracks alien artifacts in bucket #8:** "Alien artifact additions -- where AI pushed beyond your vision." Over multiple projects, studying which alien artifacts survived to production trains your intuition for recognizing valuable model contributions.

**What to do instead:** After the plan is drafted and refined but before converting to beads, run an alien artifact pass. Ask the model: what would make this dramatically better? What haven't we thought of? What would a domain expert add? Then evaluate each proposal on merit and integrate the good ones.

### 3.6.11 The Incrementalism Trap

**Anti-pattern:** Accepting the model's conservative, "practical" first output without pushing for something more ambitious.

**Why it happens:** Models have absorbed human incrementalism from training data. They default to cautious, under-ambitious proposals because the text they learned from was written by people managing expectations.

> *"It has unfortunately learned incrementalism under the guise of being 'practical' from reading too much written by humans who were trying to sandbag and control expectations so they could phone it in and not get fired. It's possible to shake them out of it, though."*
> — Jeffrey Emanuel

**How to shake them out of it:** The praise rounds (P30-P32) exist precisely for this. P30 explicitly says the first output "barely scratches the surface and is light years away from being OPTIMAL." The Innovation Boost (P26) asks for transformative, not incremental improvements. Without these prompts, the model's default incrementalism becomes your project's ceiling.

### 3.6.12 War Story: The Python-to-Rust Agent Mail Rewrite

The most concrete scaling war story from the source material. Jeff's Python Agent Mail worked at moderate scale (15-20 concurrent agents). When he pushed to 80 agents across 3 machines, it collapsed:

> *"The Python version worked when it was 15-20 agents going at once, but when it's 80 of them, it was one problem after another. The rust version has been rock solid reliable and could easily scale to literally thousands of agents (not that I had enough ram for that)."*
> -- Jeffrey Emanuel

But the Rust rewrite was not a simple port. It required first porting two upstream Python libraries (fastmcp and sqlmodel) to Rust, plus building two entirely new Rust libraries (asupersync and FrankenTUI):

> *"I waited to do this because I first had to port the fastmcp and sqlmodel Python libraries to Rust, which are fairly complex. And I also wanted to finish my asupersync library and FrankenTUI so I could use those for everything."*
> -- Jeffrey Emanuel

This is the flywheel in action: FrankenTUI feeds into Agent Mail Rust, which feeds into the next project. Each tool becomes infrastructure for the next rotation. The Rust Agent Mail port stacks 10 self-built libraries.

**Lessons from this war story:**
1. **Scale reveals architectural limits that tests do not.** 15 agents worked. 80 did not. The failure mode was not a bug but a design ceiling.
2. **The rewrite was only possible because of prior tool investments.** Without the planning methodology, the Rust port would have been a multi-month effort. With the flywheel, it was "I'll probably finish the Rust Agent Mail port today."
3. **The flywheel compounds.** Each project consumes previous projects as libraries and becomes infrastructure for the next. This compounding is the "actual flywheel" -- the planning methodology is the engine that makes each rotation clean enough to build on.
