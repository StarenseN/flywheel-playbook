# Accretive /online/ Insights — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Enrich 4 existing playbook pages with operational insights from /online/, add 3 anti-patterns, 1 principle, 2 Jeff Teaches sections, and align all counts.

**Architecture:** Pure markdown edits to existing files. No new pages. No nav changes. Surgical insertions at specific line numbers.

**Tech Stack:** Markdown (zensical/MkDocs Material), git

---

### Task 1: Enrich Phase 6 — Swarm Execute

**Files:**
- Modify: `docs/playbook/phase-6-swarm.md` (insert after line 83 "Don't specialize.", before line 85 stop condition)

**Step 1: Insert Perception Loop section after "Don't specialize." line**

Insert after `Scale based on bead graph size and budget. All implementation agents are fungible generalists. Don't specialize.`:

```markdown

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
```

**Step 2: Remove the old "Typical Formation" table**

The old 4-row table (lines 74-83) is replaced by the Formation Scale section above. Remove:
```
## Typical Formation

| Agent type | Count | Role |
|:-----------|:------|:-----|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only |

Scale based on bead graph size and budget. All implementation agents are fungible generalists. Don't specialize.
```

**Step 3: Also remove the `· 229 likes` from the second quote (line 17)**

De-slop: strip like count from the opening quote attribution.

**Step 4: Commit**

```bash
git add docs/playbook/phase-6-swarm.md
git commit -m "feat: enrich Phase 6 with conductor's manual (perception loop, patience, formations, model matrix, shutdown, recovery)"
```

---

### Task 2: Add Principle #12 — Session Hygiene

**Files:**
- Modify: `docs/playbook/principles.md`

**Step 1: Change title from "The 11 Principles" to "The 12 Principles"**

**Step 2: Add principle #12 after principle #11**

Insert after the last line of principle #11:

```markdown

## 12. Session hygiene is sacred

Your orchestration session is for orchestration only. Reviews happen in separate side sessions. If you mix planning commands with review output, the orchestrator's context gets polluted and it loses track of where you are in the workflow. One session, one purpose. Spawn dedicated sessions for review, debugging, and exploration.

Source: [ACFS Wizard](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)
```

**Step 3: Commit**

```bash
git add docs/playbook/principles.md
git commit -m "feat: add Principle #12 — session hygiene"
```

---

### Task 3: Add 3 Anti-Patterns (#12, #13, #14)

**Files:**
- Modify: `docs/reference/anti-patterns.md`

**Step 1: Change header count from "11 ways" to "14 ways"**

**Step 2: Append 3 new anti-patterns after #11**

Insert after the last line of anti-pattern #11:

```markdown

### 12. Sending execution prompts to fresh agents

Fire EX-01 (Execute Beads) at an agent that just spawned, and it goes rogue. It doesn't know the bead protocol, hasn't read AGENTS.md, doesn't understand file reservations or Agent Mail. EX-03 (Agent Introduction) is ALWAYS the first prompt on any fresh spawn. No exceptions. The agent needs the constitution before it can follow the rules.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 13. Assuming prompts were sent

You paste a prompt into a terminal pane and assume the agent received it. Roughly half the time, terminal paste silently fails. Always verify delivery: check that a new entry appeared in the session log within 10 seconds. If nothing appeared, nudge with Enter and check again. Never assume. Session logs are ground truth, not the terminal display.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### 14. Interrupting ultrathink

An agent has been "thinking" for 12 minutes and you panic-hit Ctrl+C. Those 12 minutes of deep reasoning are now gone. The model was building toward an insight it hadn't committed yet. If session logs show tokens incrementing, the agent is working. The asymmetry trap: operators tolerate 30 minutes of dead time (unsubmitted prompt) but can't tolerate 10 minutes of active deep reasoning. Invert that instinct.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)
```

**Step 3: Commit**

```bash
git add docs/reference/anti-patterns.md
git commit -m "feat: add anti-patterns #12-14 (fresh spawn, prompt verification, ultrathink)"
```

---

### Task 4: Add 2 Jeff Teaches Sections

**Files:**
- Modify: `docs/jeff-teaches.md`

**Step 1: Insert 2 new sections before "## The Source Map"**

Insert before `## The Source Map`:

