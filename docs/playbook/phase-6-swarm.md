---
icon: lucide/users
---

# Phase 6 -- Implement with Agent Swarm

Deploy coordinated agents to execute beads systematically.

---

> "Three Opus 4.5 agents with Claude Code, three gpt-5-codex-max agents in Codex, and three Gemini 3 agents in gemini-cli, all communicating with each other via my mcp agent mail system."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977))

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

## The Perception Loop

Source: [Swarm Steering Cookbook](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)

Every 30-60 seconds during active swarms, check state:

| I see | It means | Do |
|:------|:---------|:---|
| Spinner + output flowing | Working normally | Wait 2-5 min |
| Spinner + stale 15+ min | Ultrathink (deep reasoning) | Verify tokens moving. Do NOT interrupt. |
| Permission prompt on screen | Needs you NOW | Approve immediately |
| Shell prompt, no spinner, 5+ min | Finished or crashed | Assign next bead |
| Context usage > 80% | Approaching compaction | Let it finish current bead, then EX-04 |

Check at least two sources before concluding anything. Terminal output can be stale. Session logs are ground truth.

## Patience Calibration

The #1 operator mistake is an asymmetry trap: waiting 30 minutes for an unsubmitted prompt (dead time) but panic-interrupting 10 minutes of active ultrathink (productive time).

**If tokens are incrementing, the agent is WORKING. Do NOT interrupt. Do NOT Ctrl+C. Wait. Check back in 5 minutes.**

Flip the instinct: if session logs show no updates, check immediately. If session logs show activity, leave it alone.

## Formation Scale

> "OK, my upgrade to 64 cores was well-timed. I now have a disgusting number of next-gen Codex 4.6 and Codex 5.3 clankers running across 3 machines and 9 projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019571245784973452))

| Formation | Agents | When to use |
|:----------|:-------|:------------|
| A | 1 | Learning the workflow, debugging |
| B | 2-3 | Small features, quick fixes |
| C | 4-6 | Standard project (Jeff's typical) |
| D | 7-10 | Heavy projects, tight deadlines |
| E | 11-15 | Large-scale builds (frankensqlite) |
| F+ | 15+ | Feb 2026: 8 Codex + 8 Claude per project across 3 machines |

## Model-Role Matrix

Source: [ACFS Wizard](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)

Not all models are equal at all tasks. Learned the hard way:

| Role | Best model(s) | Avoid | Why |
|:-----|:-------------|:------|:----|
| Coding (EX-01) | Codex, Claude | Gemini | Shell integration issues in sandbox |
| Review (RV-02) | Gemini, Claude | -- | Gemini catches things Claude misses (orthogonal attention) |
| Commit (EX-06) | Claude ONLY | Codex, Gemini | Git edge cases (interactive rebase, merge resolution) fail |
| Alien artifacts | Gemini, o3 | -- | Formal reasoning strength |

Deploy Gemini agents expecting shell failures. Assign them to review and static analysis. Claude and Codex handle execution.

## Graceful Shutdown

Don't kill working agents. Ever.

1. Tell agents to finish: "Finish your current task, commit your work, then stop."
2. Wait for idle (don't poll).
3. Verify clean state: check for uncommitted changes.
4. Then kill the session.

Mid-kill consequence: truncated files, broken code, merge conflicts from a dead agent that can't explain what it was doing.

## When Things Break

Three failures every operator hits:

**Agent hung** (session log stale 15+ min): Nudge with Enter. If still dead, Ctrl+C, restart the agent, fire EX-03 (fresh start).

**Uncommitted files piling up** (10+ files, no commits in 20 min): Commit agent crashed. Manually commit once to clear the backlog. Respawn commit agent. Fire EX-06 immediately.

**Merge conflict** (two agents edited the same file): Stop all agents ("finish and commit, then stop"). Resolve conflicts manually. Checkpoint. Resume all agents with EX-02 (mail check & continue).

!!! important
    **Stop condition:** All beads are implemented and marked complete.
