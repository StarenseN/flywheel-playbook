---
icon: lucide/alert-triangle
---

# Anti-Patterns

14 ways to destroy your project. Each one looks reasonable. Each one will cost you days.

---

> "Friends don't let friends use git worktrees for agent swarms for high-velocity development. I give the clankers hell if I ever catch them using worktrees."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022355083845894159))

> "Don't get stuck in "communication purgatory" where nothing is getting done."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977))

### 1. Skeleton-first development

"Just get something working, then improve it." By the time you have something working, the architecture is baked. Changing it costs 10x what planning it right would have. The plan exists to prevent structural debt. Skip it and you'll spend the project paying it down.

→ See [Phase 1 — Draft](../playbook/phase-1-draft.md)

### 2. Specialized agents

"You are an expert senior backend engineer with 15 years of experience in..." Frontier models understand intent without roleplay constraints. Assigning specializations narrows the solution space and creates coordination overhead (who handles cross-cutting concerns?). Fungible generalists pick up whatever bead is next.

→ See [Doctrine #3](doctrine.md)

### 3. Communication purgatory

Agents messaging each other endlessly about who should do what, and nobody writes code. The bead system and AGENTS.md solve this architecturally: agents pick the highest-leverage bead and start working, then inform peers. If communication purgatory persists, your AGENTS.md is unclear or your beads aren't granular enough.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 4. Separate design docs

The plan is the design doc is the architecture doc. One canonical document, one source of truth. Two documents drift apart; agents read the stale one and build the wrong thing.

→ See [Doctrine #1](doctrine.md)

### 5. Skipping the praise rounds

"I'll just go straight to analytical critique." Critique refines what exists. Praise pushes (PL-03 through PL-05) expand the solution space first, encouraging the model past its default conservative output. Without expansion, you refine a mediocre plan into a polished mediocre plan.

→ See [Phase 2 — Refine](../playbook/phase-2-refine.md)

### 6. Mocked tests

Tests with mocks prove that the mock works, not that the code works. In a multi-agent swarm, this is catastrophic: Agent A writes a function with mocked tests that pass. Agent B builds on top of A's work, trusting the green test suite. When the real API behaves differently from the mock, both agents' work breaks. Use real data, real API calls, real end-to-end coverage.

→ See [Doctrine #9](doctrine.md)

### 7. Reopening the plan during execution

After beads exist, PLAN.md is closed. The bead-map is the execution surface. If an agent re-reads the plan during EX-01 and starts second-guessing bead definitions, it stalls. If the beads are wrong, fix the beads directly. The plan served its purpose; the beads carry it forward.

→ See [Doctrine #7](doctrine.md)

### 8. Skipping the premortem

If you can't imagine how your project fails in 6 months, you haven't thought about it enough. Optimistic planning finds what could go right. PL-11 Premortem finds what will go wrong: the dependency that gets abandoned, the API that rate-limits you, the edge case that corrupts data.

→ See [Phase 2 — Refine](../playbook/phase-2-refine.md)

### 9. One review pass

RV-02 Deep Review is a numbered series for a reason. Part 1 catches the obvious bugs. Part 2 catches what Part 1 missed because it was focused elsewhere. Part 5 catches things that only become visible after Parts 1-4 fixed the surrounding code. Keep going until the diffs flatline, meaning a full review pass produces zero or near-zero changes.

→ See [Phase 7 — Review](../playbook/phase-7-review.md)

### 10. Optimization theater

Proposing performance improvements without profiling first. "This loop is O(n^2), let me optimize it" when the actual bottleneck is a network call three functions away. The QA-08 Deep Performance Audit methodology exists because most guesses about where time is spent are wrong. Profile first, then optimize what the data shows.

→ See [Phase 8 — Performance](../playbook/phase-8-performance.md)

### 11. Git worktrees for agent swarms

Agents in separate worktrees never see each other's changes until merge time, by which point conflicts have piled up into a nightmare. All agents in one shared working directory surfaces merge conflicts immediately, when they're small and easy to resolve. Worktrees defer pain; shared directories surface it early.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 12. Sending execution prompts to fresh agents

Fire EX-01 (Execute Beads) at an agent that just spawned, and it goes rogue. It doesn't know the bead protocol, hasn't read AGENTS.md, doesn't understand file reservations or Agent Mail. EX-03 (Agent Introduction) is ALWAYS the first prompt on any fresh spawn. No exceptions. The agent needs the constitution before it can follow the rules.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 13. Assuming prompts were sent

You paste a prompt into a terminal pane and assume the agent received it. Roughly half the time, terminal paste silently fails. Always verify delivery: check that a new entry appeared in the session log within 10 seconds. If nothing appeared, nudge with Enter and check again. Never assume. Session logs are ground truth, not the terminal display.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 14. Interrupting ultrathink

An agent has been "thinking" for 12 minutes and you panic-hit Ctrl+C. Those 12 minutes of deep reasoning are now gone. The model was building toward an insight it hadn't committed yet. If session logs show tokens incrementing, the agent is working. The asymmetry trap: operators tolerate 30 minutes of dead time (unsubmitted prompt) but can't tolerate 10 minutes of active deep reasoning. Invert that instinct.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)