```markdown
## On Conducting the Swarm

> "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinner faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526015587266875))

> "OK, my upgrade to 64 cores was well-timed. I now have a disgusting number of next-gen Codex 4.6 and Codex 5.3 clankers running across 3 machines and 9 projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019571245784973452))

> "Phew, just barely got my 1,000 commits in today. Tough when you're focused on just 2 or 3 projects for the day. I really made those clankers think hard today."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2018946284426584559))

At Formation A or B, you're prompting agents directly. You write the prompt, paste it in, watch the output, course-correct. That's fine for a single agent working a single problem.

At Formation C and above, the job changes. The self-referential loop tweet captures the inflection point: I stopped prompting agents and started prompting my tools to route my agents. Tell BV to compute the critical path. Tell it to share the triage with the other agents via Agent Mail. Tell it to pick its own next task and start working. The human's job becomes conducting, not coding and not even prompting individual agents.

The 1,000-commits tweet is what that looks like from the output side. Not 1,000 changes I made. 1,000 changes my swarm made while I was conducting across 3 machines, checking in every 20 minutes, firing pre-written prompts with StreamDeck button presses that take under a second each.

You develop a feel for it. When an agent is stuck (session logs stale, no output) versus when it's thinking deep (session logs active, slow but steady). The perception loop becomes instinct: glance, interpret, act or wait. That's what conducting a swarm is.

→ [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On the Flywheel Effect (Libraries All the Way Down)

> "The new Rust version of Agent Mail uses the following other libraries and ports that I've made in the last month or so: FrankenTUI for the console interface, FrankenSQLite, FrankenSearch for message search, FrankenAgentDetection (abstracted out from cass), Asupersync (zero tokio allowed!) throughout, fastmcp_rust for mcp, sqlmodel_rust for orm, beads_rust for beads integration, toon_rust for token dense payloads. That's what the Flywheel is all about. Making programs and libraries and then turning around and combining them with other new libraries you've made to make still more ambitious new projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026872257172161002))

> "Clankers love working with my libraries because they're designed and built entirely by clankers. Everything is just where they expect it to be and works exactly the way they assume it would. "FCBC" (for clankers, by clankers)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026327054828880055))

The flywheel isn't just the planning methodology. It's what happens at the infrastructure level when every project you build becomes a library for the next one. That Rust Agent Mail tweet lists nine libraries from prior projects, all wired into a single new one. None of them existed six months earlier. Each was built by the same swarm workflow, using the same beads, the same prompts, the same Agent Mail that the new project is now replacing with a faster version. That's compounding.

FCBC is the design principle that makes it work. When agents build the libraries and agents consume them, the interface aligns perfectly. No documentation mismatch. No human assumptions about what the API "should" look like. The consumers shaped the API by using it. This is why my velocity looks impossible from the outside: I'm not building from scratch each time. I'm stacking nine projects deep.

→ [Toolchain](reference/toolchain.md)

---

```

**Step 2: Update the Source Map**

Update the tweet count and add the new tweets to the Source Map table. The current Source Map says "14 prompts from 7 tweets." The new sections don't source prompts, but the source map should note the new quotes. Add after the existing table rows:

No prompt-sourcing tweets to add. The 5 new tweets (1994526015587266875, 2019571245784973452, 2018946284426584559, 2026872257172161002, 2026327054828880055) are used for teaching text, not prompt sourcing. Source Map stays as-is.

**Step 3: Commit**

```bash
git add docs/jeff-teaches.md
git commit -m "feat: add Jeff Teaches sections — conducting the swarm + flywheel effect (5 verified tweets)"
```

---

### Task 5: Align All Counts (11→12 principles, 11→14 anti-patterns)

**Files:**
- Modify: `zensical.toml` (line 3: "11 principles" → "12 principles")
- Modify: `docs/llms.txt` (3 refs: line 3, 7, 27)
- Modify: `docs/llms-full.txt` (5 refs: line 3, 57, 86, 288, 1388)
- Modify: `docs/index.md` (2 refs: line 34, 39)
- Modify: `docs/jeff-teaches.md` (3 refs: lines 31, 59, 291 — "11 Principles" → "12 Principles")
- Modify: `README.md` (1 ref: line 29)

**Step 1: Principles 11→12 everywhere**

All files: replace `11 Principles` / `11 principles` with `12 Principles` / `12 principles`.

**Step 2: Anti-patterns 11→14 everywhere**

- `docs/llms.txt` line 27: "11 ways" → "14 ways"
- `docs/llms-full.txt` line 86: "11 ANTI-PATTERNS" → "14 ANTI-PATTERNS"
- `docs/llms-full.txt` line 288: "11 anti-patterns" → "14 anti-patterns"
- `docs/llms-full.txt` line 1388: "11 anti-patterns" → "14 anti-patterns"
- `docs/index.md` line 39: "11 ways" → "14 ways"

**Step 3: Add P12 and A12-A14 to llms-full.txt lists**

After P11 line, add:
```
  P12  Session hygiene is sacred — one session, one purpose
```

After A11 line, add:
```
  A12  Execution prompts on fresh agents — EX-03 first, always
  A13  Assuming prompts were sent — verify delivery in session logs
  A14  Interrupting ultrathink — if tokens move, the agent is working
```

**Step 4: Add principle #12 to README.md**

After principle #11 "Every word in a prompt is a constraint" section, add:

```markdown

### 12. Session hygiene is sacred

Your orchestration session is for orchestration only. Reviews happen in separate side sessions. Context pollution kills the state machine.
```

**Step 5: Commit**

```bash
git add zensical.toml docs/llms.txt docs/llms-full.txt docs/index.md docs/jeff-teaches.md README.md
git commit -m "chore: align counts — 12 principles, 14 anti-patterns across all files"
```

---

### Task 6: Push and Verify

**Step 1: Push**

```bash
git push
```

**Step 2: Verify deployed site**

Check that the following pages render correctly:
- https://starensen.github.io/flywheel-playbook/playbook/phase-6-swarm/
- https://starensen.github.io/flywheel-playbook/playbook/principles/
- https://starensen.github.io/flywheel-playbook/reference/anti-patterns/
- https://starensen.github.io/flywheel-playbook/jeff-teaches/

**Step 3: Final commit (design doc cleanup)**

```bash
git add docs/plans/
git commit -m "docs: save implementation plan for accretive /online/ insights"
git push
```
