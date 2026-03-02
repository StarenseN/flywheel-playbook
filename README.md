# The Flywheel Planning Playbook

**Complete guide to building software with AI agent swarms.** 12 principles, 9 phases, 47 prompts, 14 doctrine rules, 14 anti-patterns. Based on Jeffrey Emanuel's ([@doodlestein](https://x.com/doodlestein)) agentic coding methodology.

**[Read the full playbook](https://starensen.github.io/flywheel-playbook/)**

---

## The Premise

Most people using AI agents are doing it wrong. They open Claude, describe what they want, accept whatever comes back, and move on. Then they wonder why the code is brittle and the architecture is incoherent.

**Planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous.**

| Activity | How most people work | How this works |
|----------|---------------------|----------------|
| Planning | ~10% | ~85% |
| Implementation | ~70% | ~10% |
| Review/Fix | ~20% | ~5% |

And there's a deeper flywheel most people miss: **each project you build becomes a library for the next project.** The tools compound. The planning methodology is the engine that makes each rotation clean enough to build on.

---

## What's Inside

| Section | What you get | Link |
|---------|-------------|------|
| **12 Principles** | The ideas behind the workflow | [Read](https://starensen.github.io/flywheel-playbook/playbook/principles/) |
| **9 Phases** | Sequential steps from prerequisites to performance | [Read](https://starensen.github.io/flywheel-playbook/playbook/phase-0-prerequisites/) |
| **47 Prompts** | Verbatim, copy-paste, organized by category | [Read](https://starensen.github.io/flywheel-playbook/prompts/prompt-pack/) |
| **Dispatch Table** | "When in situation X, use prompt Y" | [Read](https://starensen.github.io/flywheel-playbook/reference/dispatch/) |
| **14 Doctrine Rules** | Non-negotiable rules of the methodology | [Read](https://starensen.github.io/flywheel-playbook/reference/doctrine/) |
| **14 Anti-Patterns** | Ways to destroy your project | [Read](https://starensen.github.io/flywheel-playbook/reference/anti-patterns/) |
| **Toolchain** | 23 tools that make the flywheel turn | [Read](https://starensen.github.io/flywheel-playbook/reference/toolchain/) |
| **Jeff Teaches** | Jeff's words, mined from 8,914 tweets and 385 threads | [Read](https://starensen.github.io/flywheel-playbook/jeff-teaches/) |

---

## Agent Injection

Point your AI coding agent at one of these URLs and it absorbs the full methodology in one fetch:

| Format | Size | Best for |
|--------|------|----------|
| [llms-full.txt](https://starensen.github.io/flywheel-playbook/llms-full.txt) | ~45KB | Full methodology kernel + DSL + REPL protocol + 47 prompts |
| [llms.txt](https://starensen.github.io/flywheel-playbook/llms.txt) | ~2KB | Quick site map with links |
| [Prompt Injector](https://starensen.github.io/flywheel-playbook/agent/) | ~200KB | 47 prompts + dispatch + chaining rules (HTML) |
| [Methodology Kernel](https://starensen.github.io/flywheel-playbook/agent/kernel/) | ~200KB | Full DSL with state machine + REPL (HTML) |

**Recommended:** `llms-full.txt` — plain text, fits in one context window, boots the agent into a REPL loop.

---

## Attribution

Based on [Jeffrey Emanuel's](https://jeffreyemanuel.com/) agentic coding methodology. Extended by [@voidserf](https://x.com/voidserf). Synthesized from forensic analysis of 5 repos, 69 sessions, and 8,914 tweets.

Sources:
- [Agentic Coding Flywheel Setup](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup) (GitHub)
- [Agent Flywheel Wizard](https://agent-flywheel.com/) (Website)
- [Jeffrey's Prompts](https://jeffreysprompts.com/) (Website)
- [Beads](https://github.com/steveyegge/beads) (GitHub)
- [@doodlestein](https://x.com/doodlestein) (Twitter)
