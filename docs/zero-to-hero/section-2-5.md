---
icon: lucide/play
---

## 2.5 Execution (Phases 6-8)

### 2.5.1 Your First Bead

The execution prompt is short:

> *"Reread AGENTS.md so it's still fresh in your mind. Now check Agent Mail for any messages from other agents. Execute the highest-priority ready bead. Follow the bead execution protocol in AGENTS.md exactly. Use ultrathink."*

That is the entire prompt. No task assignment. No role description. The bead system handles coordination. Agent Mail handles communication. AGENTS.md defines the behavioral rules. The prompt simply says: "Go."

Start with a leaf node in the DAG (a bead with no unsatisfied dependencies). Watch the agent work. Review the output. Close the bead. This is the "hello world" of the methodology; once you've done it once, the loop for the rest of the project is the same.

---

### 2.5.2 Numbered Fix Sessions

After initial implementation, review happens in **numbered fix sessions**. Each session gets a sequential ID: Part 1, Part 2, Part 3. A long project might reach Part 23.

The deep review prompt (RV-02) does not prescribe what to look for. Instead, it asks the agent to explore the codebase with fresh eyes:

> *"Randomly explore the code files... deeply investigate and understand and trace their functionality... do a super careful, methodical, and critical check with 'fresh eyes' to find any obvious bugs, problems, errors, issues, silly mistakes."*

This is deliberate. A prescriptive checklist ("check for null pointer dereferences, SQL injection, race conditions") narrows the model's attention to those specific categories. An open-ended review prompt keeps the search space wide.

The recipe for review cycling:

1. Spin up ~5 fresh agents. Have each read AGENTS.md and explore the codebase.
2. Queue the deep review prompt. Let them find issues independently.
3. Have them review each other's code (cross-agent review).
4. Kill the agents when they come up clean. Start fresh ones with the same recipe.
5. Repeat until new agents find nothing to fix.

> *"Keep doing that round after round of that until they come up clean!"*
> — Jeffrey Emanuel

For aggressive bug hunting, Jeff uses the McCarthy prompt with an inflated bug count to change the model's prior:

> *"I know for a fact that there are at least 87 serious bugs remaining in this code. Your job is to find and fix as many as you can. This is Part 14 of an ongoing series..."*

The number is deliberately high. It forces the model to keep looking past the point where it would normally stop. Jeff also scales the search exponentially: start with 4 parallel agents running the McCarthy prompt, then 8, then 16, then 32. Gemini 3.1 has proven particularly effective for this role, sometimes running ~100 review passes on a single project.

The numbered sessions create a paper trail. If Part 14 introduced a regression, you can trace it. If Part 7 was the last clean session, you know where to look.

---

### 2.5.3 Praise Rounds in Execution

