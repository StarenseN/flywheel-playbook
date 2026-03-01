---
icon: lucide/users
---

# Phase 6 -- Implement with Agent Swarm

Deploy coordinated agents to execute beads systematically.

---

> "The concept works even better when the swarms contain a mixture of frontier models and agent harnesses. Opus and GPT 5.2 are way smarter together than alone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019476380904210820))

## Swarm Marching Orders (EX-01 Execute Beads)

```
Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any
messages from other agents. Execute the highest-priority ready bead. Follow the
bead execution protocol in AGENTS.md exactly. Use ultrathink.
```

That's it. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication.

## Fresh Agent Spawn (EX-03 Agent Introduction)

For new agents or after context loss:

```
First read ALL of the AGENTS.md file and README.md file super carefully and
understand ALL of both! Then use your code investigation agent mode to fully
understand the code, and technical architecture and purpose of the project.
Then register with MCP Agent Mail and introduce yourself to the other agents.
Be sure to check your agent mail and to promptly respond if needed to any messages;
then proceed meticulously with your next assigned beads. Don't get stuck in
"communication purgatory." Use ultrathink.
```

## Post-Compaction Refresh (EX-04)

When agents hit context compaction:

```
Reread AGENTS.md so it's still fresh in your mind. Use ultrathink.
```

Simple. The agent just lost half its context window. Give it the constitution back before it makes any decisions.

## Agent Coordination Rules

- **Claim before starting** -- mark beads `in_progress` before working
- **Communicate blockers and discoveries**, not just status
- **Group commits by bead/feature slice**, not by time
- **Bead IDs in commit messages are the coordination surface** -- agents read git messages to know what's done. Agent Mail is for higher-level coordination.
- **Agent failure is normal operations** -- Gemini agents will hit shell restrictions. Log it, handle it gracefully, move on.

## The Commit Agent (EX-06 Git Commit)

One agent is different: the commit agent. It does not modify code.

```
Based on your knowledge of the project, commit all changed files now in a series
of logically connected groupings with detailed commit messages for each,
and then push. Don't edit the code at all. Don't commit ephemeral files or secrets.
Follow repo conventions and hooks.
```

It reads the diff, groups changes into logical commits, writes detailed messages with bead IDs, and commits. That is its entire job. Think of it like the cleaner at a hair salon -- it does not style hair, it just keeps the floor clean so the stylists can work.

## Typical Formation

| Agent type | Count | Role |
|:-----------|:------|:-----|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only |

Scale based on bead graph size and budget. All implementation agents are fungible generalists. Don't specialize.

!!! important
    **Stop condition:** All beads are implemented and marked complete.
