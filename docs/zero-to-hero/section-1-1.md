---
title: "1.1 The Problem This Solves"
icon: lucide/alert-triangle
---

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

!!! tip "Related pages on this site"

    - [1.2 What the Flywheel Is](section-1-2.md) -- How the methodology addresses these problems
    - [1.4 The Economics](section-1-4.md) -- The economic case for planning over debugging
    - [12 Principles](../playbook/principles.md) -- The full set of operational rules