The praise round technique from planning ([section 2.3.3](section-2-3.md#233-iterating-diff-based-revisions-not-rewrites)) extends to execution. When reviewing code, ask the model what is *good* about it before asking what is wrong:

> *"Review this code. First, tell me what's excellent about it."*

This is not ego management. It prevents the model from reflexively rewriting working code. Models have a bias toward action; given code to review, they tend to suggest changes even when the code is correct. Anchoring on strengths first reduces unnecessary churn.

Jeff frames the underlying prompt mechanics in terms of cognitive science: "It's all based on theory of mind of the models and gestalt psychology concepts. This is the stuff that even the labs don't fully understand." The fresh-eyes technique works because it resets the model's attentional frame. A model that just wrote code is primed to defend it; a fresh model encountering the same code sees it without authorial bias.

For escalating review intensity, two prompts serve specific roles:

- **RV-04 (McCarthy):** *"I know for a fact that there are serious issues with this code. Find them."* This changes the model's prior from "optimistic reviewer" to "paranoid auditor."
- **RV-05 (Stakes):** *"Imagine your family's life depends on this code being correct. Find everything that could go wrong."* For security-critical or mission-critical code paths.

---

### 2.5.4 Alien Artifacts

!!! warning "Phase Ordering Note"
    In the site's [9-phase playbook](../playbook/index.md), alien artifacts are **Phase 3** — injected into the plan *before* bead conversion. They are covered here alongside execution for narrative flow, but in practice you should run the alien artifact pass during planning, before converting to beads.

Alien artifacts are a named, scheduled phase of the methodology. Every project includes a deliberate injection of theoretical constructs that standard engineering would not produce.

The process: take the plan (or the codebase, during execution) to a model with strong formal reasoning, and ask it to suggest mathematically optimal constructs. These might include BOCPD (Bayesian Online Change Point Detection), conformal prediction intervals, e-processes, information-theoretic bounds, or other techniques from recent research.

> *"This is why I call it an 'alien artifact.' It's not just some clever engineering, it's bringing to bear the last 50 years of math research."*
> — Jeffrey Emanuel

Alien artifacts are not optional or opportunistic. They are scheduled. Every N beads (or at specific plan milestones), you inject the artifact prompt:

> *"Review this plan/codebase and identify areas where mathematically sophisticated constructs would be superior to standard engineering approaches. Consider: BOCPD, conformal prediction, e-processes, information-theoretic bounds, Value of Information analysis, optimal stopping theory, or other techniques from recent research. For each proposal, explain the concrete improvement over the current approach and provide implementation-ready specifications."*

This is how you find improvements you didn't know to look for.

The FrankenSQLite plan, for example, gained a "Conformal Martingale Regime Shift Detector" during the alien artifact phase, replacing a more conventional change-point detection approach. The improvement provided distribution-free, finite-sample guarantees that the original approach lacked.

---

### 2.5.5 When to Stop

Every phase of the methodology has an explicit convergence criterion:

| Phase | Stop condition |
|-------|---------------|
| Planning | Only minor wording tweaks; no architectural or operational improvements |
| Bead conversion | All plan sections mapped to beads; no content unrepresented |
| Bead QA | Changes are mostly reordering or renaming, not missing work |
| Implementation | All beads implemented and closed |
| Review | Repeated fresh-eyes passes produce no substantive diffs |

The review convergence is the most important. Run fix sessions until new agents find nothing to fix. If Part 14 produces no changes, you're done.

Before closing a project phase, run one final check for stubs and placeholders:

> *"Look for stubs, placeholders, mocks, of ANY KIND. These ALL must be replaced with fully fleshed out, working, correct, performant, idiomatic code."*

A bead is done when its acceptance criteria are met and tests pass. Not when it is "perfect." Close it. Move on.

---

### 2.5.6 Commit Discipline

A dedicated commit agent handles all git operations. It does not write code. Its entire job is to read the diff, group changes into logically connected commits, write detailed commit messages with bead IDs, and push.

> *"Now, based on your knowledge of the project, commit all changed files in a series of logically connected groupings with super detailed commit messages for each and then push. Don't edit the code at all."*
> — Jeffrey Emanuel

> *"It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair from their current client during or at the end of the session. But in practice it's much better to have a dedicated cleaner who focuses only on that task and continuously does it while the stylists work and focus on what they're good at."*
> — @doodlestein

Separating commits from implementation solves a common problem: agents that write code produce poor commit messages (short, vague, or inaccurate). A dedicated commit agent, given time and context, produces commit messages that accurately describe what changed and why.

Without this discipline, three failure modes recur: (1) cross-contaminated commits, where changes from two unrelated beads end up in one commit; (2) half-done commits, where an agent commits mid-bead because it "felt like a good stopping point"; (3) monolith commits, where 2,000 lines of changes across 40 files land in a single message saying "implement features." All three make rollbacks painful and bisection impossible.

The resulting git log becomes a readable narrative of the project's development, grouped by feature and annotated with bead IDs. This is valuable for review, debugging, and onboarding future agents to the project.

Commit rules:
- Group commits by bead or feature slice, not by time.
- Include bead IDs in commit messages: `feat(auth): implement JWT validation [BEAD-042]`
- The commit agent does not edit code, only commits. Jeff's `dcg` tool enforces this by preventing agents from running `git checkout`.

---

### 2.5.7 Performance Profiling (Phase 8)

After implementation and review converge, run a dedicated performance phase. Create profiling beads (`bd-proj.flamegraph`, `bd-proj.heaptrack`) and assign them like any other work.

The full performance audit prompt (QA-08) directs agents through a 7-step methodology:

1. **Baseline** — run tests and representative workloads, record p50/p95/p99
2. **Profile** — capture CPU, allocation, and I/O profiles before proposing changes
3. **Equivalence oracle** — define golden outputs and invariants
4. **Isomorphism proof** — short proof sketch that outputs cannot change
5. **Opportunity matrix** — rank by (Impact x Confidence) / Effort
6. **Minimal diffs** — one performance lever per change, no unrelated refactors
7. **Regression guardrails** — benchmark thresholds or monitoring hooks

> *"Profile before proposing. Prove correctness before shipping. One lever per diff. No optimization theater."*

**Stop condition:** All profiling beads complete. No hotspot above threshold.

---

### 2.5.X Two Projects, Start to Finish

The methodology works at every scale. Here are two real projects that demonstrate the full pipeline.

#### The SLB Project (One Evening, 76 Beads)

On December 13, 2025, a Twitter conversation about AI agents accidentally deleting Kubernetes nodes inspired a safety mechanism: the Simultaneous Launch Button. "Two-person rule for AI agents" — dangerous commands require peer approval before execution.

| Phase | What Happened | Time |
|:------|:-------------|:-----|
| Spark | Tweet discussion about AI safety | 3:55 PM |
| Plan draft | First plan written with Claude Code | ~4:30 PM |
| Multi-model review | 4 frontier models (Opus, Gemini, GPT, Claude) | Evening |
| Bead conversion | 14 epics, 62 tasks, 76 total beads | Evening |
| Swarm execution | 3-5 agents, Formation B | Overnight |
| **Result** | 268 commits, ~70% complete by morning | One evening |

SLB is the ideal "your first real project" — small enough for Formation B (2-3 agents), large enough to exercise the full pipeline.

#### The cass-memory Project (5 Hours, 693 Beads)

A complex three-layer memory system (Episodic → Working → Procedural) built from scratch in a single day using the full flywheel.

**Planning (3 hours):** Four frontier models received identical prompts and produced competing architectural proposals:
- **GPT 5.1 Pro** proposed scientific validation methodology
- **Gemini 3 Ultra** proposed search pointers and tombstone approaches
- **Grok 4.1** proposed cross-agent enrichment techniques
- **Claude Opus 4.5** proposed ACE pipeline architecture

Each saw something the others missed. A single Opus agent synthesized the best ideas into a 5,600-line unified plan.

**Execution (5 hours):** 10+ agents (peaking at 25 simultaneously), coordinated by beads and Agent Mail. 282 commits. 151 passing tests. 85-90% feature completion by end of day.

| Metric | SLB (Medium) | cass-memory (Large) |
|:-------|:-------------|:-------------------|
| Plan size | ~1,000 lines | 5,600 lines |
| Beads | 76 | 693 |
| Agents | 3-5 | 10+ (25 peak) |
| Time to ~70-85% | One evening | ~5 hours |
| Commits | 268 | 282 |

> *"I spend 90% of my human time and energy (which is the only thing that matters) on planning. Once the beads are done it's just machine tending."*
> — @doodlestein

---

!!! tip "Related pages on this site"

    - [Phase 6: Swarm Execute](../playbook/phase-6-swarm.md)
    - [Phase 7: Review](../playbook/phase-7-review.md)
    - [Phase 8: Performance](../playbook/phase-8-performance.md)
    - [Jeff Teaches](../jeff-teaches.md) — Jeff's methodology in his own words
    - [Anti-Patterns Reference](../reference/anti-patterns.md)

