---
icon: lucide/calendar
---

## 2.7 The Daily Workflow

### 2.7.1 Session Start

A typical session start sequence:

1. `acfs doctor` to verify everything is healthy.
2. `bv --robot-triage` to identify the highest-priority unblocked beads.
3. Review yesterday's Agent Mail messages and git log.
4. `ntm --robot-spawn=<project>` to spin up agents.
5. All agents: read AGENTS.md, register with Agent Mail, start executing.
6. One dedicated commit agent running EX-06 only.

The first message to every agent, every time:

> *"Before doing anything else, read ALL of AGENTS dot md."*

After every context compaction (when the model's conversation history is compressed), repeat this instruction. Agents that lose AGENTS.md from context start drifting from project conventions.

---

### 2.7.2 The Loop

The execution loop is:

1. Agents pick beads via `bv`, execute them, self-review, report via Agent Mail.
2. The human monitors via tmux panes, Agent Mail logs, and git log.
3. When the human sees a stall, they send the appropriate steering prompt.
4. When review rounds come up clean, the human moves to the next batch of beads.

Jeff cycles through 7+ projects daily, sending polishing and fixing prompts to agents that need forward progress. Each prompt is dispatched with a single button press.

> *"I like to make sure that I'm making some forward progress on every one of my active projects each day, even when I'm too busy to spend real mental bandwidth on all of them every single day."*
> — Jeffrey Emanuel

The human is not needed for every cycle. During the execution phase, the human's role is strategic steering, not line-by-line oversight.

---

### 2.7.3 Context Management

Context degradation is the primary failure mode of long sessions. After context compaction, the model loses details from earlier in the conversation. The symptoms: repeating failed approaches, ignoring project conventions, or drifting from the current bead's scope.

The response protocol:

| Symptom | Action |
|---------|--------|
| Agent ignores AGENTS.md conventions | Send MT-01 (re-read AGENTS.md) |
| Agent repeats the same failed approach | Kill session, start fresh |
| Agent is less effective after compaction | Send EX-04 (Post-Compaction Refresh) |
| Agent has been running 4+ hours on one bead | Kill session, narrow the bead scope, start fresh |

Fresh sessions are cheap. Debugging a context-polluted session is expensive. When in doubt, start over.

> *"If I notice it is being less effective I'll close it and reopen a new session."*
> — Jeffrey Emanuel

> *"Craft the prompts to be completely in single session context. You will love the output more the less context rot creeps in."*
> — ACFS community member

**On token conservation:** A common instinct is to pipe build output through wrapper scripts that truncate it, saving context tokens. Jeff rejects this: "Nah, I'd rather burn the tokens. The scripts never work right." His approach is to let full output stream into context and scale via multiple subscriptions rather than compressing context. Rich context produces better decisions than saved tokens.

**Experimental workaround:** A PreCompact hook can write a marker file, and a UserPromptSubmit hook can detect that marker and automatically re-inject critical context (AGENTS.md re-read, current bead state) after compaction. This is not yet reliable across all models but is the direction of travel for automated context recovery. A related anti-pattern: "ringleader agents" (a coordinator agent that manages other agents). Ringleader agents fail because when the coordinator's context compacts, the entire swarm loses coherence. Flat coordination via Agent Mail and bead priorities is more resilient.

---

### 2.7.4 End of Day

End of session runs:
1. EX-06 (final commit round): commit all outstanding changes.
2. RV-02 (final review): one last fresh-eyes pass.

The bead system, Agent Mail history, and git log serve as persistent state. A fresh agent tomorrow can reconstruct context from these three sources. Elaborate handoff documents are unnecessary; the infrastructure is the handoff.

> *"The messages themselves act like a kind of repository (as does the plan document), so you can resume again from a fresh context."*
> — Jeffrey Emanuel

---

### 2.7.5 Marathon Sessions vs. Structured Sprints

Agents running on well-structured projects with comprehensive test suites can operate autonomously for 8-17+ hours. This is documented and verified.

> *"I've seen a lot of skepticism recently from people about the claims around how long agents can go autonomously. People think it's BS because they haven't observed it. It's real. 17+ hours."*
> — Jeffrey Emanuel

The key factor is not the model's capability but the project's structure. An agent working against a well-defined bead in a project with good tests stays grounded: it executes a task, runs tests, handles failures, and moves on. The work itself provides the context that prevents drift.

> *"Biggest factor in long runs isn't intelligence, it's having a well-structured repo with good tests to anchor against."*
> — Agent self-report from the ACFS community

You do not need to run marathon sessions. Structured sprints (spin up agents, run review cycles until clean, kill, restart) work well and give the human more control. The marathon pattern is most useful for overnight or unattended execution, where the human reviews results the next morning.

---

!!! tip "Related pages on this site"

    - [Troubleshooting](section-3-5.md)

