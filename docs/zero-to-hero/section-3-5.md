---
icon: lucide/bug
---

## 3.5 Troubleshooting

### 3.5.1 Post-Install Issues

**Problem: `acfs doctor` reports missing tools.**

The install script is idempotent. Re-run it:

```bash
curl -fsSL "https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh?$(date +%s)" | bash -s -- --yes --mode vibe
```

If individual tools are missing, check that Cargo, Node, Python, and Bun are all on PATH. ACFS tools install to `~/.cargo/bin/`, `~/.local/bin/`, and `~/node_modules/.bin/`.

**Problem: Agent Mail not starting.**

Verify the port before any agent spawn:

```bash
lsof -i :8765 || echo "START AGENT MAIL FIRST"
```

Agent Mail must be up before agents are spawned. If agents start without it, they cannot register, cannot coordinate, and will collide. This is the single most common first-session failure.

**Problem: Agent cannot find `bd`, `br`, `bv`, or other tools.**

Check AGENTS.md. The tools must be referenced with full paths or be on the agent's PATH. Agents inherit the shell environment of the tmux session they're spawned in. Verify with:

```bash
ntm --robot-send=SESSION --msg="which bd && which br && which bv"
```

### 3.5.2 Plan Quality Problems

**Symptom: Agents produce inconsistent architecture across beads.**

Root cause: the plan is ambiguous or contains contradictions. Two different agents read the same ambiguous sentence and reach opposite conclusions.

Fix: Return to Phase 2 (plan refinement). Run P04 (Plan Critique) specifically looking for ambiguous requirements. Run the Multi-Model Synthesis (P15) to surface divergent interpretations.

**Symptom: Beads keep getting added during execution.**

Root cause: the plan missed significant work. Some bead additions during execution are normal (discovered integration issues, edge cases). If more than 20% of final beads were added post-Phase 5 (QA the Beads), the plan was incomplete.

Fix: For the current project, accept the additions. For the next project, run more praise rounds (P30-P32) and ensure the premortem pass was done before converting to beads.

**Symptom: Plan quality score (SA-10) below 10/16.**

This is a hard gate. Do not proceed to beads with a score below 10. The quality score rubric covers 16 dimensions of plan quality; a low score means the plan has structural deficiencies that agents will inherit and amplify. Fix the plan first.

### 3.5.3 Bead Granularity Problems

**Symptom: Agents take 4+ hours on a single bead without completing it.**

Root cause: the bead is too large. A single bead should represent 1-4 hours of agent work. If an agent cannot complete a bead in one context window's worth of work, the bead needs to be split.

Fix: Kill the agent. Split the bead into 2-3 smaller beads with explicit dependencies. Restart.

**Symptom: Agents complete beads in under 10 minutes and the diffs are trivial.**

Root cause: the beads are too small. Micro-beads create coordination overhead (claim, execute, self-review, report, close, next) that exceeds the implementation time.

Fix: Merge adjacent trivial beads. The sweet spot is 30-120 minutes of agent execution time per bead.

**Symptom: QA rounds (P06) keep finding missing beads on every pass.**

This is normal for the first 3-5 QA rounds. Run 5-15 passes (the Playbook specifies "however many it takes"). The stop condition is when changes become reordering and renaming, not finding missing work. If you are still finding missing work after 10 passes, the plan itself is incomplete.

### 3.5.4 Agent Collisions

**Symptom: Two agents edit the same file simultaneously, causing merge conflicts.**

Root cause: advisory file reservations are not being checked, or beads that touch the same files are both marked `ready` with no dependency between them.

Fix:
1. Ensure AGENTS.md instructs agents to check file reservations before editing.
2. Add dependencies between beads that touch the same core files.
3. In extreme cases, use the commit agent more frequently (every 10-15 min instead of 20) to surface conflicts early.

> *"Agents can also observe if reserved files haven't been touched recently and reclaim them. This is critical because you want a system that's robust to agents suddenly dying or getting their memory wiped, because that happens all the time! That's why you also don't want ringleaders."*
> -- Jeffrey Emanuel

**Symptom: Agents dead-lock waiting on each other.**

Root cause: circular dependencies in the bead graph, or agents waiting for Agent Mail responses that never come ("communication purgatory").

Fix: Run `bv --robot-triage` to check for circular dependencies. Send P09 (Agent Mail Check & Continue) to stuck agents with the explicit instruction: "Don't get stuck in 'communication purgatory' where nothing is getting done; be proactive about starting tasks that need to be done."

### 3.5.5 Rogue Agents

**Symptom: Agent ignores AGENTS.md conventions, deletes files, rewrites unrelated code, or goes on a tangent.**

> *"They have a tendency to go rogue and act like wild animals (fortunately, they're at least muzzled wild animals since I'm also using my dcg tool)."*
> -- Jeffrey Emanuel

Root causes:
- **Context compaction stripped AGENTS.md from memory.** The agent lost its "constitution" and is operating on base-model instincts.
- **The bead description was vague.** The agent filled in the ambiguity with its own interpretation, which diverged from project intent.
- **The session has been running too long without a refresh.** Quality degrades over extended sessions.

