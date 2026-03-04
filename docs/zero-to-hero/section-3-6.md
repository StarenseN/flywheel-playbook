---
title: "3.6 Anti-Patterns"
icon: lucide/shield-alert
---

## 3.6 Anti-Patterns and War Stories

### 3.6.1 The Mega-Prompt Trap

**Anti-pattern:** Writing enormous, multi-page prompts that attempt to specify every constraint, exception, and edge case in a single instruction.

**Why it fails:** Frontier models attend to every word (see [section 1.5.2](section-1-5.md#152-every-word-in-a-prompt-is-a-constraint)). A two-page prompt does not provide twice the guidance; it provides twice the constraints, many of which conflict or force the model into narrow solution paths that preclude better alternatives.

> *"Yeah, resist the temptation early on. Bloated prompts just confuse the model more than they guide it."*
> -- @glennsc (ACFS community)

**What to do instead:** Jeff's operational prompts (PL-01 through MT-08) are deliberately short. EX-01 (the main execution loop) is two sentences. EX-04 (post-compaction refresh) is one sentence. The detail lives in AGENTS.md and the bead descriptions, not in the runtime prompt. The prompt sets intent and posture; the project infrastructure provides context.

> *"All done with 5 word prompts."*
> -- Jeffrey Emanuel (demonstrating with a screenshot)

**The rule:** If your prompt exceeds 100 words for a routine operation, you are over-specifying. Move the detail into AGENTS.md or the bead description.

### 3.6.2 Skipping the Plan

**Anti-pattern:** Jumping directly to code because "we know what we want" or "the plan is in my head."

**Why it fails:** This is the most expensive anti-pattern. By the time you discover that your architecture is wrong, you have hundreds of committed files that need to be refactored. The refactoring itself introduces bugs. The bugs need debugging. The debugging takes longer than the original planning would have.

From the FLYWHEEL_PLAYBOOK_DEFINITIVE.md:

> *"Skeleton-first development: 'Just get something working, then improve it.' No. By the time you have something working, the architecture is already compromised and you'll spend 10x longer fixing it than you would have spent planning it right."*
> -- Playbook anti-pattern #1

> *"Planning tokens are cheap. Code tokens are expensive. Debugging tokens are ruinous."*
> -- The Playbook's foundational premise

**The economics:** A 6-hour planning session prevents 60 hours of agent chaos. A bad plan wastes multi-agent hours across parallel workers. The 85/10/5 split (planning/implementation/review) is not aspirational; it is the empirically observed ratio from projects that complete successfully.

**What to do instead:** Follow the phases in order. Start with a first-principles prompt (PL-01). Run praise rounds (PL-03 through PL-05). Get multi-model feedback (PL-07). Score the plan (minimum 10/16 on the quality rubric). Run the premortem. Only then convert to beads.

### 3.6.3 Over-Beading

**Anti-pattern:** Decomposing the plan into hundreds of micro-beads where each bead represents 5-15 minutes of work.

**Why it fails:** The coordination overhead per bead (claim it, execute it, self-review, report via Agent Mail, close it, pick next) is roughly constant regardless of bead size. When beads are trivially small, this overhead dominates. An agent spends more time in protocol than in implementation.

The bead QA process (BD-02) has a specific check for this: "is it optimally scoped?" If QA keeps finding beads that are too small, merge adjacent beads.

**The right granularity:** 30-90 minutes of agent execution time per bead. This gives enough substance for meaningful self-review (RV-01) while keeping beads small enough to complete in a single context window.

### 3.6.4 Under-Reviewing

**Anti-pattern:** Running one review pass (RV-02 Part 1) and calling it done.

**Why it fails:** From the Playbook:

> *"One review pass: Part 1 is not enough. Part 2 catches what Part 1 missed. Part 5 catches what Part 4 missed. Part 23 still finds things. Keep going until the diffs flatline."*
> -- Playbook anti-pattern #9

Each review pass operates from a fresh vantage point. RV-02 Part 1 catches the obvious issues. Part 2 catches the issues that Part 1's fixes introduced. Part 5 catches the subtle architectural inconsistencies that only become visible after the obvious bugs are fixed. The stop condition is convergence: when review passes produce no substantive diffs.

Jeff runs escalation prompts when reviews plateau:
- **RV-04 (McCarthy Bug Hunt):** "I know for a fact that there are serious issues with this code. Think like Joe McCarthy: assume there's a spy. The bugs are there. Find them."
- **RV-05 (Stakes Escalation):** "Imagine your family's life depends on this code being correct. Not metaphorically. Literally."

These prompts change the model's internal prior from optimistic to paranoid. Sometimes you need paranoid.

### 3.6.5 Single-Model Monoculture

**Anti-pattern:** Using only one model (e.g., only Claude, only Codex) for all work.

**Why it fails:** Every model has systematic blind spots. Claude is strong on architecture reasoning but can miss implementation details. Codex follows instructions precisely but can be unimaginative. Gemini brings a perspective that makes the other two look incomplete. Using a single model means its blind spots become your blind spots.

> *"I thought codex was a thorough, judicious, genius engineer, and claude was competent but a little behind. After bringing gemini into the fold, codex looks lazy and claude seems totally inept."*
> -- Jeffrey Emanuel

**The multi-model advantage surfaces in three specific places:**

1. **Planning (Phase 2):** PL-07 (Multi-Model Synthesis) has models review each other's competing plans. The synthesis produces a plan stronger than any single model's output.
2. **Review (Phase 7):** RV-03 (Cross-Agent Review) instructs: "Run this with a different model than the one that wrote the code for true 'fresh eyes.'"
3. **Execution (Phase 6):** Formation D+ mixes 3 model families. Different models catch different bugs, produce different edge-case handling, and surface different architectural concerns.

**The rule:** For any project above Formation B (3+ agents), use at least 2 model families. For serious projects (Formation D+), use all 3.

### 3.6.6 Gold-Plating

**Anti-pattern:** Agents spending excessive time polishing code that works correctly, adding unnecessary abstractions, or over-engineering edge cases that will never occur in practice.

**Why it fails:** Agent time is not free. Every minute an agent spends gold-plating a working component is a minute not spent on the next bead. Over-abstraction also introduces complexity that makes future agents' work harder: they must understand the abstraction before modifying the code.

**Symptoms:**
- Agent creates elaborate type hierarchies for a module with 2 implementations.
- Agent adds caching, retry logic, and circuit breakers to a function called once at startup.
- Agent refactors working code for "elegance" during an implementation phase.

**The countermeasure:** The bead system is the countermeasure. Each bead has acceptance criteria. When the criteria are met, the bead is closed. The agent moves on. There is no "make it prettier" bead (unless you explicitly create one). The self-review prompt (RV-01) looks for bugs, not beauty.

**Optimization is a scheduled phase (Phase 8), not a continuous activity.** Profiling beads (`bd-proj.flamegraph`, `bd-proj.heaptrack`) are assigned like any other work. The Playbook rule: "Profile before proposing. Prove correctness before shipping. One lever per diff."

> *"Optimization theater: Proposing performance improvements without profiling first. The Deep Performance Audit methodology exists because most 'optimizations' are guesses that don't move the needle."*
> -- Playbook anti-pattern #10

### 3.6.7 Placeholder Regression

**Anti-pattern:** An agent, while fixing a bug in one function, replaces a nearby working function with a placeholder stub ("TODO: implement this later").

**Why it happens:** The model's context includes the fix intent but not the full implementation of the adjacent function. It "simplifies" the surrounding code to focus on the fix, silently destroying working logic. This is Jeff's most hated failure mode.

**How to catch it:** The stub eliminator prompt (RV-07, in [section 3.2.5](section-3-2.md#325-review-prompts-rv-01-through-rv-09)) specifically targets this. Run it after every fix session, not just at project end. Code review (RV-03) by a different model also catches it, because the reviewing model has no memory of the "simplification" that seemed reasonable to the fixing model.

### 3.6.8 Abstraction Sprawl

**Anti-pattern:** Agents create elaborate test infrastructure, utility libraries, or abstraction layers that look impressive but test nothing real or add no value.

> *"The test suite is largely performative."*
> — Jeff's assessment of an agent-generated test suite

**Why it happens:** Models are trained on codebases with extensive test infrastructure. Given freedom, they reproduce what they've seen: mock factories, test helpers, assertion utilities, base classes. The result passes all checks ("100% test coverage!") while testing nothing of substance.

**The countermeasure:** QA-02 (E2E Pipeline Validator) explicitly demands "without ANY mocks or fake data, fake API calls." The no-mocks rule is not about testing philosophy; it is about preventing agents from building a parallel universe where everything passes and nothing works.

### 3.6.9 The 800 LOC Review Threshold

**Anti-pattern:** Asking an agent to review a diff larger than ~800 lines of code in a single pass.

**Why it fails:** Model review quality degrades sharply past this boundary. The model starts skimming, misses interactions between early and late changes, and produces generic feedback ("looks good, consider adding error handling") instead of specific, actionable findings.

**What to do instead:** Split large reviews at the 800 LOC boundary. For a 2,400-line diff, run three separate review passes, each focused on a specific third. The overhead of three passes is lower than the cost of a single low-quality review that misses critical issues.

### 3.6.10 Ignoring Alien Artifacts

**Anti-pattern:** Treating unexpected model contributions as bugs or noise instead of evaluating them on merit.

**What alien artifacts are:** When a frontier model produces something beyond what was specified in the plan -- a novel algorithm, an unexpected optimization, an architectural pattern you didn't envision -- Jeff calls these "alien artifacts" (see [section 2.5.4](section-2-5.md#254-alien-artifacts)). They are one of the methodology's most valuable outputs.

> *"The 'alien artifact' stuff has specific meaning in my case; I have these 'skills' that explain what these things mean."*
> -- Jeffrey Emanuel

**Why ignoring them fails:** Alien artifacts represent the model's ability to exceed your own engineering knowledge. Phase 3 (Alien Artifacts) exists specifically to harvest these contributions before the plan is frozen into beads. If you skip this phase, you get only what you could have thought of yourself, which defeats half the point of using frontier AI.

**The spec evolution analysis (a post-project retrospective) explicitly tracks alien artifacts in bucket #8:** "Alien artifact additions -- where AI pushed beyond your vision." Over multiple projects, studying which alien artifacts survived to production trains your intuition for recognizing valuable model contributions.

**What to do instead:** After the plan is drafted and refined but before converting to beads, run an alien artifact pass. Ask the model: what would make this dramatically better? What haven't we thought of? What would a domain expert add? Then evaluate each proposal on merit and integrate the good ones.

### 3.6.11 The Incrementalism Trap

**Anti-pattern:** Accepting the model's conservative, "practical" first output without pushing for something more ambitious.

**Why it happens:** Models have absorbed human incrementalism from training data. They default to cautious, under-ambitious proposals because the text they learned from was written by people managing expectations.

> *"It has unfortunately learned incrementalism under the guise of being 'practical' from reading too much written by humans who were trying to sandbag and control expectations so they could phone it in and not get fired. It's possible to shake them out of it, though."*
> — Jeffrey Emanuel

**How to shake them out of it:** The praise rounds (PL-03 through PL-05) exist precisely for this. PL-03 explicitly says the first output "barely scratches the surface and is light years away from being OPTIMAL." The Innovation Boost (PL-10) asks for transformative, not incremental improvements. Without these prompts, the model's default incrementalism becomes your project's ceiling.

### 3.6.12 Git Worktrees for Agent Swarms

**Anti-pattern:** Giving each agent its own git worktree (isolated working directory) so they never interfere with each other.

**Why it fails:** Worktrees defer conflicts to merge time. When Agent A edits `auth.rs` in worktree-A and Agent B edits it in worktree-B, neither sees the other's changes until someone merges. By then, both agents have moved on, lost context, and cannot explain their changes. The merge conflict becomes a puzzle with no witnesses.

> *"All agents in same repo — surfaces conflicts early."*
> -- ACFS methodology (FLYWHEEL.md)

**What to do instead:** All agents share one working directory. Agent Mail file reservations prevent simultaneous edits to the same file. When conflicts do occur, both agents still have context and can resolve them immediately. The commit agent groups changes by bead, not by branch.

**Exception:** If using `jujutsu` (jj) instead of git, its first-class conflict representation makes worktree-like isolation viable because conflicts are data, not errors. This is mentioned in [section 2.4.6](section-2-4.md) but is not the default ACFS workflow.

### 3.6.13 Sending Execution Prompts to Fresh Agents

**Anti-pattern:** Sending EX-01 (Execute Beads) to an agent that has not yet run EX-03 (Agent Introduction).

**Why it fails:** A fresh agent (whether newly spawned or recovered from context compaction) has no project context. It does not know the project's conventions, file structure, naming patterns, or architectural decisions. Without EX-03, the agent hallucinates project structure, invents naming conventions that contradict AGENTS.md, and writes code that does not integrate with the existing codebase.

**Symptoms:** The agent creates files in wrong directories, uses camelCase in a snake_case project, implements features that already exist under different names, or produces code that passes its own tests but fails integration.

**What to do instead:** Always EX-03 first. Wait for the agent to read AGENTS.md, register with Agent Mail, and demonstrate understanding (check its first Agent Mail message). Only then send EX-01. After context compaction, send EX-04 (which re-reads AGENTS.md) before resuming execution.

### 3.6.14 Assuming Prompts Were Sent

**Anti-pattern:** Pasting a prompt into a tmux pane and assuming the agent received it without verification.

**Why it fails:** Terminal paste silently fails roughly 50% of the time. Tmux pane focus, clipboard race conditions, and terminal buffering all conspire against reliable delivery. An agent sitting idle after you "sent" a prompt may have never received it. You wait 20 minutes for output. The agent waits forever for input. Both sides lose.

**Always verify after sending:**
```bash
# Use ntm for reliable delivery:
ntm --robot-send=SESSION --msg='<prompt>'

# MANDATORY: verify arrival (wait 10s, check JSONL):
sleep 10 && tail -3 ~/.claude/projects/-data-projects-myproject/*.jsonl | jq -r '{type, ts: .timestamp}'

# If JSONL shows no new activity, nudge with Enter:
tmux send-keys -t SESSION Enter
```

This is the #1 operational failure mode that catches every new practitioner. See [section 2.7.5](section-2-7.md#275-prompt-delivery-verification) for the full verification protocol.

### 3.6.15 War Story: The Python-to-Rust Agent Mail Rewrite

The most concrete scaling war story from the source material. Jeff's Python Agent Mail worked at moderate scale (15-20 concurrent agents). When he pushed to 80 agents across 3 machines, it collapsed:

> *"The Python version worked when it was 15-20 agents going at once, but when it's 80 of them, it was one problem after another. The rust version has been rock solid reliable and could easily scale to literally thousands of agents (not that I had enough ram for that)."*
> -- Jeffrey Emanuel

But the Rust rewrite was not a simple port. It required first porting two upstream Python libraries (fastmcp and sqlmodel) to Rust, plus building two entirely new Rust libraries (asupersync and FrankenTUI):

> *"I waited to do this because I first had to port the fastmcp and sqlmodel Python libraries to Rust, which are fairly complex. And I also wanted to finish my asupersync library and FrankenTUI so I could use those for everything."*
> -- Jeffrey Emanuel

This is the flywheel in action: FrankenTUI feeds into Agent Mail Rust, which feeds into the next project. Each tool becomes infrastructure for the next rotation. The Rust Agent Mail port stacks 10 self-built libraries.

**Lessons from this war story:**
1. **Scale reveals architectural limits that tests do not.** 15 agents worked. 80 did not. The failure mode was not a bug but a design ceiling.
2. **The rewrite was only possible because of prior tool investments.** Without the planning methodology, the Rust port would have been a multi-month effort. With the flywheel, it was "I'll probably finish the Rust Agent Mail port today."
3. **The flywheel compounds.** Each project consumes previous projects as libraries and becomes infrastructure for the next. This compounding is the "actual flywheel" -- the planning methodology is the engine that makes each rotation clean enough to build on.

---

!!! tip "Related pages on this site"

    - [Anti-Patterns Quick Reference](../reference/anti-patterns.md)

