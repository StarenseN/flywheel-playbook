---
icon: lucide/dollar-sign
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

!!! tip "Related pages on this site"

    - [Doctrine](../reference/doctrine.md)

