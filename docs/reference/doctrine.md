---
icon: lucide/shield
---

# Doctrine

15 rules that are not negotiable.

---

> "It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair. But in practice it's much better to have a dedicated cleaner who focuses only on that task. You want your agents focusing on the coding itself. If you try to make them deal with commits, they will do a poor job."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022377209848328610)) · 74 likes

> "Then they just keep cranking on their own for a really long time. And you don't need to supervise them much so you can be juggling multiple projects like this at once."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · 1,442 likes

1. **One canonical plan per project.** Not a plan plus a design doc. One document, one source of truth.

2. **AGENTS.md is written by hand** (or heavily directed). Not generated and forgotten.

3. **Agents are fungible generalists.** No roles, no specialization, no backstories.

4. **The commit agent is separate from coding agents.** EX-06 Git Commit never touches code.

5. **RV-02 Deep Review is a numbered series,** not a single event. Part 1, Part 2, ... Part 23+.

6. **Bead IDs appear in every commit message.** This is the ground-truth coordination surface.

7. **After beads exist, PLAN.md is closed.** The bead-map is the execution surface.

8. **Hooks on bead close cause damage** on incomplete code. Don't add them. Use RV-01 Self-Review instead.

9. **Tests must not use mocks.** Real data, real API calls, real end-to-end.

10. **Recovery runbooks are a planning deliverable,** not a post-failure afterthought.

11. **Pre-integration dependency analysis is required.** Write COMPREHENSIVE_ANALYSIS before integrating anything.

12. **Agent failure is normal operations.** Log it, handle it gracefully, move on.

13. **Profiling is a scheduled bead,** not reactive debugging.

14. **The spec evolution analysis is mandatory** for any project over 100 commits.

15. **Marathon sessions beat distributed sessions.** Context is perishable.
