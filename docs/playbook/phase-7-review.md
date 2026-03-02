---
icon: lucide/eye
---

# Phase 7 -- Fresh-Eyes Review

Repeatedly review implemented code until review passes produce no substantive diffs. Numbered sessions, not one-shot.

---

> "Fresh eyes is a massive unlock. This is the stuff that even the labs don't fully understand. It's all based on theory of mind of the models and gestalt psychology concepts."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022356686774899051)) · 90 likes

> "Keep doing that round after round until they come up clean!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025057469945286763)) · 205 likes

Most people drop the ball here. One review pass, done. I run numbered fix sessions. Part 1, Part 2, all the way to Part 23 or further.

## Self-Review After Each Bead (RV-01)

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

Runs after every bead, not just at the end. Baked into the bead execution protocol.

## Deep Review -- Numbered Sessions (RV-02)

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

Notice what this does. It does not tell the model what to look for. It does not enumerate bug categories. It asks the model to explore, understand, and then apply judgment. Every prescriptive bug checklist is a tunnel that narrows the model's attention. RV-02 keeps it wide.

## Cross-Agent Review (RV-03)

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

!!! tip
    Run this with a different model than the one that wrote the code for true "fresh eyes."

## Escalation Prompts

When reviews plateau and the model seems too comfortable:

**RV-04 McCarthy Hunt:**
```
I know for a fact that there are serious issues with this code. I need you to
find them. Think like Joe McCarthy: assume there's a spy, and your job is to
find them. The bugs are there. Find them.
```

**RV-05 Stakes Escalation:**
```
Imagine your family's life depends on this code being correct. Not metaphorically.
Literally. Find everything that could go wrong.
```

These change the model's internal prior. By default, models are optimistic reviewers. RV-04 and RV-05 make them paranoid. Sometimes you need paranoid.

## Stub Elimination

Before final review, hunt all placeholders:

```
I need you to look for stubs, placeholders, mocks, of ANY KIND. These ALL must
be replaced with FULLY FLESHED OUT, working, correct, performant, idiomatic code
as per the beads. Do this meticulously and carefully!
```

Agents leave stubs during initial passes. This catches them all.

## The Mechanics

Each review pass catches things the previous one missed because the agent's mental model has shifted. First pass: obvious bugs. Second: architectural inconsistencies. Third: edge cases. By the fourth or fifth, results are clean. The numbered counter (Part 1, Part 2, Part 3...) provides accountability and progress tracking.

!!! important
    **Stop condition:** Repeated fresh-eyes passes produce no substantive diffs. The code has converged.
