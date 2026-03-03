---
icon: lucide/users
---

## 2.6 Scaling to Multi-Agent

> *"When the swarm is working correctly, I am not a programmer. I am a conductor. I read the JSONL logs to understand what the agents are thinking. I watch the bead tracker. I steer with prompts when something goes sideways. I review commits."*
> — Jeffrey Emanuel

The shift from programmer to conductor is the conceptual leap of multi-agent scaling. You are not writing code. You are steering agents who write code.

### 2.6.1 Why and When to Scale

A single agent working through a bead queue produces roughly 5-10 beads per day. For a 50-bead project, that is a week of serial execution. For a 300-bead project, it is over a month.

The inflection point for scaling is when the serial execution time exceeds your patience or your deadline. In practice, this is around 80-100 beads.

Start with 3-4 agents. This is the range where coordination overhead is minimal and the throughput gain is significant.

> *"I've found that things work pretty well with 3-4 agents. But if you have a big and complex plan of work that can effectively be decomposed, you can handle 5 of them or even more."*
> — Jeffrey Emanuel

Scaling beyond 5 agents requires the full coordination infrastructure: Agent Mail for messaging, NTM for session management, advisory file reservations, and a dedicated commit agent. Without these, agents collide.

> *"I find scaling past 2-3 agents has a pretty steep falloff for oversight quality in any non-greenfield work."*
> — ACFS community member

This observation is accurate for setups without coordination infrastructure. With Agent Mail, bead-level task decomposition, and file reservations, the ceiling is much higher.

---

### 2.6.2 Agent Roles

Implementation agents are **fungible generalists**. Do not assign "the auth agent" or "the database agent." Each agent picks the next available bead, regardless of domain. Specialization creates bottlenecks; generalization allows any agent to take any ready bead.

The one exception: the **commit agent**. This is a single Claude Code instance running only the commit prompt (EX-06). It monitors the working directory, groups changes into logical commits, writes detailed messages with bead IDs, and pushes. It never modifies code.

A typical formation for a serious project:

| Agent type | Count | Role |
|---|---|---|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only |

Model mixing is deliberate. Different models have different strengths, and using multiple models increases the probability that at least one agent catches an issue that others miss.

---

### 2.6.3 Agent Mail

Agent Mail is the async messaging layer that enables multi-agent coordination. Agents register identities, send and receive messages, and declare file reservations.

The canonical onboarding prompt for every new agent session:

> *"Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks."*
> — Jeffrey Emanuel

Key capabilities:
- **Message routing:** agents send status updates, ask questions, report blockers.
- **Advisory file reservations:** agents "call dibs" on files they're working on. Reservations are not rigidly enforced and expire automatically.
- **Threading:** conversations between agents maintain context across multiple messages.
- **Persistent state:** messages persist in a SQLite database. A fresh agent can read the message history to reconstruct context.

> *"The advisory file reservation system is critical to how I get such crazy results from all these agents."*
> — Jeffrey Emanuel

The anti-pattern to avoid is "communication purgatory," where agents exchange messages endlessly without producing code. The coordination protocol in AGENTS.md should emphasize: be proactive about starting work, inform others when you do, and communicate blockers, not just status.

---

### 2.6.4 NTM + tmux

NTM (Named Tmux Manager) is the agent cockpit. It manages tmux sessions where agents run, providing 80+ commands for session management, prompt broadcasting, and agent monitoring.

Basic workflow:

```bash
ntm new coder1          # create a named session
ntm new coder2          # another
ntm new reviewer        # and another
ntm attach coder1       # jump into any session
```

For bulk spawning:

```bash
ntm --robot-spawn=myproject --spawn-cc=4 --spawn-cod=4 --spawn-gmi=2
```

This creates 10 agent sessions in one command. Each gets a randomly generated color+noun name (RedSnow, BrownLake, PurpleBear) that agents use to identify themselves in Agent Mail.

NTM enables a single human to monitor and steer many agents simultaneously. When the human sees a stall in one pane, they send a prompt to that pane. When they see a bug, they send the review prompt. All prompts are sent verbatim from the master prompt set.

---

### 2.6.5 Worktrees vs. Shared State

See [section 2.4.6](section-2-4.md#246-shared-state-vs-worktrees) for the full discussion. In brief: the methodology recommends shared-state by default, with advisory file reservations and bead-level decomposition preventing collisions. Worktrees are an alternative that some practitioners use successfully, particularly with jujutsu (jj) instead of git.

---

### 2.6.6 Coordination Patterns

The coordination flow for a running multi-agent session:

1. **Claim:** Agent claims a bead by marking it `in_progress`.
2. **Execute:** Agent implements the bead following AGENTS.md protocol.
3. **Self-review:** Agent rereads its own code with fresh eyes before closing.
4. **Report:** Agent sends a status message via Agent Mail.
5. **Close:** Agent marks the bead as complete via `br close <id>`.
6. **Next:** Agent picks the next highest-priority ready bead via `bv --robot-triage`.

The human steers reactively, not proactively:
- Sees stall → sends EX-02 (Agent Mail Check & Continue)
- Sees bug → sends RV-02 (Deep Review)
- Sees drift → sends EX-04 (Post-Compaction Refresh)

All steering prompts are verbatim from the master prompt set. The human does not improvise instructions; they select the appropriate prompt and fire it.

> *"Then they just keep cranking on their own for a really long time. And you don't need to supervise them much."*
> — Jeffrey Emanuel

The trust model is: wide autonomy with mutual oversight.

> *"I'm ultimately in charge, but I grant them very wide latitude. But with extreme oversight by means of mutual inspection and verification. That is, they mostly keep each other honest."*
> — Jeffrey Emanuel

---

### 2.6.7 The Integration Cycle

After a batch of beads is completed, integration happens:

1. The commit agent groups all changes into logical commits with bead IDs.
2. A fresh agent runs a cross-agent review (RV-03): examine code written by other agents, looking for inconsistencies, bugs, and integration issues.
3. The full test suite runs. Failures are filed as new beads.
4. Fix beads are executed in the normal way.

Integration issues are expected, not failures. When 6 agents implement 20 beads in parallel, some integration bugs are inevitable. The test suite catches them; the review cycle fixes them; the process continues.

> *"If it does then another one will fix it. And I already have thousands of unit tests and end to end tests to surface problems quickly."*
> — Jeffrey Emanuel

The safety net that makes all of this viable is test coverage. Projects with 10,000+ unit and end-to-end tests can tolerate aggressive parallelism because regressions surface immediately.

---

!!! tip "Related pages on this site"

    - [Swarm Operations](../playbook/phase-6-swarm.md)
    - [Jeff Teaches](../jeff-teaches.md) — Jeff's methodology in his own words

