---
icon: lucide/git-compare
---

## 1.3 What Makes This Different

### 1.3.1 vs. Cursor / Copilot / Windsurf (IDE-Assist)

IDE-assist tools are reactive by design. They watch what you type and suggest completions, or they accept natural-language instructions and generate code in your editor. They are excellent at what they do: reducing keystrokes, generating boilerplate, and answering inline questions about code.

What they do not provide:

- **A persistent plan.** Each prompt exists in isolation. There is no shared architecture document that all suggestions align with.
- **Multi-agent coordination.** You get one model in one context window. If you want a second opinion, you switch tabs manually.
- **Cross-session memory.** Context resets when you close the window. The model that helped you yesterday does not remember today.
- **Task decomposition.** There is no mechanism to break a project into atomic units and track their completion.

The Flywheel methodology is tool-agnostic. It operates at a layer above any individual IDE or agent. The plan, the bead-map, and the coordination infrastructure (Agent Mail, NTM) work with any coding agent that supports MCP or can read files.

> *"My system has worked perfectly with Cursor for 4 months since I first released it. It uses MCP so it works with everything. Standards!"*
> — Jeffrey Emanuel

You can use Cursor as one of your execution agents inside the Flywheel. The point is that Cursor alone, like any IDE-assist tool, provides generation without coordination.

---

### 1.3.2 vs. Devin / OpenHands / SWE-agent (Autonomous Agents)

Autonomous agent platforms attempt to handle the entire development cycle: understand the task, plan the approach, write the code, test it, iterate. The promise is fully hands-off development.

In practice, these systems work well on bounded, single-feature tasks (the kind that SWE-bench measures). They fail on multi-feature applications where components must interact coherently over a long execution window. This has been documented repeatedly by practitioners:

> *"Build systems that decompose a problem in a haystack of needles."*
> — @TheodoreGalanos, Feb 2025

The Flywheel's approach to this problem is different in a specific way: the **human remains the strategic planner**. Agents do not decide *what* to build; they execute against a pre-existing plan. The plan absorbs the complexity that autonomous agents cannot handle, because the plan was written (and stress-tested through multi-model competition) before any agent started coding.

> *"All my experiments have failed when starting with autonomous agents. So I'M the orchestrator."*
> — ACFS community member

The tradeoff is explicit: the Flywheel requires significant human effort in the planning phase. Autonomous agents require less human effort upfront but produce lower-quality results on complex projects. For projects over ~5K LOC with multiple interacting components, the upfront planning investment pays for itself.

---

### 1.3.3 vs. "Just Use Claude Code" (Raw CLI)

Claude Code is the primary execution engine in most Flywheel setups. The question is what you wrap around it.

Raw Claude Code gives you one agent in one context window. It can spawn subagents, but each subagent operates in a context silo; it cannot see what the other subagents are doing, and the human cannot easily monitor multiple subagent streams.

> *"I tried the orchestrator agent pattern today across a 3-project stack and while it was neat, some sub agents made silly mistakes due to the context silos and I couldn't see inside the agent streams while they worked."*
> — @ShannonCode

The Flywheel adds three layers on top of raw Claude Code:

1. **The plan document** as shared context that all agents can read.
2. **Agent Mail** for async inter-agent messaging (agents report status, request help, coordinate merges).
3. **NTM + tmux** for human oversight of multiple agent sessions simultaneously.

These layers let you run 4, 8, or 12+ Claude Code instances on the same project, each working on a different bead, coordinating through a shared bead-map and agent mail rather than through a single orchestrator's context window.

> *"Thinking is not scalable and it is already my bottleneck."*
> — @steipete (Peter Steinberger)

The Flywheel's answer to this bottleneck is to front-load the thinking into the plan, then parallelize the execution. The human thinks once (during planning), and that thinking is distributed to all agents via the plan and bead-map.

---

### 1.3.4 The Missing Layer: Planning Leverage

Across all three comparisons, the pattern is the same. IDE tools, autonomous agents, and raw CLI agents all lack the same thing: a structured, version-controlled planning phase that converts high-level intent into an executable task graph before any code is written.

This planning phase is the leverage point. It is where:

- Architectural decisions are made and stress-tested (via multi-model competition)
- The work is decomposed into units small enough for a single agent session
- Dependencies are mapped so agents can work in parallel without conflicts
- Acceptance criteria are defined so "done" is unambiguous

Without this layer, every other tool is just generating code faster into a void of uncoordinated decisions. With it, any tool becomes an execution engine for a coherent plan.

> *"My approach really works. You don't have to be some crazy savant to apply it, either. Just read my posts about planning and use the tools and workflows, and you will also get these kinds of results. I've now seen at least 10 people who I don't even know report similar outcomes."*
> — Jeffrey Emanuel

The following section covers the economics of this approach: what it costs in time and money, when the investment is justified, and when it is overkill.

---


---

!!! tip "Related pages on this site"

    - [Toolchain Reference](../reference/toolchain.md)

