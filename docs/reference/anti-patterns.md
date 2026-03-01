---
icon: lucide/alert-triangle
---

# Anti-Patterns

10 ways to destroy your project.

---

### 1. Skeleton-first development

"Just get something working, then improve it." No. By the time you have something working, the architecture is already compromised and you'll spend 10x longer fixing it than you would have spent planning it right.

### 2. Specialized agents

"You are an expert senior backend engineer with 15 years of experience in..." Stop. Frontier models understand intent. Role-playing constraints narrow the solution space. Use fungible generalists.

### 3. Communication purgatory

Agents messaging each other endlessly without shipping code. The bead system and AGENTS.md solve this architecturally. If it keeps happening, your AGENTS.md is unclear.

### 4. Separate design docs

The plan is the design doc. There is one canonical plan. Not a plan plus a design doc plus an architecture doc. One document, one source of truth.

### 5. Skipping the praise rounds

"I'll just go straight to analytical critique." The praise rounds push the model past its default comfort zone. Analytical critique refines; praise rounds expand.

### 6. Mocked tests

"We need to have totally complete, totally comprehensive, granular, perfect end to end testing coverage without ANY mocks or fake data, fake api calls, etc." Tests with mocks prove that mocks work, not that code works.

### 7. Reopening the plan during execution

After beads exist, PLAN.md is closed. The bead-map is the execution surface. If an agent re-reads the plan during P08, something went wrong in bead conversion.

### 8. Skipping the premortem

If you can't imagine how your project fails, you haven't thought about it hard enough. The premortem pass catches things optimistic planning never will.

### 9. One review pass

Part 1 is not enough. Part 2 catches what Part 1 missed. Part 5 catches what Part 4 missed. Part 23 still finds things. Keep going until the diffs flatline.

### 10. Optimization theater

Proposing performance improvements without profiling first. The Deep Performance Audit methodology exists because most "optimizations" are guesses that don't move the needle.
