---
icon: lucide/check-square
---

# Phase 0 -- Prerequisites

Environment is ready for agent-driven development.

---

> "Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · 1,442 likes · Source of prompt EX-03

> "These little blurbs [in AGENTS.md] are so critical, and why they should focus on convincing rather than ordering the agents! Moral suasion ain't just for humans..."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984403875714027571))

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

## AGENTS.md Template

Adapt this to your project. Delete sections that don't apply. Add project-specific rules.

```markdown
# AGENTS.md — <PROJECT_NAME>

## What This Project Is
<2-3 sentences. What it does, who it's for, what stack it uses.>

## Tool Rules
- Use `br` (beads_rust) for all bead operations, not `bd`
- Use `bv --robot-triage` to pick your next bead
- Use Agent Mail for coordination (register on first spawn)
- Do NOT use `git push` — the commit agent handles this

## Model Selection
| Task type            | Model              |
|:---------------------|:-------------------|
| Implementation       | Claude Code (Opus) |
| Review               | Gemini             |
| Plan critique        | Extended reasoning  |
| Commits              | Any (commit agent)  |

## Safety Constraints — NEVER Do These
- Force push to any branch
- Delete production data or databases
- Commit secrets, .env files, or credentials
- Skip tests to close a bead faster
- Modify AGENTS.md without human approval

## Repository Conventions
- Commit messages: `feat|fix|refactor|test|docs(<scope>): <description>`
- Include bead ID in commit message: `feat(auth): implement JWT refresh [bd-4.2.1]`
- Branch naming: `feat/<bead-id>-<short-description>`
- All code must pass `cargo test` / `npm test` before bead close

## Bead Execution Protocol (MANDATORY)
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Mark it `in_progress`: `br start <id>`
3. Implement it completely
4. Before closing: read over ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
5. Only then: `br close <id>`
6. Communicate to fellow agents via Agent Mail
7. Pick next bead. Repeat.

Do NOT commit (the commit agent handles this).
Do NOT stop between beads. Keep the loop going.

## Coordination Rules
- Check Agent Mail at the start of each bead cycle
- Respond to blocking messages before starting new work
- If blocked, message the blocking agent and pick a different bead
- Do NOT get stuck in "communication purgatory" — ship code

## Key Files
- `PLAN.md` — canonical plan (read-once, do not reopen after beads exist)
- `master-todo-bead-map.md` — execution surface
- `AGENTS.md` — this file (the constitution)
```

## About Beads

[Beads](https://github.com/steveyegge/beads) is a dependency-aware task graph. Three structural levels: Epics, Tasks, Subtasks. Each bead carries enough context for agents to work without asking questions.

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
