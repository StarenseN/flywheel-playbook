# Design: Accretive Insights from /online/ into the Playbook

**Date:** 2026-03-02
**Source material:** `/data/projects/Doodlestein/online/web/` (06_swarm_operations.md, 05_acfs_wizard_methodology.md, 07_jeff_philosophy.md)
**Principle:** No new pages. Deepen existing pages where thin. Same tone. Hands-on, not theory.

---

## Sources of Truth

Three accepted source types:

| Source | Attribution format | When to use |
|--------|-------------------|-------------|
| **Tweet** | `— @doodlestein ([source](https://x.com/doodlestein/status/XXX))` | Jeff's public thoughts, methodology statements |
| **Website** | `— ACFS Wizard ([source](https://agent-flywheel.com/))` | Operational docs, learning hub content |
| **GitHub** | `— ACFS ([source](https://github.com/Dicklesworthstone/REPO))` | READMEs, cookbooks, code docs |

Tweets remain the primary source for Jeff Teaches (verbatim quotes). Website and GitHub sources are used for operational content (Phase 6, anti-patterns, principles) where the knowledge comes from battle-tested docs rather than tweets.

---

## Addition 1 — Phase 6: The Full Conductor's Manual

**File:** `docs/playbook/phase-6-swarm.md`
**Current state:** 87 lines. Covers WHAT to deploy. Says nothing about HOW to operate.
**Target:** ~170 lines. A real operator's manual.

### 1a. The Perception Loop

**Source:** Swarm Steering Cookbook ([ACFS](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup))

What to check every 30-60 seconds during active swarms. NOT a theory diagram — a real operator checklist:

| I see | It means | Do |
|-------|----------|----|
| Spinner + output flowing | Working | Wait 2-5 min |
| Spinner + stale 15+ min | Ultrathink (deep reasoning) | Verify tokens moving. Do NOT interrupt. |
| "Waiting for user confirmation" | Permission prompt | Approve NOW |
| Shell prompt, no spinner, 5+ min | Finished or crashed | Assign next bead |
| Context usage > 80% | Approaching compaction | Let it finish current bead, then EX-04 |

Cardinal rule: **check at least 2 sources** before concluding anything. Output can be stale. JSONL is ground truth.

### 1b. Patience Calibration

**Source:** Swarm Steering Cookbook

The asymmetry trap, the #1 operator mistake:

> **WRONG:** Wait 30 min for an unsubmitted prompt (dead time), then panic-interrupt 10 min of active ultrathink (productive time).
>
> **RIGHT:** If JSONL shows no updates → check immediately. If JSONL shows incrementing tokens → leave it alone.

Rule: **If tokens are incrementing, the agent is WORKING. Do NOT interrupt. Do NOT Ctrl+C. Wait. Check back in 5 min.**

### 1c. Formation Scales

**Sources:** tweet `2019571245784973452` + ACFS Wizard docs

Replace current 4-row table with Jeff's formation terminology:

| Formation | Agents | When |
|-----------|--------|------|
| A | 1 | Learning, debugging |
| B | 2-3 | Small features |
| C | 4-6 | Jeff's typical project |
| D | 7-10 | Heavy projects |
| E | 11-15 | frankensqlite scale |
| F+ | 15+ | Feb 2026: 8 Codex + 8 Claude per project, 3 machines |

### 1d. Model-Role Matrix

**Source:** ACFS Wizard methodology docs

Which model for which task — this is operational knowledge that comes from running swarms daily:

| Role | Best model(s) | Avoid | Why |
|------|---------------|-------|-----|
| Coding (EX-01) | Codex, Claude | Gemini | Shell integration issues |
| Review (RV-02) | Gemini, Claude | — | Gemini catches things Claude misses |
| Commit (EX-06) | Claude ONLY | Codex, Gemini | Git edge cases fail |
| Alien artifacts (Phase 3) | Gemini, o3 | — | Formal reasoning strength |

### 1e. Graceful Shutdown

**Source:** Swarm Steering Cookbook

Don't kill working agents. Ever.

