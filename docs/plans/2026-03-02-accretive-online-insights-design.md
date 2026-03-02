# Design: Accretive Insights from /online/ into the Playbook

**Date:** 2026-03-02
**Source material:** `/data/projects/Doodlestein/online/web/` (06_swarm_operations.md, 05_acfs_wizard_methodology.md, 07_jeff_philosophy.md)
**Principle:** No new pages. Deepen existing pages where thin. Same tone.

---

## Sources of Truth

Three accepted source types, in order of preference:

| Source | Attribution format | When to use |
|--------|-------------------|-------------|
| **Tweet** | `— @doodlestein ([source](https://x.com/doodlestein/status/XXX))` | Jeff's public thoughts, methodology statements |
| **Website** | `— ACFS Wizard ([source](https://agent-flywheel.com/))` | Operational docs, learning hub content |
| **GitHub** | `— ACFS ([source](https://github.com/Dicklesworthstone/REPO))` | READMEs, cookbooks, code docs |

Tweets remain the primary source for Jeff Teaches (verbatim quotes). Website and GitHub sources are used for operational content (Phase 6 additions, principles) where the knowledge comes from docs rather than tweets.

---

## Addition 1 — Phase 6: The Conductor's Eye

**File:** `docs/playbook/phase-6-swarm.md`
**Insert after:** "Typical Formation" table (line ~83), before the stop condition

### 1a. The Perception Loop (~15 lines)

**Source:** Swarm Steering Cookbook ([ACFS Wizard](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup))

What to check every 30-60 seconds during active swarms. Condensed from the 5-step loop into a tight table:

| I see | It means | Do |
|-------|----------|----|
| Spinner + output flowing | Working | Wait |
| Spinner + stale 15+ min | Deep thinking (ultrathink) | Check, don't interrupt |
| Permission prompt | Needs you NOW | Approve |
| Shell prompt, no spinner, 5+ min | Finished or crashed | Assign next task |

Plus the cardinal rule: **check at least 2 sources before concluding anything** (output + JSONL, not just one).

### 1b. Patience Calibration (~10 lines)

**Source:** Swarm Steering Cookbook ([ACFS Wizard](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup))

The asymmetry trap: operators wait 30 min for an unsubmitted prompt (dead time) but panic-interrupt 10 min of active ultrathink (productive time).

Rule: **If tokens are incrementing, the agent is WORKING. Do NOT interrupt.**

### 1c. Formation Scales (expand existing table ~8 lines)

**Sources:** tweet `2019571245784973452` + ACFS Wizard docs

Replace the current 4-row table with Jeff's formation terminology from Feb 2026.

---

## Addition 2 — Principle #12: Session Hygiene

**File:** `docs/playbook/principles.md`
**Insert after:** principle #11

New principle:

> ## 12. Session hygiene is sacred
>
> Wizard context is orchestration only. Reviews happen in separate sessions. If you mix planning state with review output, the orchestrator's context gets polluted and the state machine breaks. One session, one purpose.

**Source:** ACFS Wizard methodology docs ([source](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup))

**Count updates:** 11→12 principles across: principles.md title, zensical.toml, llms.txt, llms-full.txt, docs/index.md, jeff-teaches.md (3 refs), README.md, agent/index.md (if referenced)

---

## Addition 3 — Jeff Teaches: 2 New Sections

**File:** `docs/jeff-teaches.md`
**Insert before:** the Source Map / final section

### 3a. "On Conducting the Swarm"

**Quotes (verified in archive):**

1. Self-referential loop — tweet `1994526015587266875` (Nov 28 2025)
   > "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinner faster and faster now..."

2. 64 cores / scale — tweet `2019571245784973452` (Feb 6 2026)
   > "OK, my upgrade to 64 cores was well-timed. I now have a disgusting number of next-gen Codex 4.6 and Codex 5.3 clankers running across 3 machines and 9 projects."

**Teaching text (~8-10 lines):** Jeff's first-person reflection on how his role shifted from "prompting agents" to "prompting tools to route agents." The perception loop as instinct — you develop a feel for when an agent is stuck vs thinking. The conductor metaphor.

### 3b. "On the Flywheel Effect (Libraries All the Way Down)"

**Quotes (verified in archive):**

1. Franken stack / libraries — tweet `2026872257172161002` (Feb 26 2026)
   > "That's what the Flywheel is all about. Making programs and libraries and then turning around and combining them with other new libraries you've made to make still more ambitious new projects."

2. FCBC — tweet `2026327054828880055` (Feb 24 2026)
   > "Clankers love working with my libraries because they're designed and built entirely by clankers. Everything is just where they expect it to be and works exactly the way they assume it would. 'FCBC' (for clankers, by clankers)."

**Teaching text (~8-10 lines):** First-person reflection on compounding. Each project consumes the previous ones as libraries. The Franken stack as proof — 9 libraries from prior projects wired into one new one. FCBC as a design principle: build for your agents, not for yourself.

---

## Addition 4 — Count Alignment

All files referencing "11 principles" → "12 principles":
- `zensical.toml` site_description
- `docs/llms.txt` (3 refs)
- `docs/llms-full.txt` (4 refs: header, section title, diagnose, boot)
- `docs/index.md` (1 ref)
- `docs/jeff-teaches.md` (3 refs)
- `docs/playbook/principles.md` title
- `README.md` (1 ref)
- `docs/agent/index.md` (if present)

New P12 line in llms-full.txt principles list:
```
  P12  Session hygiene is sacred — one session, one purpose
```

---

## What We're NOT Doing

- No new nav pages
- No toolchain expansion (already decent)
- No doctrine changes (stays at 14)
- No anti-patterns changes (stays at 11)
- No Phase 6 ntm commands (too tool-specific for the playbook's level of abstraction)
- No unverified quotes (tweets verified in archive, docs verified in /online/)
