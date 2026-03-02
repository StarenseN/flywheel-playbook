---
icon: lucide/lightbulb
---

# The 12 Principles

These are the ideas behind the workflow. Understand these first; the phases are just the implementation.

---

> "My approach really works. You don't have to be some crazy savant to apply it, either. Just read my posts about planning and use the tools and workflows, and you will also get these kinds of results. I've now seen at least 10 people who I don't even know report similar outcomes."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021641758690681021)) · 283 likes

> "I spend 90% of my human time and energy (which is the only thing that matters) on planning. Once the beads are done it's just machine tending."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021695143187767360))

## 1. Planning is leverage

High-quality planning compresses implementation time, reduces rework, and improves architecture and UX before code exists. Not "writing docs before coding." The plan IS the product. Code is just the plan's compiled form.

## 2. Iterative refinement beats single-shot

No single pass -- no matter how good the model -- catches everything. Repeated critique cycles improve robustness, testability, and clarity. Four to five refinement rounds is the floor. On a serious project, total revision count including multi-model passes, alien artifacts, and post-merge alignment will hit 8-15.

## 3. Diff-based revisions are the key mechanic

Requiring git-diff style edits prevents vague advice and forces concrete, integratable changes. When a model says "consider adding error handling," that is worth nothing. When it produces a diff that adds error handling to a specific function with specific retry logic, that is worth everything. Always demand diffs against the current plan.

## 4. Three spaces, not two

Most people think there are two spaces: plan space and code space. Wrong. There are three:

1. **Plan space** -- where the architecture lives (`PLAN.md`)
2. **Bead-map space** -- where the execution graph lives (`master-todo-bead-map.md`)
3. **Code space** -- where the implementation lives (`src/`)

The plan is read-once. After beads are created, `PLAN.md` is closed. Agents read the bead-map during execution, never the plan. The bead-map bridges design intent to executable work.

## 5. Use the right model for the job

Match task complexity to model capability:

| Task | Best model | Why |
|:-----|:----------|:----|
| High-volume deterministic work | GPT-5.2 / Codex (xhigh-thinking) | Reliable, fast, cheap |
| Creative/strategic reasoning | Claude Opus | Best architectural judgment |
| Long plan critiques | Extended reasoning via web app | Cheap relative to code tokens |
| Formal reasoning / alien artifacts | o3 or equivalent | Mathematical depth |

But: **do not specialize agents.** Diversify models; don't constrain roles. Every agent is a fungible generalist. The bead system handles coordination, not role descriptions. The single exception is the dedicated commit agent (EX-06 Git Commit), which never edits code.

## 6. Beads are an execution graph, not a todo list

You're converting narrative intent into granular tasks with explicit dependencies so agents can coordinate safely. Each bead has: scope, acceptance criteria, context, rationale, dependencies, constraints. An agent working on a bead should not need to constantly refer back to the plan.

## 7. Check your beads N times, implement once

The asymmetry: revising a bead takes seconds; fixing a wrong implementation takes hours. Repeated beads review catches missing tasks, wrong dependencies, unclear acceptance criteria, and missing test coverage. Run 5-15 QA passes until changes are mostly reordering and renaming, not discovering missing work.

## 8. Avoid communication purgatory

Coordination is valuable, but progress requires agents to pick actionable beads and ship. If an agent spends more time coordinating than coding, something is wrong. Be proactive about starting tasks. Inform fellow agents when you start work, don't ask for permission. Mark beads so others can see what's claimed.

## 9. Fresh-eyes review loops work

After agents implement, run repeated "reread what you wrote and fix obvious issues" loops until changes flatline. Each pass catches things the previous one missed. First pass: obvious bugs. Second: architectural inconsistencies. Third: edge cases. By the fourth or fifth, results are clean. Run these as numbered sessions -- Part 1, Part 2, all the way to Part 23 if needed.

## 10. Alien artifacts are a scheduled discipline

Before converting the plan to beads, deliberately inject mathematically advanced constructs -- BOCPD, VOI, conformal prediction, e-processes. A named phase, not an ad-hoc addition. Take the plan to a model strong at formal reasoning and tell it to add "alien artifacts" -- theoretically optimal constructs that a brilliant but alien mathematician might suggest. This pushes plans beyond standard engineering into genuinely novel territory.

## 11. Every word in a prompt is a constraint

Short prompts that force the model beyond its default posture beat long prompts that enumerate everything it should think about. Counterintuitive, but true. Models are not search engines with a personality -- they are collaborators. They respond to framing. They respond to stakes. They respond to praise. Use that.

## 12. Session hygiene is sacred

Your orchestration session is for orchestration only. Reviews happen in separate side sessions. If you mix planning commands with review output, the orchestrator's context gets polluted and it loses track of where you are in the workflow. One session, one purpose. Spawn dedicated sessions for review, debugging, and exploration.

Source: [ACFS Wizard](https://github.com/Dicklesworthstone/agentic_coding_flywheel_setup)
