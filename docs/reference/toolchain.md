---
title: Toolchain
layout: default
parent: Reference
nav_order: 4
---

# The Toolchain
{: .fs-8 }

23 tools that make the flywheel turn.
{: .fs-5 .fw-300 }

---

## Core Tools

| Tool | What it does | Why it matters |
|:-----|:------------|:---------------|
| **[Beads](https://github.com/steveyegge/beads)** (bd/br) | Dependency-aware task graph | The execution surface for all agent work |
| **BV** (Beads Viewer) | DAG analysis, PageRank prioritization, `--robot-next` | Tells agents what to work on next |
| **[Agent Mail](https://github.com/Dicklesworthstone/mcp_agent_mail)** | Gmail-like inter-agent messaging | Async coordination without blocking |
| **NTM** (Named Tmux Manager) | Agent cockpit, 80+ commands | Spawns, monitors, and manages agent sessions |
| **UBS** (Ultimate Bug Scanner) | 1000+ detection rules, AST-grep, 8 languages | Catches issues linting misses |
| **CASS** (Coding Agent Session Search) | 11 agent formats, Tantivy-powered <60ms | Find what agents did across sessions |
| **SLB** (Simultaneous Launch Button) | Nuclear-launch-style safety, 4 risk tiers | Prevents accidental mass operations |
| **DCG** (Destructive Command Guard) | SIMD-accelerated safety net, 50+ packs | Catches dangerous commands before execution |

## Supporting Tools

| Tool | What it does |
|:-----|:------------|
| **CM** (CASS Memory) | Three-layer cognitive architecture for agents |
| **CAAM** (Account Manager) | Sub-100ms auth switching between AI accounts |
| **RU** (Repo Updater) | Multi-repo sync with AI-driven commits |
| **MS** (Meta Skill) | Skill management with UCB bandit optimization |
| **RCH** (Remote Compilation Helper) | Transparent build offloading to remote workers |
| **WA** (WezTerm Automata) | Terminal hypervisor for multi-agent automation |
| **JFP** (JeffreysPrompts CLI) | Browse/install prompts as Claude Code skills |
| **SRPS** (System Resource Protection) | Auto-deprioritize background processes |
| **APR** (Automated Plan Reviser) | Iterative spec refinement with extended reasoning |
| **PT** (Process Triage) | Bayesian zombie process detection |
| **XF** (X Archive Search) | BM25 + semantic search over Twitter archives |
| **TRU** (TOON Rust) | Token-optimized notation, 30-50% reduction |
| **MDWB** (Markdown Web Browser) | Convert websites to markdown for LLM consumption |
| **S2P** (Source to Prompt TUI) | Interactive code-to-prompt generator |
| **CAUT** (Usage Tracker) | Track LLM provider usage and costs |

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
