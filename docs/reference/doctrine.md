---
icon: lucide/shield
---

# Doctrine

15 rules that are not negotiable.

---

1. **One canonical plan per project.** Not a plan plus a design doc. One document, one source of truth.

2. **AGENTS.md is written by hand** (or heavily directed). Not generated and forgotten.

3. **Agents are fungible generalists.** No roles, no specialization, no backstories.

4. **The commit agent is separate from coding agents.** P11 never touches code.

5. **P02 is a numbered series,** not a single event. Part 1, Part 2, ... Part 23+.

6. **Bead IDs appear in every commit message.** This is the ground-truth coordination surface.

7. **After beads exist, PLAN.md is closed.** The bead-map is the execution surface.

8. **Hooks on bead close cause damage** on incomplete code. Don't add them. Use P02b instead.

9. **Tests must not use mocks.** Real data, real API calls, real end-to-end.

10. **Recovery runbooks are a planning deliverable,** not a post-failure afterthought.

11. **Pre-integration dependency analysis is required.** Write COMPREHENSIVE_ANALYSIS before integrating anything.

12. **Agent failure is normal operations.** Log it, handle it gracefully, move on.

13. **Profiling is a scheduled bead,** not reactive debugging.

14. **The spec evolution analysis is mandatory** for any project over 100 commits.

15. **Marathon sessions beat distributed sessions.** Context is perishable.