1. Tell agents to finish: "Finish your current task, commit your work, then stop."
2. Wait for idle (don't poll — block).
3. Verify clean state: `git diff --stat`
4. Then kill the session.

**Consequence of mid-kill:** Truncated files, broken code, merge conflicts that are hard to diagnose because the agent that created them is dead.

### 1f. When Things Break (3 most common)

**Source:** Swarm Steering Cookbook + ACFS Wizard

Short reference for the 3 failures every operator hits:

**Agent hung:** JSONL stale 15+ min → nudge with Enter → if still dead → Ctrl+C → restart → EX-03 (fresh start)

**Uncommitted files piling up:** Commit agent crashed → manually commit once → respawn commit agent → EX-06 immediately

**Merge conflict:** Stop all agents → resolve manually → checkpoint → EX-02 (mail check & continue) on all agents

---

## Addition 2 — Principle #12: Session Hygiene

**File:** `docs/playbook/principles.md`
**Insert after:** principle #11
**Source:** ACFS Wizard methodology docs ([source](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup))

New principle:

> ## 12. Session hygiene is sacred
>
> Your orchestration session is for orchestration only. Reviews happen in separate sessions. If you mix planning state with review output, the orchestrator's context gets polluted and it loses track of where you are. One session, one purpose. Spawn dedicated side sessions for review work.

**Count updates:** 11→12 principles across all files.

---

## Addition 3 — Anti-Patterns: 3 New Entries

**File:** `docs/reference/anti-patterns.md`
**Current:** 11 anti-patterns. **Target:** 14.
**Source:** Swarm Steering Cookbook + ACFS Wizard methodology

### #12. Sending execution prompts to fresh agents

Fire EX-01 (Execute Beads) at an agent that just spawned without EX-03 first, and it goes rogue. It doesn't know the bead protocol, doesn't understand file reservations, hasn't read AGENTS.md. EX-03 (Agent Introduction) is ALWAYS the first prompt on any fresh spawn. No exceptions.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### #13. Assuming prompts were sent

You paste a prompt into a tmux pane and assume the agent received it. Roughly half the time, TUI paste silently fails. Always verify delivery: check that a new entry appeared in the session log within 10 seconds. If nothing appeared, nudge with Enter and check again. Never assume.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

### #14. Interrupting ultrathink

An agent has been "thinking" for 12 minutes and you panic-hit Ctrl+C. Those 12 minutes of deep reasoning are now gone. The model was building toward an insight. If the session log shows tokens incrementing — even slowly — the agent is working. The asymmetry trap: operators tolerate 30 minutes of dead time (unsubmitted prompt) but can't tolerate 10 minutes of active deep reasoning. Invert that instinct.

→ See [Phase 6 — Swarm](../playbook/phase-6-swarm.md)

---

## Addition 4 — Jeff Teaches: 2 New Sections

**File:** `docs/jeff-teaches.md`
**Insert before:** the Source Map section

### 4a. "On Conducting the Swarm"

**Quotes (verified in archive):**

1. Self-referential loop — tweet `1994526015587266875` (Nov 28 2025)
   > "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinner faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools."
   > [includes the full prompt he used, telling Opus to use bv for graph-theory triage]

2. Scale — tweet `2019571245784973452` (Feb 6 2026)
   > "OK, my upgrade to 64 cores was well-timed. I now have a disgusting number of next-gen Codex 4.6 and Codex 5.3 clankers running across 3 machines and 9 projects."

3. 1000 commits — tweet `2018946284426584559` (Feb 4 2026)
   > "Phew, just barely got my 1,000 commits in today. Tough when you're focused on just 2 or 3 projects for the day."

**Teaching text (~12 lines):** First-person reflection on how the role shifts as you scale. At Formation A-B you're prompting agents. At Formation C-D you're prompting tools to route agents. At Formation E-F+ you're tending machines across multiple projects, checking in every 20 minutes, firing prompts with StreamDeck button presses that take under a second. The self-referential loop tweet captures the inflection point: when you realize you're not driving the agents anymore, you're driving the tools that drive the agents.

The 1000-commits tweet is what that looks like from the output side. Not 1000 changes you made. 1000 changes your swarm made while you were conducting.

Cross-references: → [Phase 6 — Swarm](playbook/phase-6-swarm.md) · [The Human's Role](#the-humans-role-machine-tender)

### 4b. "On the Flywheel Effect (Libraries All the Way Down)"

**Quotes (verified in archive):**

1. Flywheel definition — tweet `2026872257172161002` (Feb 26 2026)
   > "That's what the Flywheel is all about. Making programs and libraries and then turning around and combining them with other new libraries you've made to make still more ambitious new projects."
   > [includes the full Franken stack: 9 libraries wired into Rust Agent Mail]

2. FCBC — tweet `2026327054828880055` (Feb 24 2026)
   > "Clankers love working with my libraries because they're designed and built entirely by clankers. Everything is just where they expect it to be and works exactly the way they assume it would. 'FCBC' (for clankers, by clankers)."

**Teaching text (~10 lines):** The flywheel isn't just the planning methodology. It's what happens when every project you build becomes a library for the next one. The Rust Agent Mail tweet shows 9 libraries from prior projects wired into a single new one. That's compounding at the infrastructure level, and it explains why Jeff's velocity looks impossible from the outside: he's not building from scratch each time, he's stacking.

FCBC is the design principle that makes it work. When agents build the libraries and agents consume them, the interface aligns perfectly. No documentation mismatch. No human assumptions about what the API should look like. The consumers shaped the API by using it.

Cross-references: → [Toolchain](reference/toolchain.md)

---

## Addition 5 — Count Alignment

All files referencing "11 principles" → "12 principles":
- `docs/playbook/principles.md` title
- `zensical.toml` site_description
- `docs/llms.txt` (3 refs)
- `docs/llms-full.txt` (4 refs: header, section title, diagnose, boot)
- `docs/index.md` (1 ref)
- `docs/jeff-teaches.md` (3 refs)
- `README.md` (1 ref)

New P12 line in llms-full.txt principles list:
```
  P12  Session hygiene is sacred — one session, one purpose
```

Anti-patterns: 11→14 in:
- `docs/reference/anti-patterns.md` header
- `docs/llms.txt` (if referenced)
- `docs/llms-full.txt` (3 refs: anti-patterns section header, diagnose, boot)
- `docs/index.md` (if referenced)

---

## Summary: What Changes

| Page | Current | After | Delta |
|------|---------|-------|-------|
| Phase 6 Swarm | 87 lines, WHAT to deploy | ~170 lines, HOW to operate | +perception loop, patience, formations, model matrix, shutdown, recovery |
| Principles | 11 | 12 | +session hygiene |
| Anti-Patterns | 11 | 14 | +fresh agent, +prompt verification, +ultrathink interruption |
| Jeff Teaches | 11 sections | 13 sections | +conducting the swarm, +flywheel effect |
| Counts | 11 principles, 11 anti-patterns | 12 principles, 14 anti-patterns | aligned everywhere |

## What We're NOT Doing

- No new nav pages
- No toolchain expansion (already decent)
- No doctrine changes (stays at 14)
- No ntm/bv commands in Phase 6 (tool-specific; we describe the WHAT and WHY, operators find the HOW in the tools)
- No unverified quotes (tweets verified in archive, docs verified in /online/)
- No quality gates page (too tool-specific for the playbook's abstraction level)
- No recovery runbook template (belongs in AGENTS.md, not the playbook)
