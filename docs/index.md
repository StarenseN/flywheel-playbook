---
icon: lucide/home
---

# The Flywheel Planning Playbook

Definitive guide to building software with AI agent swarms.

[Get Started](playbook/principles.md) |
[Prompt Catalog](prompts/prompt-pack.md) |
[Zero to Hero](zero-to-hero/index.md)

!!! tip "2026-09 update — supersede pass"
    Latest pass against the July–September 2026 corpus: **3 new prompts** (PL-14 Agent Ergonomics Pass, EX-07 Encouragement Rotation, RV-10 Fresh-Session Critique), a dated [Model Roster](reference/models.md) (position at 2026-09), and a [Supersede policy + log](reference/supersede.md) recording the 14 positions replaced since July 2026. Current recommendations now carry freshness markers where models are concerned; dated quotes stay untouched.

---

Most people using AI agents are doing it wrong. Not slightly wrong. Catastrophically wrong. They open Claude, describe what they want, accept whatever comes back, and move on. Then they wonder why the code is brittle, the architecture is incoherent, and they spent three days debugging something an agent wrote in four minutes.

**Planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous.**

| Activity | How most people work | How this works |
|:---------|:---------------------|:---------------|
| Planning | ~10% | ~85% |
| Implementation | ~70% | ~10% |
| Review/Fix | ~20% | ~5% |

That's not a typo. Eighty-five percent planning. A 6-hour planning session prevents 60 hours of agent chaos. A bad plan wastes multi-agent hours across parallel workers. A good plan pays for itself before the first line of code.

There is a deeper flywheel most people miss: **each project you build becomes a library for the next project.** The tools compound. FrankenTUI feeds FrankenSQLite feeds FrankenTerm feeds Agent Mail Rust. The February 2026 Rust Agent Mail port stacks 10 self-built libraries. That compounding -- each project consuming previous projects and becoming infrastructure for the next -- is the actual flywheel. The planning methodology is just the engine that makes each rotation clean enough to build on.

---

## What's in this guide

| Section | What you get |
|:--------|:------------|
| [12 Principles](playbook/principles.md) | The ideas behind the workflow |
| [9 Phases](playbook/phase-0-prerequisites.md) | Sequential steps from prerequisites to performance |
| [50 Prompts](prompts/prompt-pack.md) | Verbatim, copy-paste, organized by category |
| [Dispatch Table](reference/dispatch.md) | "When in situation X, use prompt Y" |
| [Toolchain](reference/toolchain.md) | 23 tools that make the flywheel turn |
| [Model Roster](reference/models.md) | Which model plays which role, dated at 2026-09 |
| [Supersede Log](reference/supersede.md) | Supersede policy + 14 replaced positions with dates and reasons |
| [Anti-Patterns](reference/anti-patterns.md) | 14 ways to destroy your project |
| [Doctrine](reference/doctrine.md) | 16 non-negotiable rules |
| **[Zero to Hero](zero-to-hero/index.md)** | **The complete guide: strategy, operations, and reference in one place** |

---

## Attribution

Based on [Jeffrey Emanuel's](https://jeffreyemanuel.com/) agentic coding methodology. Incorporates the [Flywheel Planning Playbook](https://gulp.github.io/flywheel-planning/) by [@voidserf](https://x.com/voidserf). Extended with forensic analysis of 5 repos, 69 sessions, and 8,914 tweets.

- [Agentic Coding Flywheel Setup](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)
- [Agent Flywheel Wizard](https://agent-flywheel.com/)
- [Jeffrey's Prompts](https://jeffreysprompts.com/)
- [Beads](https://github.com/steveyegge/beads)
