---
icon: lucide/folder-tree
---

## 2.2 Anatomy of an ACFS Project

### 2.2.1 Creating a Project

```bash
acfs newproj
```

This creates a directory under `/data/projects/` with the canonical structure, detects your tech stack, initializes git, and generates AGENTS.md from a template. The resulting directory looks like this:

```
project/
├── AGENTS.md           # Agent behavioral rules (the most important file)
├── CLAUDE.md           # Project context for Claude Code
├── README.md           # Human-facing documentation
├── docs/plans/         # Plan versions and design documents
├── src/                # Source code (agent-generated)
├── tests/              # Test suites
└── .claude/            # Claude Code session state
```

---

### 2.2.2 AGENTS.md: The Project Constitution

AGENTS.md is the single most important file in any ACFS project. It is not documentation; it is an operating manual for minds that wake up without memory. Every agent reads this file first, before touching anything else.

> *"Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents."*
> — Jeffrey Emanuel

AGENTS.md must contain:

- **Project identity:** what this is, what stack it uses, what the goals are.
- **Behavioral rules:** what agents can and cannot do (file deletions, git operations, scope boundaries).
- **Coordination protocol:** how to register with Agent Mail, how to claim beads, how to communicate with other agents.
- **Bead execution protocol:** the step-by-step process for picking, executing, self-reviewing, and closing a bead.

A typical bead execution protocol section:

```
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Mark it `in_progress`: `br start <id>`
3. Implement it completely
4. Before closing: reread ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
5. Only then: `br close <id>`
6. Communicate status to fellow agents via Agent Mail
7. Pick next bead. Repeat.
```

After every context compaction, agents must re-read AGENTS.md. This is critical enough that Jeff built a dedicated tool to automate the reminder.

> *"You just have to tell it at the beginning and after every compaction to reread AGENTS dot md."*
> — Jeffrey Emanuel

---

### 2.2.3 CLAUDE.md: Project Context

CLAUDE.md provides project-specific context that Claude Code loads automatically when starting a session in that directory. It contains: file structure descriptions, conventions, how to run tests, project-specific quirks.

CLAUDE.md and AGENTS.md serve different purposes. AGENTS.md is behavioral rules (what to do, what not to do). CLAUDE.md is project context (where things are, how they work). Keep them separate.

Project-level CLAUDE.md (`/project/CLAUDE.md`) overrides user-level CLAUDE.md (`~/.claude/CLAUDE.md`).

---

### 2.2.4 The Plan Document

The plan document is a markdown file that describes the entire project: architecture, component interfaces, data flows, edge cases, deployment requirements. For serious projects, this document ranges from 2,000 to 21,000+ lines.

This is not a README. A README tells users what the project does. The plan tells agents how to build it.

The plan is a *read-once* artifact in the execution workflow. Once beads are created from it, agents work from the bead-map, not the plan. The plan's purpose is to produce a good bead-map; after that, it is archived in `docs/plans/` and version-controlled for reference.

> *"This is truly nuts. The plan is 11k lines long and is ridiculously over the top."*
> — Jeffrey Emanuel (on the FrankenSQLite specification)

---

### 2.2.5 Directory Conventions

When multi-model competition runs during planning (see [section 2.3.4](section-2-3.md#234-multi-model-competition)), the competing specs appear as parallel files:

```
docs/plans/
├── PLAN_v1.md                              # Primary plan (Claude)
├── PLAN_v1_CODEX.md                        # Codex revision
├── PLAN_v1_GEMINI.md                       # Gemini revision
├── ANALYSIS_OF_SPEC_DOC_DIFFS.md           # Post-mortem of spec evolution
└── master-todo-bead-map.md                 # Plan-to-bead mapping
```

When `PLAN_CODEX.md` appears alongside the primary plan in a repository, it signals that multi-model competition was run. This pattern is visible in the FrankenSQLite git history, where `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1.md` (Claude) sits alongside `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1_CODEX.md` (Codex).

---

