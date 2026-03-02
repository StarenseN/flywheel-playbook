---
icon: lucide/shield
---

# Doctrine

14 rules that are not negotiable. Each exists because violating it has destroyed real projects.

---

> "It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair. But in practice it's much better to have a dedicated cleaner who focuses only on that task. You want your agents focusing on the coding itself. If you try to make them deal with commits, they will do a poor job."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022377209848328610))

> "Then they just keep cranking on their own for a really long time. And you don't need to supervise them much so you can be juggling multiple projects like this at once."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619))

1. **One canonical plan per project.** Not a plan plus a design doc plus an architecture doc. One document, one source of truth. Multiple documents drift apart and agents read stale versions.

2. **AGENTS.md is written by hand** (or heavily directed). It embodies the human's taste, architecture, and quality bar. A generated AGENTS.md carries no conviction and agents treat it accordingly.

3. **Agents are fungible generalists.** No roles, no specialization, no backstories. "You are a senior backend engineer" narrows the model's solution space. Let any agent pick up any bead.

4. **The commit agent is separate from coding agents.** Coding agents forced to commit produce lazy messages, monolithic commits, and accidentally bundle other agents' changes. One dedicated agent running EX-06 does nothing but group, document, and push.

5. **RV-02 Deep Review is a numbered series,** not a single event. Part 1, Part 2, ... Part 23+. Each pass shifts the model's attention to a different region of the codebase, catching things prior passes missed.

6. **Bead IDs appear in every commit message.** This is the traceability layer. Without it, you cannot tell which commit implements which task, and agents reading git log lose their coordination surface.

7. **After beads exist, PLAN.md is closed.** The bead-map is the execution surface. If an agent re-reads the plan during execution, something went wrong in bead conversion. Fix the beads, not the plan.

8. **Hooks on bead close cause damage.** Auto-running tests or linters when a bead is marked done catches the code in an incomplete state. Use RV-01 Self-Review instead; it runs at the agent's discretion when the work is actually finished.

9. **Tests must not use mocks.** Real data, real API calls, real end-to-end. Mocks prove the mock works, not the code. In an agentic swarm, a passing mocked test gives false confidence that propagates across agents building on top of the supposedly-working code.

10. **Recovery runbooks are a planning deliverable,** not a post-failure afterthought. Write them during Phase 1-2 while you still have full context on what can go wrong.

11. **Pre-integration dependency analysis is required.** Write COMPREHENSIVE_ANALYSIS before integrating anything. Agents that skip this step introduce dependencies they don't understand, creating fragile coupling that surfaces as mysterious failures later.

12. **Agent failure is normal operations.** Agents crash, context compacts, sessions die. Design for it: no single points of failure, advisory file reservations that expire, semi-persistent identity that doesn't break when an agent vanishes.

13. **Profiling is a scheduled bead,** not reactive debugging. Performance work happens proactively on a cadence, not as a fire drill after users report slowness.

14. **Marathon sessions beat distributed sessions.** Context is perishable. A 17-hour continuous session with full context produces better work than five 3-hour sessions where the agent rebuilds understanding each time.