Fix protocol:
1. If the agent is salvageable: send P20 (Post-Compaction Refresh): "Reread AGENTS.md so it's still fresh in your mind. Use ultrathink."
2. If the agent has already done damage: kill the session, `git checkout` the affected files, start a fresh agent with P18.
3. Preventively: install the post-compaction hook that auto-triggers AGENTS.md re-read after every compaction event.

> *"You need to have a whole section in AGENTS dot md telling it to never do stuff like that."*
> -- Jeffrey Emanuel

DCG alone has prevented catastrophic actions over 100 times across Jeff's projects. Agents also learn from DCG blocks via in-context learning: after being blocked once, they stop attempting the blocked pattern for the rest of the session.

**Defensive tools against rogue agents:**

| Tool | Protection |
|------|-----------|
| **DCG** (Destructive Command Guard) | Intercepts dangerous commands (`rm -rf`, `git push --force`, etc.) before execution. SIMD-accelerated, 50+ rule packs. |
| **SLB** (Simultaneous Launch Button) | Nuclear-launch-style safety requiring two-person confirmation for high-risk operations. |
| **SRPS** (System Resource Protection) | Prevents a single rogue agent from consuming all CPU/RAM. |
| Advisory file reservations | Limits blast radius by making agents aware of each other's working files. |

> *"Resource monitoring + hard limits is the answer. Run each agent with capped cpu/memory + auto-kill on threshold breach. Keeps one rogue agent from nuking the whole system."*
> -- ACFS community member

### 3.5.6 Cost Spiraling

**Symptom: Token spend is growing but output (commits, closed beads) is flat.**

Root causes:
- Agents stuck in retry loops on broken tests.
- Agents re-reading large files repeatedly without making progress.
- Too many review agents relative to coding agents (all review, no implementation).
- Agents in communication purgatory.

Fix protocol:
1. Check ground truth: `git log --oneline --since="1 hour ago" | wc -l`. If commits are near zero, agents are churning.
2. Check bead velocity: `br stats`. Compare beads closed in the last hour vs. the previous hour.
3. Kill agents that show no git output in 45+ minutes.
4. Rebalance the formation: more coders, fewer reviewers.
5. For chronic cost issues, move to cheaper models for bulk work (Sonnet for implementation, Opus only for review).

### 3.5.7 Context Exhaustion

**Symptom: Agent repeats failed approaches, hallucinates function signatures, ignores recent changes, or produces output that contradicts its own earlier work.**

Root cause: context window is saturated or compacted. The model has lost access to critical earlier context.

> *"Yeah, I let it auto compact a few times but always tell it to reread AGENTS dot md right after. If I notice it is being less effective I'll close it and reopen a new session."*
> -- Jeffrey Emanuel

Fix protocol:
1. **After compaction:** Send P20 immediately. This is non-negotiable: "Reread AGENTS.md so it's still fresh in your mind. Use ultrathink."
2. **If quality continues to degrade:** Kill the session. Start a fresh agent with P18. Fresh context is cheap; debugging a confused agent is expensive.
3. **For very long beads:** Break them up. If a bead exceeds one context window's worth of work, it is too large.

**Prevention:** The post-compaction hook (when working) automatically sends the AGENTS.md re-read instruction. Until the hook is reliable, manual monitoring for the compaction indicator (`C:>0` in the session status) is required.

### 3.5.8 Broken Builds

**Symptom: Tests pass for individual beads but the integrated build is broken.**

Root cause: agents implemented beads against different snapshots of the codebase. Agent A's changes assume Agent B's old code; Agent B's changes assume Agent A's old code.

Fix protocol:
1. Run the commit agent (P11) more frequently during high-parallelism phases.
2. After every commit round, all coding agents should pull the latest state.
3. Run cross-agent review (P03) specifically looking for integration seams.
4. Add integration test beads that explicitly test the interactions between components built by different agents.

> *"I already have thousands of unit tests and end to end tests to surface problems quickly."*
> -- Jeffrey Emanuel

The safety net is test coverage. Projects with comprehensive test suites catch integration failures at build time, not in production. Projects without them discover failures via user reports. The methodology demands no mocks, real data, real API calls, real end-to-end coverage precisely because mocked tests cannot detect integration failures.

### 3.5.9 The `acfs doctor` Command

`acfs doctor` is the first command of every session. It runs a preflight checklist:

| Check | What it verifies |
|-------|-----------------|
| Tool availability | `bd`, `br`, `bv`, `ntm`, `ubs`, `cass` on PATH |
| Agent Mail status | Port 8765 is listening |
| Git health | Clean working tree, no unresolved merges |
| Bead state | No orphaned `in_progress` beads (from crashed agents) |
| Disk space | Sufficient space for builds and agent temp files |
| SSH connectivity | Remote compilation hosts reachable (if configured) |

If `acfs doctor` reports issues, fix them before spawning agents. Starting a swarm with broken infrastructure compounds the problem.

---


---

!!! tip "Related pages on this site"

    - [Anti-Patterns Quick Reference](../reference/anti-patterns.md)

