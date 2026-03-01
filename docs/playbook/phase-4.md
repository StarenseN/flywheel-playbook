---
title: "Phase 4: Convert to Beads"
layout: default
parent: Playbook
nav_order: 6
---

# Phase 4 -- Convert Plan to Beads
{: .fs-8 }

Transform the stable plan into a granular execution graph with epics, tasks, subtasks, and explicit dependencies.
{: .fs-5 .fw-300 }

---

## The Conversion Prompt (P05)

```
Reread AGENTS.md so it's fresh in your mind.

Now read ALL of <PLAN_FILE_PATH>.

Please take ALL of that and elaborate on it more and then create a comprehensive
and granular set of beads for all this with:

- epics + tasks + subtasks (as needed)
- dependency structure overlaid (blocks/related/parent-child/discovered-from)
- detailed comments so the beads are self-contained and self-documenting

Include relevant background, reasoning/justification, constraints, and acceptance
criteria so we never need to refer back to <PLAN_FILE_PATH>.

Also include beads for:

- comprehensive unit tests (meaningful coverage, not shallow mocks)
- e2e/integration scripts with great, detailed logging
- observability/alerts for silent-failure risk areas

Use only the bd tool to create and modify beads and add dependencies. Be exhaustive.
```

## What Makes Good Beads

Each bead must include:

- **Clear scope** -- what exactly needs to be done
- **Acceptance criteria** -- how to know when it's done
- **Context and rationale** -- why this task exists
- **Dependencies** -- what must complete first (blocks/related/parent-child)
- **Constraints** -- guardrails the implementation must respect

Don't forget beads for: tests (unit + e2e), observability, performance profiling, security checks.

## The Bead Map

Then create `master-todo-bead-map.md`:

```markdown
## Section 3: Rendering Engine
- [ ] Core diffing algorithm -> bd-10i.3.1
- [ ] PackedRgba blending -> bd-10i.3.2
- [ ] CellAttrs + style flags -> bd-10i.3.3
```

This exists for one reason: **agents should never need to reopen the plan.** The plan is read-once. The map is the execution surface. During implementation, agents read the map, pick a bead, implement it, close it, pick the next one. PLAN.md is closed.

After beads exist, PLAN.md is closed. The bead-map is the execution surface. If an agent re-reads the plan during P08, something went wrong in bead conversion.
{: .warning }

**Stop condition:** All plan sections have been converted to beads with dependencies. No plan content is left unrepresented.
{: .important }
