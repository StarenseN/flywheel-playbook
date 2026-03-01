---
title: "Phase 7: Fresh-Eyes Review"
layout: default
parent: Playbook
nav_order: 9
---

# Phase 7 -- Fresh-Eyes Review
{: .fs-8 }

Repeatedly review implemented code until review passes produce no substantive diffs. Numbered sessions, not one-shot.
{: .fs-5 .fw-300 }

---

This is where most people drop the ball. They do one review pass and call it done. I run numbered fix sessions. Part 1, Part 2, all the way to Part 23 or further.

## Self-Review After Each Bead (P02b)

```
Great. Now carefully read over all of the new code you just wrote and any
existing code you modified.

With fresh eyes, look for:

- obvious bugs
- incorrect edge cases
- mismatched types / error handling gaps
- unclear naming / confusing flows
- missing tests
- silent failure modes
- performance footguns

Carefully fix anything you uncover. Be meticulous.
```

This runs after every bead, not just at the end. It's baked into the bead execution protocol.

## Deep Review -- Numbered Sessions (P02)

```
I want you to sort of randomly explore the code files in this project, choosing
code files to deeply investigate and understand and trace their functionality and
execution flows through the related code files which they import or which they are
imported by. Once you understand the purpose of the code in the larger context of
the workflows, I want you to do a super careful, methodical, and critical check
with "fresh eyes" to find any obvious bugs, problems, errors, issues, silly
mistakes, etc. and then systematically and meticulously and intelligently correct
them. Be sure to comply with ALL rules in AGENTS.md and ensure that any code you
write or revise conforms to the best practice guides referenced in AGENTS.md.
Use ultrathink.
```

Notice what this does. It does not tell the model what to look for. It does not enumerate bug categories. It asks the model to explore, understand, and then apply judgment. Every prescriptive bug checklist is a tunnel that narrows the model's attention. P02 keeps it wide.

## Cross-Agent Review (P03)

Have agents review each other's work -- cast a wide net:

```
Now review code written by other agents across the project (not just the
latest commit). Look for:

- bugs, edge cases, and inconsistencies
- security/privacy issues
- reliability issues (retries, timeouts, idempotency)
- performance regressions
- unclear or brittle architecture
- missing or low-quality tests

Diagnose root causes using first-principles reasoning and then fix issues you find.
Don't restrict yourself to the latest commits -- cast a wider net and go super deep!
```

Run this with a different model than the one that wrote the code for true "fresh eyes."
{: .tip }

## Escalation Prompts

When reviews plateau and the model seems too comfortable:

**P27 -- McCarthy Bug Hunt:**
```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

**P28 -- Stakes Escalation:**
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

These change the model's internal prior. By default, models are optimistic reviewers. P27 and P28 make them paranoid. Sometimes you need paranoid.

## Stub Elimination

Before final review, hunt all placeholders:

```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

Agents leave stubs during initial passes. This catches them all.

## Why Fresh-Eyes Loops Work

Each review pass catches things the previous one missed because the agent's mental model has shifted. The first pass catches obvious bugs. The second catches architectural inconsistencies. The third catches edge cases. By the fourth or fifth pass, you're getting clean results. Run these as Part 1, Part 2, Part 3... the counter provides accountability and progress tracking.

**Stop condition:** Repeated fresh-eyes passes produce no substantive diffs. The code has converged.
{: .important }
