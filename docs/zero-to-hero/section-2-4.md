---
icon: lucide/git-branch
---

## 2.4 Converting to Beads (Phases 4-5)

### 2.4.1 What a Bead Is

A bead is an atomic unit of work: one task that one agent can complete in one session without needing external input. Each bead has an ID, a description, dependencies on other beads, acceptance criteria, and a status.

> *"Beads are extremely complementary to Agent Mail, and the combination lets you decompose a very big and complex plan into nice units."*
> — Jeffrey Emanuel

The bead system replaces ad-hoc task management. Agents do not choose what to work on based on conversation context or human instruction; they query the bead database for the highest-priority unblocked task, execute it, close it, and move on.

Beads are not limited to code. The bead set for a project includes implementation beads, test beads, observability beads, security review beads, and profiling beads.

---

### 2.4.2 Granularity: How Big Should a Bead Be?

The right size for a bead is 30-90 minutes of agent work, producing one meaningful commit. Rules of thumb:

**Too big:** "Implement the authentication system." This is 10-20 beads, not one. An agent will either lose coherence partway through or produce a monolithic commit that is hard to review.

**Too small:** "Add the import statement for jwt." This is a line of code. If the bead can be completed in under 5 minutes with trivial output, merge it into a larger bead.

**Right size:** "Implement JWT token validation middleware with expiry checking and refresh logic."

Complex plans decompose into 200 to 500 initial beads. The bead conversion prompt (BD-01) requests "epics + tasks + subtasks with appropriate level of granularity." A QA pass specifically checks granularity: are beads too large or too small?

> *"Starts out with just one agent [for bead creation]. Then I add more when it turns into 'make sure we didn't leave something important out' and polishing the beads."*
> — Jeffrey Emanuel

---

### 2.4.3 Dependency Ordering

Beads form a directed acyclic graph (DAG). Core infrastructure beads come first. Feature beads depend on infrastructure. Integration beads depend on features. The dependency structure is the primary coordination mechanism for multi-agent execution.

> *"If you have a large number of beads, you don't want agents just randomly choosing them. Because there's usually a 'right answer' for what each agent should do. And that right answer comes from the dependency structure of the tasks, and this can be mechanically computed using basic graph theory."*
> — Jeffrey Emanuel

The beads viewer (`bv`) computes this automatically: it identifies the critical path, ranks beads by priority (based on how many downstream beads they unblock), and recommends which bead each agent should take next.

A QA pass on the bead graph checks: are orderings correct? Are there missing dependency links? Are there circular dependencies?

---

### 2.4.4 Research Beads vs. Implementation Beads

Not all beads produce code. Research beads produce knowledge: API compatibility analysis, performance benchmarking of alternative approaches, or architecture spike investigations. Their output feeds back into plan revisions or informs implementation bead designs.

The transition from research to implementation is explicit: research beads produce findings; those findings may modify the plan; the modified plan generates new implementation beads. The two types do not blur.

New beads can be created at any point during a project's lifecycle. Discovering that you need a new bead during execution is normal operations, not a planning failure.

> *"Yes, I create new beads for in-progress projects all the time."*
> — Jeffrey Emanuel

---

### 2.4.5 Using Beads Viewer (bv)

The beads viewer (`bv`) is the coordination compass for multi-agent execution. It reads the bead database, computes the dependency graph, and answers the question: "What should each agent work on next?"

> *"[bv] is like a compass that each agent can use to tell them which direction will unlock the most work overall."*
> — Jeffrey Emanuel

In practice, agents invoke `bv --robot-triage` to get a prioritized list of ready beads. The human can also use `bv` interactively to visualize the full DAG, see the critical path, and identify bottlenecks.

The meta-prompt (where an agent uses bv to coordinate other agents):

> *"Re-read AGENTS dot md first. Then, can you try using bv to get some insights on what each agent should most usefully work on? Then share those insights with the other agents via agent mail."*
> — Jeffrey Emanuel

> *"Beads are insane. Slept on those until last week. Beads-viewer makes me happy."*
> — ACFS community member

---

### 2.4.6 Shared State vs. Worktrees

Jeff's approach to parallel agent execution is distinctive and sometimes controversial: all agents work in the same shared directory. No git worktrees.

> *"Working in one shared space surfaces conflicts immediately so you can deal with them, which is easy when agents can communicate."*
> — Jeffrey Emanuel

The coordination mechanisms that make this viable:
- **Advisory file reservations** via Agent Mail (agents "call dibs" on files temporarily)
- **Bead-level task decomposition** (agents work on different beads, minimizing file overlap)
- **Frequent commits** (changes are committed often, keeping the shared state clean)

The alternative (worktrees) defers conflict resolution to merge time, when the context about why a change was made has already been lost. The tradeoff: shared-state requires tighter coordination discipline but catches conflicts earlier.

> *"No worktrees with multiple agents working off same tasks list? They won't step on each other?"*
> — ACFS community member
>
> *"Doesn't seem to be a problem in practice. The beads keep them on track."*
> — Jeffrey Emanuel

Jeff tried worktrees and abandoned them:

> *"Friends don't let friends use git worktrees for agent swarms."*
> — Jeffrey Emanuel

Some practitioners successfully use jujutsu (jj) instead of git for agent isolation. The methodology does not require shared-state; it does require explicit coordination mechanisms regardless of the branching strategy.

---

### 2.4.7 The Bead Stabilization Loop

Bead QA (BD-02) is a convergence loop, not a single pass:

1. **Convert:** A single agent converts the plan to beads via `br create`, producing the initial bead graph with epics, tasks, subtasks, and dependencies.
2. **Graph correctness pass:** A different agent reviews the dependency structure. Are dependencies correct? Are there circular dependencies? Missing parent-child links?
3. **Granularity pass:** Are beads properly scoped (30-90 min each)? Split oversized beads, merge trivial ones.
4. **Refinement loop:** Run BD-02 (QA Beads) repeatedly, alternating between Claude and Gemini per round. Each model catches different structural issues.
5. **Convergence check:** When 2 consecutive rounds produce no meaningful changes (only reordering or renaming), the bead graph is stable.

The Playbook specifies 5-15 QA rounds as typical. Some projects converge after 5; complex projects (300+ beads) may need 15.

```bash
# Alternate review agents per round
ntm --robot-spawn=review-cc --spawn-cc=1     # Claude reviews odd rounds
ntm --robot-spawn=review-gmi --spawn-gmi=1   # Gemini reviews even rounds

# Verify stability
bv --robot-triage | jq '.quick_ref'
br stats
```

---

!!! tip "Related pages on this site"

    - [Phase 4: Beads](../playbook/phase-4-beads.md)
    - [Phase 5: QA](../playbook/phase-5-qa.md)

