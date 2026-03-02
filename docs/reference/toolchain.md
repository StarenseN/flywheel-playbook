---
icon: lucide/wrench
---

# The Toolchain

23 tools that make the flywheel turn. Three are load-bearing; the rest are accelerators.

The core trio works together: **Beads** decomposes work into a dependency graph, **BV** uses that graph to tell each agent what to do next, and **Agent Mail** lets agents coordinate without blocking each other. Everything else plugs into this loop.

---

## Core Tools

| Tool | What it does | Why it matters |
|:-----|:------------|:---------------|
| **[Beads](https://github.com/steveyegge/beads)** (bd/br) | Dependency-aware task graph: epics, tasks, subtasks with blocking relationships | The execution surface for all agent work. Without beads, agents randomly pick tasks or over-communicate. |
| **BV** (Beads Viewer) | DAG analysis, PageRank prioritization, `--robot-next` for automated assignment | Answers "what should I work on next?" using graph theory instead of guesswork. |
| **[Agent Mail](https://github.com/Dicklesworthstone/mcp_agent_mail)** | Async inter-agent messaging with targeted recipients, file reservations, semi-persistent identity | Coordination without blocking. No broadcast-all, no ringleaders, no single points of failure. |
| **NTM** (Named Tmux Manager) | Agent cockpit: spawn, monitor, and manage agent sessions across 80+ commands | The human's control plane for tending 9+ agents across multiple projects. |
| **UBS** (Ultimate Bug Scanner) | 1000+ detection rules via AST-grep, 8 languages, deeper than standard linting | Catches semantic issues that linters miss: dead code paths, type mismatches, suspicious patterns. |
| **CASS** (Coding Agent Session Search) | Tantivy-powered search across 11 agent formats, sub-60ms results | Find what any agent did in any session. Essential for debugging cross-agent issues. |
| **SLB** (Simultaneous Launch Button) | Nuclear-launch-style confirmation with 4 risk tiers | Prevents accidental mass operations (killing all agents, wiping all state). |
| **DCG** (Destructive Command Guard) | SIMD-accelerated pattern matching, 50+ safety rule packs | Catches dangerous commands (`rm -rf /`, `git push --force main`) before they execute. |

## Supporting Tools

| Tool | What it does |
|:-----|:------------|
| **CM** (CASS Memory) | Persistent memory layer for agents: short-term, episodic, and semantic recall across sessions |
| **CAAM** (Account Manager) | Sub-100ms auth switching between multiple AI provider accounts |
| **RU** (Repo Updater) | Multi-repo sync with AI-generated commit messages |
| **MS** (Meta Skill) | Skill management with adaptive selection (recommends which skills to use based on context) |
| **RCH** (Remote Compilation Helper) | Transparent build offloading to remote workers for heavy compiles |
| **WA** (WezTerm Automata) | Terminal hypervisor: automates multi-pane agent workflows |
| **JFP** (JeffreysPrompts CLI) | Browse and install prompts as Claude Code slash commands |
| **SRPS** (System Resource Protection) | Auto-deprioritize background processes to protect agent sessions |
| **APR** (Automated Plan Reviser) | Iterative spec refinement with extended reasoning |
| **PT** (Process Triage) | Bayesian detection of zombie and runaway processes |
| **XF** (X Archive Search) | BM25 + semantic search over Twitter/X archives |
| **TRU** (TOON Rust) | Token-optimized notation for LLM context compression |
| **MDWB** (Markdown Web Browser) | Convert websites to clean markdown for LLM consumption |
| **S2P** (Source to Prompt TUI) | Interactive code-to-prompt generator with file selection |
| **CAUT** (Usage Tracker) | Track LLM provider usage and costs across accounts |

## Setup

Transform a fresh Ubuntu VPS into a complete agentic environment:

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh?$(date +%s)" | bash -s -- --yes --mode vibe
```

Idempotent. If interrupted, re-running resumes from last completed phase.

**Monthly cost:** VPS ($40-56) + Claude Max ($200) + ChatGPT Pro ($200) = ~$440-656.

### Key Links

- [Agentic Coding Flywheel Setup](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)
- [Agent Flywheel Wizard](https://agent-flywheel.com/)
- [Jeffrey's Prompts](https://jeffreysprompts.com/)
- [MCP Agent Mail (Python)](https://github.com/Dicklesworthstone/mcp_agent_mail)
- [MCP Agent Mail (Rust)](https://github.com/Dicklesworthstone/mcp_agent_mail_rust)
