---
icon: lucide/brain
---

# Phase 9 -- Metacognition

Study the spec evolution to train intuition for the next project. The part nobody talks about.

---

Every major project gets an `ANALYSIS_OF_SPEC_DOC_DIFFS.md`. Same format every time.

## The 10-Bucket Taxonomy

Classify every commit to the spec:

| Bucket | Category | What it tells you |
|:-------|:---------|:------------------|
| 1 | Logic / Math fixes | Where reasoning was wrong |
| 2 | Legacy system handling | Where existing constraints were underestimated |
| 3 | Sync / replication | Where distributed systems bit you |
| 4 | Conceptual / Architecture | Where the abstraction was wrong |
| 5 | Scrivening (formatting) | Noise -- don't count this |
| 6 | Added context / explanation | Where the plan was ambiguous |
| 7 | Standard engineering | Normal refinement |
| 8 | Alien artifact additions | Where AI pushed beyond your vision |
| 9 | Clarification / disambiguation | Where the spec could be misread |
| 10 | Other | Uncategorized |

## The Point

The evolution of the spec is a map of where your thinking was wrong. Bucket 4 (conceptual/architectural fixes) tells you where you had the wrong abstraction. Bucket 8 (alien artifacts) tells you where the AI pushed beyond your original vision. Bucket 5 (scrivening) is noise; don't count it.

Audience: future-you starting the next project. It answers:

- What did you get wrong the fastest?
- What survived every review unchanged?
- What kept getting harder to specify?

Over time, these documents train your intuition. Which parts of a database spec need 11 revisions before they're right (transaction slots). Which parts get it right in round 1 (documentation, always).

!!! note
    In frankensqlite, the 10-bucket taxonomy was used on 232 commits to the planning docs across 19 days. In frankentui, Jeff applied the same framework to his own project -- metacognitive feedback loop made explicit.

!!! important
    **Stop condition:** Analysis document complete. Patterns identified for next project.
