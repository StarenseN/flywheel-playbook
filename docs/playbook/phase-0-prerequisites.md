---
icon: lucide/check-square
---

# Phase 0 -- Prerequisites

Environment is ready for agent-driven development.

---

Before you start planning, make sure the scaffolding is in place:

- [ ] Your repo has an `AGENTS.md` explaining tool rules, model selection, safety constraints, and coordination rules
- [ ] Beads is initialized (`bd` or `br`) for the project
- [ ] You've decided where your canonical plan lives (e.g., `PLAN.md` in repo root)
- [ ] Agent coordination tooling is available (Agent Mail, if using multi-agent)
- [ ] You have access to a strong reasoning model for plan critique (web app with extended thinking)

## AGENTS.md is the project constitution

Not documentation. An operating manual for minds that will wake up without memory of this conversation. It tells them exactly what this project is, what tools exist, what the bead workflow looks like, what they must never do, and where to find things. I write this myself or heavily direct it. This is not optional.

AGENTS.md covers:
- **Tool rules** -- which tools agents can use and how
- **Model selection guidance** -- which model to use for which task type
- **Safety constraints** -- what agents must never do (force push, delete prod data, etc.)
- **Repository conventions** -- commit message format, branch naming, test requirements
- **Coordination rules** -- how agents claim work and communicate (if multi-agent)

The mandatory section is the Bead Execution Protocol:

```
## Bead Execution Protocol (MANDATORY)
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Implement it completely
3. Before closing the bead: read over ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
4. Only then: `br close <id>`
5. Communicate to fellow agents via Agent Mail
6. Pick next bead. Repeat.
Do NOT commit (the commit agent handles this).
Do NOT stop between beads. Keep the loop going.
```

!!! note
    Notice the self-review step. This is RV-01 Self-Review baked into the workflow. The agent reviews its own code before closing the bead. Not a hook. Not a separate agent. The coder, with fresh eyes, on its own work.

## About Beads

[Beads](https://github.com/steveyegge/beads) is a dependency-aware task graph with structural elements: Epics, Tasks, and Subtasks. Each bead is self-documenting with sufficient context for agents to work independently.

## Agent Mail

- Python: [mcp_agent_mail](https://github.com/Dicklesworthstone/mcp_agent_mail)
- Rust: [mcp_agent_mail_rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust)

## Setup

Transform a fresh Ubuntu VPS into a complete agentic environment in 30 minutes:

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh?$(date +%s)" | bash -s -- --yes --mode vibe
```

Idempotent; if interrupted, re-running resumes from last completed phase.

!!! important
    **Stop condition:** All checklist items completed.
