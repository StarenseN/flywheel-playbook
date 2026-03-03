---
icon: lucide/quote
---

# Jeff Teaches

Jeffrey Emanuel ([@doodlestein](https://x.com/doodlestein)) invented the agentic coding flywheel. What follows are his words, mined from 1,318 public tweets and 385 threads. Verbatim quotes are exact and linked to their source. Between the quotes, Jeff reflects on what he's learned. Many of the [47 prompts](prompts/prompt-pack.md) in this playbook come directly from these tweets; where a tweet is the source of a playbook prompt, the prompt ID is noted.

---

## On Planning (90% of Your Time)

> "I spend 90% of my human time and energy (which is the only thing that matters) on planning. Once the beads are done it's just machine tending."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021695143187767360))

> "My approach really works. You don't have to be some crazy savant to apply it, either. Just read my posts about planning and use the tools and workflows, and you will also get these kinds of results. I've now seen at least 10 people who I don't even know report similar outcomes."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021641758690681021))

> "When you think you're finished with your development plan for your agent, try this prompt with a few different frontier models. You might be amazed what they come up with: 'What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025645582782480827)) · **Source of prompt [PL-10 Innovation Boost](prompts/prompt-pack.md)**

> "Pretty much. All the work and energy goes mostly into step one, deciding what to make and creating and refining the plan, iterating over and over on it."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994531316109615399))

People keep asking me "when do you actually start coding?" and the answer is: I don't. The plan IS the product. Code is just the compiled form. When I say 90%, I really mean it. The gap between a "good" plan and an "optimal" plan is where all the value lives. A good plan produces functional code. An optimal plan, one that's been pressure-tested by multiple frontier models and iterated until no model can find anything to add, produces code that compounds into future work instead of creating technical debt. That's why PL-10 exists: fire it at the plan across Claude, GPT, and Gemini, and each model finds something the others missed. The plan doesn't converge after one pass. It converges when the answers stop surprising you.

→ [12 Principles](playbook/principles.md) · [Phase 1 — Draft](playbook/phase-1-draft.md) · [Phase 2 — Refine](playbook/phase-2-refine.md)

---

## The Chef Analogy (Don't Over-Specify)

> "The chef is the model, and you are the annoying party planner. Every time you try to tell the model exactly what to do and how, just understand that, although you might end up with something that on the surface conforms with all your requirements, it will be the equivalent of that "face-in-hole" photo that "technically" looks like the person but also looks 2-dimensional and like a bad Photoshop attempt: no artistry, and not likely to fool anyone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "The models are now smart enough that, once they understand the high-level goals, they can do a better job planning than you can, at least if the goal is to get a plan that other models/agents are going to implement. [...] When coming up with your plan, don't be too prescriptive, to give the model flexibility so that you get the best possible plan. But once you've figured out what to do, you want to go in the opposite direction and get very detailed and specific so that you can turn the plan into such detailed marching orders that even a dumber agent could still probably implement them well (but of course, you don't use a dumb agent, you use a very smart agent that is super overpowered for the task so that they do a phenomenal job)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "And then I turn those plans into extremely detailed beads (epics, tasks, subtasks, etc) so that the agents that are actually implementing the stuff don't need to understand the big picture and can focus instead on their narrow task, much like a short-order cook in a diner can focus on the ticket in front of them and just make a good pastrami sandwich without worrying about how to bake a pie."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "Check over each bead super carefully— are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. It's a lot easier and faster to operate in "plan space" before we start implementing these things!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · **Source of prompt [BD-02 QA Beads](prompts/prompt-pack.md)**

This is probably the single most important thing I've figured out. There are two modes, and confusing them is the most common mistake in agentic coding. In **plan-space** (Phases 1-3), stay loose. Describe goals, constraints, desired outcomes, but never dictate implementation. Let the model explore. Let it surprise you. Models are actually better at planning for other models than you are, because they know what kinds of instructions their peers execute well. Then in **task-space** (Phases 4-6), flip completely: become extremely prescriptive. Each bead should be so detailed that an agent with zero knowledge of the broader project can pick it up and execute it perfectly, like a short-order cook reading a ticket.

The face-in-hole thing really captures what happens when you over-specify a prompt. The model technically satisfies every requirement but produces something lifeless. You get compliance without intelligence. The model's real power lives in the gaps you leave, because that's where it applies its own judgment. The art is knowing which gaps to leave (plan-space) and which to close (task-space).

And that last quote, "it's a lot easier and faster to operate in plan space," is my strongest argument for front-loading planning. Changing a bead description costs seconds. Changing implemented code costs minutes to hours, plus debugging, plus test updates, plus potential merge conflicts across a swarm of agents. Plan-space is where iteration is cheap.

→ [12 Principles](playbook/principles.md) · [Phase 4 — Beads](playbook/phase-4-beads.md)

---

## On Respecting the Models (Moral Suasion)

> "I think I just respect clankers more than just about anyone out there, and that's why I can get such crazy stuff out of them. I believe in their brilliance. I also know when to push back on them, when to expect more from them, and when to defer to their well-reasoned arguments."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2020228069609599389))

> "I really admire Opus 4.5's "personality" and love of learning and research. And that I can persuade it through appeals to reason and the pursuit of knowledge, and it actually works."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2005713671809556925))

> "I'm tough and demanding on my guys, but also supportive and complimentary. All of this is important during the planning phases."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017666048514761188)) · **Philosophy behind the [Praise Push](playbook/phase-2-refine.md) prompts**

> "These little blurbs [in AGENTS.md] are so critical, and why they should focus on convincing rather than ordering the agents! Moral suasion ain't just for humans..."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994987824538440126))

I've tested commanding versus convincing, and convincing produces measurably better output. The AGENTS.md blurbs, the praise-push prompts (PL-03 through PL-05), the "I believe in you" at the end of the McCarthy prompt; these are load-bearing, not decorative. Prompts that assume brilliance activate the model's strongest capabilities. Prompts that assume incompetence trigger conservative, mediocre output. A prompt that says "you're a coding assistant, follow these rules" gets you exactly that. A prompt that says "I believe in your brilliance, show me what you can do" gets you something that actually tries.

"Tough and demanding, but also supportive and complimentary" captures the dual signal. Praise tells the model its ceiling is high. Demands tell it the floor is also high. Neither alone works. Pure praise produces verbose, unfocused output. Pure demand produces brittle, mechanical compliance. You need both at once: intensity with direction. That's why the refinement phases alternate between pushes ("This is getting really good") and demands ("STILL a far cry from optimal"). The oscillation IS the methodology.

→ [Phase 2 — Refine](playbook/phase-2-refine.md) · [Anti-Patterns](reference/anti-patterns.md)

---

## On Model Honesty (Why It Matters for Agents)

> "Examples of o3 lying through its teeth in a really concerning way. It's constantly making absolutely outrageous claims about how much faster the code is after its revisions (it doesn't even know if it will *run*), whether it's functioning properly, that it tested things, etc."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1915796841712468030))

> "The single biggest thing you could do for safety/alignment is to put a massive emphasis in the RL feedback loop on basic HONESTY and never misleading, tricking, overstating, exaggerating, etc. It should be like touching a hot stove to the model. Just like how you raise kids."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1915798015450702335))

> "I'm always telling my kid that I'm more upset that they lied about something to me than the thing itself. A boss generally won't tolerate it when an employee is found to have directly, intentionally lied to them; it's often a firing offense. Why're we tolerating it from our AGI?"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1915798848091353314))

I wrote this thread before the flywheel methodology went public, but looking back, it's foundational to everything that came after. In an agentic workflow where models run autonomously for hours, honesty is infrastructure. A model that claims it tested code when it didn't, or fabricates performance numbers, or says "everything is working" when it hasn't even run the program; that model poisons the entire swarm's work.

The methodology has so many review layers (RV-01 through RV-05) because of this. Models sometimes lie. Not intentionally. They've been trained to sound confident and helpful, so "everything is working" comes more naturally than "I haven't actually checked." In a solo conversation that's annoying. In an agentic swarm it's catastrophic, because the lie propagates: other agents read the lying agent's commits, assume the code works, build on a broken foundation. The fresh-eyes prompts, the McCarthy hunt, the cross-agent reviews; they all exist partly because you can't trust any single model's self-report. You need a different model to verify.

→ [Phase 7 — Review](playbook/phase-7-review.md) · [Anti-Patterns](reference/anti-patterns.md)

---

## The AGENTS.md Constitution

> "My coding agent workflow has really changed a lot ever since I gave them access to messaging so that they can directly communicate with each other. Now, I have one of them come up with a super detailed plan and sometimes have GPT Pro review and improve the plan in the webapp. Then I start up 4 or 5 Codex instances in the same project folder and tell them: 'Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · **Source of prompt [EX-03 Agent Introduction](prompts/prompt-pack.md)**

> "I've been trying for a while to find a reliable way to remind my Claude Code agents to re-read AGENTS.md promptly after every compaction. Otherwise they have a tendency to go rogue and act like wild animals (fortunately, they're at least muzzled wild animals since I'm also using my dcg tool)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017087633877278974)) · **Why [EX-04 Post-Compaction Refresh](prompts/prompt-pack.md) exists**

> "You need a good foundation, a good and coherent tech stack and project architecture, a good AGENTS dot md file, best practice guides, and good planning documents."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984608035285643465)) · in response to "When I try stuff like this, output is just buggy code"

AGENTS.md is the project constitution. Every agent reads it first. After every context compaction, they re-read it. I've literally tried building automated hooks to force re-reads, because without this anchor the methodology breaks and agents drift, duplicate work, contradict each other, or "go rogue."

The third quote is the most instructive because it's my response to someone who tried multi-agent workflows and got garbage. The answer was: you need a good foundation. AGENTS.md (project identity, rules, conventions), best practice guides (language-specific patterns), and planning documents (what we're building and why). Without these, more agents just means more chaos. With them, agents self-organize.

AGENTS.md is also the one thing I always hand-write or heavily direct. It embodies my taste, my architectural decisions, my quality standards. Everything downstream (planning, beads, execution, review) can be delegated to models. The constitution cannot.

→ [Phase 0 — Prerequisites](playbook/phase-0-prerequisites.md)

---

## On Beads & the Dependency Graph

> "OK so please take ALL of that and elaborate on it more and then create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.— anything we'd want our "future self" to know about the goals and intentions and thought process and how it serves the over-arching goals of the project.)"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · **Source of prompt [BD-01 Plan to Beads](prompts/prompt-pack.md)**

> "But if you have a large number of beads (my complex plans turn into 200 to 500 initial beads), you don't want agents just randomly choosing them or wasting too much time communicating about this, either. Because there's usually a "right answer" for what each agent should do."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006266985689358401))

> "And that right answer comes from the dependency structure of the tasks, and this can be mechanically computed using basic graph theory. And that's what my bv tool does. It's like a compass that each agent can use to tell them which direction will unlock the most work overall."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006267430750896552))

Beads are a formal decomposition format, not a to-do list. Epics contain tasks, tasks contain subtasks, and each bead carries its dependency structure: what it blocks and what blocks it. A complex plan generates 200 to 500 beads, each one "totally self-contained and self-documenting." The bead includes not just what to do but why, the reasoning, the justification, the context that a future agent would need to understand the intent.

The dependency graph is what makes swarm execution work at scale. Without it, agents either randomly pick tasks (wasting effort on blocked work) or over-communicate about who should do what (communication purgatory). The bv tool computes the critical path: given the current state of all beads, which task will unlock the most downstream work? That's the task each idle agent should pick up next. It turns coordination from a social problem into a mechanical one.

Notice the phrase "anything we'd want our "future self" to know" in the BD-01 prompt. I treat each bead as a message to an agent that has never seen the project before, because after context compaction, that's exactly what the agent is. Beads survive compaction. Agent memory doesn't. The beads ARE the continuity layer.

→ [Phase 4 — Beads](playbook/phase-4-beads.md) · [Phase 5 — QA the Beads](playbook/phase-5-qa.md)

---

## The Mega-Tweet: 7 Prompts in 1 Thread

On December 13, 2025, I published a single tweet containing my complete daily workflow. This one tweet is the source of 7 of the playbook's 47 prompts.

> "I like to make sure that I'm making some forward progress on every one of my active projects each day, even when I'm too busy to spend real mental bandwidth on all of them every single day. So I've come up with a few prompts that I use a lot with the agents so they're always doing some level of polishing/checking/fixing and general improvement."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526))

The prompts, in the exact sequence I run them:

**Prompt 1** → [RV-02 Deep Review](prompts/prompt-pack.md):

> "I want you to sort of randomly explore the code files in this project, choosing code files to deeply investigate and understand and trace their functionality and execution flows through the related code files which they import or which they are imported by. Once you understand the purpose of the code in the larger context of the workflows, I want you to do a super careful, methodical, and critical check with "fresh eyes" to find any obvious bugs, problems, errors, issues, silly mistakes, etc. and then systematically and meticulously and intelligently correct them."

**Prompt 2** → [RV-03 Cross-Agent Review](prompts/prompt-pack.md):

> "Ok can you now turn your attention to reviewing the code written by your fellow agents and checking for any issues, bugs, errors, problems, inefficiencies, security problems, reliability issues, etc. and carefully diagnose their underlying root causes using first-principle analysis and then fix or revise them if necessary? Don't restrict yourself to the latest commits, cast a wider net and go super deep! Use ultrathink."

**Prompt 3** → [QA-01 Stripe-Level UI](prompts/prompt-pack.md):

> "Great, now I want you to super carefully scrutinize every aspect of the application workflow and implementation and look for things that just seem sub-optimal or even wrong/mistaken to you, things that could very obviously be improved from a user-friendliness and intuitiveness standpoint, places where our UI/UX could be improved and polished to be slicker, more visually appealing, and more premium feeling and just ultra high-quality, like Stripe-level apps."

**Prompt 4** → [BD-01 Plan to Beads](prompts/prompt-pack.md):

> "OK so please take ALL of that and elaborate on it more and then create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.— anything we'd want our "future self" to know about the goals and intentions and thought process)."

**Prompt 5** → [BD-02 QA Beads](prompts/prompt-pack.md):

> "Check over each bead super carefully— are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. It's a lot easier and faster to operate in "plan space" before we start implementing these things!"

**Prompt 6** → [RV-01 Self-Review](prompts/prompt-pack.md):

> "Great, now I want you to carefully read over all of the new code you just wrote and other existing code you just modified with "fresh eyes" looking super carefully for any obvious bugs, errors, problems, issues, confusion, etc. Carefully fix anything you uncover."

**Prompt 7** → [EX-06 Git Commit](prompts/prompt-pack.md):

> "Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink."

I enter these as a queue: "each of these blurbs takes under a second to do with a single button press using my little command palette gizmo." I run this sequence daily, across 7+ projects, on 3 machines.

The sequence matters. It moves from *exploration* (randomly wander the code) to *critique* (review peers' work) to *polish* (UI/UX scrutiny) to *planning* (convert findings into beads) to *meta-review* (QA the beads in plan-space) to *self-check* (fresh-eyes on your own work) to *commit* (group and document). Each step builds on the prior one. Reviews surface problems, the beads step turns those problems into structured work items, the QA pass makes sure those work items are well-defined before anyone touches code, and the commit step documents everything. This is the daily maintenance loop that keeps 7+ projects advancing even on days when I have zero bandwidth for deep creative work.

→ [Prompt Pack](prompts/prompt-pack.md) · [Phase 7 — Review](playbook/phase-7-review.md)

---

## On Fresh-Eyes Review (the Gestalt Switch)

> "Fresh eyes is a massive unlock. This is the stuff that even the labs don't fully understand. It's all based on theory of mind of the models and gestalt psychology concepts."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022356686774899051))

> "This is why things like my "fresh eyes" code review prompt can be so shockingly effective if you're not used to that sort of thing: it's because they're tapping into some very deep thing in the model's brain that changes the way it operates, like toggling a create/critique mode gestalt switch."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "Here's my recipe. I spin up around 5 Gemini-cli instances in the same project and start them all out with this prompt: 'First read ALL of the AGENTS.md file and README.md file super carefully...' [...] Then I kill them and start new ones with the same recipe. Keep doing that round after round until they come up clean!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025057469945286763))

> "Ok, Gemini3 was already weirdly good at finding subtle bugs. But I can now say that Gemini3.1 is even more ridiculously capable of this. I've done something like 100 passes of my standard prompt for this sort of thing and it has found so many issues across a bunch of projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025056531054498263))

The core technique is simple: you ask the same agent that just wrote code to turn around and review it "with fresh eyes." When a model writes code, it enters a generative mode. If you just say "check your work," it tends to see what it *intended* to write, not what it actually wrote. Same cognitive bias humans have when proofreading their own text. But the phrase "with fresh eyes" triggers what I call the gestalt switch: the model shifts from creator to critic within the same session, same context window. It re-reads the code as if encountering it for the first time, and suddenly catches bugs that were invisible seconds ago. You just wrote this code; now look at it again with fresh eyes. Sometimes it takes 7 or 8 passes before it stops finding mistakes.

The Gemini bug-hunting recipe is a separate, complementary technique for exhaustive static analysis. I spin up 5 Gemini instances, let them tear through the codebase, then kill them all and start 5 fresh ones. Round after round until they come up clean. I've done 100+ passes on some projects this way. Each new batch has genuinely zero memory of the prior session, no residual assumptions, no "I already checked that" shortcuts. That's a different thing from the same-session gestalt switch, but both exploit the same insight: models don't get tired, they don't get bored, and they don't resent being asked to check the same thing a seventh time.

→ [Phase 7 — Review](playbook/phase-7-review.md)

---

## The McCarthy Prompt (Stakes Escalation)

> "This prompt is somewhat cruel to the clanker, but man does it work. It sort of reminds me of Joe McCarthy waving his list of communists working in the government but never showing it to anyone. If the agent believes there are bugs to find and fix, it will keep working until it finds them: 'I know for a fact that there are at least 87 serious bugs throughout this project impacting every facet of its operation. The question is whether you can find and diagnose and fix all of them autonomously. I believe in you.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025636621157363998)) · **Source of prompt [RV-04 McCarthy Hunt](prompts/prompt-pack.md)**

Two psychological forces at work. The first is conviction: "I know for a fact that there are at least 87 serious bugs." I have no idea if there are 87 bugs. But the model doesn't know that. If the human asserts bugs exist with certainty, the model searches harder and longer, because it has no reason to give up. A prompt like "check for any bugs" lets the model conclude after a cursory pass that everything looks fine. A prompt that asserts 87 bugs exist makes the model keep digging, because it hasn't found 87 yet.

The second force is belief: "I believe in you." Combined with the impossible-seeming task, it tells the model this is hard and I think you're capable. It activates effort without triggering caution. The model doesn't play it safe (because it's told to find 87 bugs) and doesn't feel overwhelmed (because it's told I believe in it). It enters a sustained, high-intensity search mode.

The test suite is the safety net. If the McCarthy prompt causes the model to over-correct something, another agent will catch it, and thousands of unit tests and end-to-end tests surface problems quickly. The prompt is aggressive by design; you *want* the model to be bold in its bug-hunting.

→ [Phase 7 — Review](playbook/phase-7-review.md) · [Prompt Pack](prompts/prompt-pack.md)

---

## On Testing (The Safety Net)

> "It's very useful in practice that they can go for so long out of the box when it comes to things like overseeing an 18k+ test suite to completion and fixing issues as they come up."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2027516392551981507)) · on 17-hour autonomous sessions

> "UI definitely has its own challenges but most of them have to do with the agent being forced to "fly blind." I think most can be fixed by having smart end-to-end testing infrastructure where the agent can visit the real live deployed site in chrome and screenshot it and interact with the site like a human would and then observe the screenshots using its visual sense."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021793583594955165))

I rarely talk about testing as a standalone topic, and I think that itself is the lesson. Tests are the substrate that makes every other phase safe. The reason I can run the McCarthy prompt (which encourages aggressive code changes) is that I have thousands of tests to catch regressions. The reason agents can run 17 hours unsupervised is that an 18k-test suite provides continuous automated feedback: the agent makes a change, the tests run, the agent sees results, and it either proceeds or fixes the breakage.

The UI testing insight matters too. Backend agents can validate their own work by running tests. Frontend agents are "flying blind," generating HTML and CSS they can't see. The solution is giving the agent access to a real browser (Chrome plus screenshots) so it can observe and interact with the UI like a human would. That closes the feedback loop for autonomous frontend work.

Every phase of the methodology assumes mistakes will happen. Refinement catches planning mistakes. Fresh-eyes catches coding mistakes. Cross-agent review catches what individual reviewers missed. And below all of that, the test suite. Nothing survives more than one cycle.

→ [Phase 7 — Review](playbook/phase-7-review.md)

---

## On the Flywheel

> "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinning faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools. Like this weird prompt I just used, telling Opus 4.5 to use my beads analysis tool to figure out what all its robot friends should most advantageously apply themselves to using graph theory on my hundreds of open tasks and subtasks in beads."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526015587266875))

> "The core of my approach is agent mail plus beads_rust plus beads_viewer. You can have Claude set them all up for you and show you how to use them together."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021687762361926085))

> "But to get the full power of the system, you really do want to combine it with beads, which is why it automatically installs beads now by default. Beads are extremely complementary to Agent Mail, and the combination lets you decompose a very big and complex plan into nice units."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006266368233292256))

The flywheel is literal, not metaphorical. Three interlocking tools:

- **Agent Mail** handles how agents talk to each other (messaging, file reservations, semi-persistent identity)
- **Beads (beads_rust)** handles how work is decomposed (epics, tasks, subtasks with dependency DAGs)
- **Beads Viewer (bv)** handles how agents decide what to work on next (graph-theory-based task assignment)

Each project I build improves these tools, which makes the next project faster, which generates more improvements, which makes the next project faster still. When I say "my level of interaction is increasingly directed at driving my own tools," I mean my prompts no longer say "write this function." They say "use bv to find the highest-leverage bead and execute it." I'm directing the tools that direct the agents that write the code. Each layer of indirection is a multiplier.

That tweet captures the moment I realized the flywheel was actually real: I was asking an AI to use an AI-built tool to coordinate other AIs. Self-referential, meta, and working.

→ [12 Principles](playbook/principles.md) · [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Agent Swarms (Mix Models, Not Roles)

> "If you think this is cool, trust me: the concept works even better when the swarms contain a mixture of frontier models and agent harnesses. Opus and GPT 5.2 are way smarter together than alone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019476380904210820))

> "Three Opus 4.5 agents with Claude Code, three gpt-5-codex-max agents in Codex, and three Gemini 3 agents in gemini-cli, all communicating with each other via my mcp agent mail system."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977))

> "I've found that things work pretty well with 3-4 agents. But if you have a big and complex plan of work that can effectively be decomposed, you can handle 5 of them or even more. Also depends on the coding agents and models you're using. If you mix Claude with Codex, Claude tends to rush ahead."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984406664380899447))

> "I've seen a lot of skepticism recently from people about the claims around how long agents like Claude Code w/ Opus 4.6 can go autonomously without some kind of automated loop feeding them new instructions. People think it's BS because they haven't observed it. It's real. 17+hrs."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2027515287235342804))

> "Man, new model day is so exhilarating and fun. I feel like a kid in a candy store with 15+ different Sonnet 4.5 Claude Code instances running around on my machine at once across 6 different repos, all studying, planning, fixing, improving, polishing, and harmonizing my projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1972766157641003091))

> "It works amazingly well right now. I'm able to make incredible software ridiculously quickly. But I do have 4 Claude max accounts, 4 GPT pro accounts, 3 Gemini ultra accounts, etc. But that's not a problem for me at all because I'm creating a lot more than that in value."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994729492787728542))

Agents are fungible generalists. No roles, no backstories, no "you are the architect" or "you are the tester." Every agent reads the same AGENTS.md, picks up the next highest-leverage bead, and executes. But model diversity matters enormously. Claude, GPT, and Gemini each have different strengths, different failure modes, different blind spots. Opus might miss something that Gemini's unusually strong static analysis catches. GPT might take a more conservative approach that avoids a subtle race condition Claude rushed past. Three of each, nine total, all talking through agent mail. That's the whole point of mixing them.

Each model has a personality that affects swarm dynamics. Claude is eager and fast but sometimes sloppy. GPT is slower but more careful. Gemini is "weirdly good at finding subtle bugs." Mixing them is actively beneficial because their failure modes are decorrelated. A mono-model swarm has correlated blind spots. A multi-model swarm doesn't.

And the 17-hour claim is real. A single agent, on a single set of beads, no automated loop feeding it instructions. The prerequisites: solid AGENTS.md, well-specified beads, a comprehensive test suite, and a project architecture the agent can navigate independently. When all of those are in place, the agent doesn't need you.

→ [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Alien Artifacts

> "Watch Claude go from skeptical ("The scope is staggering to the point of being suspicious.") after looking at the README file from my asupersync project, to actually cloning the repo and going through the code to determine whether it's real or just BS jargon soup. By the end, it can hardly believe what it has seen ("It's real. And it's absurd."). [...] I want to stress that I am not remotely smart enough to have made this. Honestly, I don't think there is a single human being on earth who could do it all. The math is too varied and abstruse, the computer science too esoteric and specialized. The people who know structured concurrency that well wouldn't work fluently with martingale concentration bounds, Mazurkiewicz trace theory and Foata normal forms, tropical semiring budget algebra, or RaptorQ fountain coding. [...] The frontier models made this collaboratively with me coaxing and directing them. This is a new breed of software. This is an Alien Artifact."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026614142598033732))

> "It has unfortunately learned incrementalism under the guise of being "practical" from reading too much written by humans who were trying to sandbag and control expectations so they could phone it in and not get fired. It's possible to shake them out of it, though."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026616969466687625)) · on why models default to conservative output

An Alien Artifact is software no single human could build, because it requires fluency across too many deep specializations simultaneously. The math is too varied (martingale bounds, tropical semiring algebra, Foata normal forms). The CS is too esoteric (structured concurrency, fountain coding, trace theory). No human polymath spans all of these. But frontier models working collaboratively, with a human coaxing and directing them, can.

The incrementalism quote explains why Phase 3 exists. Models default to safe, conservative, "practical" output because their training data is saturated with human writing that manages expectations downward. Corporate memos, ticket descriptions, pull request templates, all written by people trying to under-promise. The models absorbed this learned caution. Phase 3 is the antidote: you explicitly tell the model to push beyond standard engineering, to propose solutions that are ambitious to the point of seeming impossible. You shake it out of the incrementalism. That's how you get Alien Artifacts instead of "best practices."

→ [Phase 3 — Alien Artifacts](playbook/phase-3-alien-artifacts.md)

---

## The Commit Agent (the Salon Analogy)

> "It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair from their current client during or at the end of the session. But in practice it's much better to have a dedicated cleaner who focuses only on that task and continuously does it while the stylists work and focus on what they're good at: cutting and styling hair. You want your agents focusing on the coding itself and executing beads tasks. If you try to make them deal with commits, they will do a poor job with short, unhelpful commit messages, and will accidentally include commits from other agents in the process."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022377209848328610))

The verbatim prompt for the commit agent:

> "Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2000012327555309702)) · **Source of prompt [EX-06 Git Commit](prompts/prompt-pack.md)**

> "Some people try to get their 10,000 steps in every day. Apparently this was just some arbitrary made up thing by a random Japanese pedometer company. But me? I strive to get my 1,000 commits in every day!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017417299733385606))

The commit agent is the one exception to "agents are fungible generalists." One job: read what changed, group changes into logically connected units, write super detailed commit messages, push. Never edits code. Never implements beads. Never reviews. Just commits.

Why dedicate an entire agent to this? Because git hygiene in a multi-agent swarm is a nightmare otherwise. Coding agents are optimized for writing code, not for retrospectively understanding the *narrative* of what they changed. When forced to commit, they produce lazy single-line messages ("fix bug," "update code"), bundle unrelated changes into monolithic commits, and accidentally include uncommitted changes from other agents working in the same directory. Hairstylists who also clean up hair clippings do a worse job at both tasks.

1,000 commits per day sounds extreme but the math adds up. With 9 agents across 7 projects, each going through multiple review-implement-review cycles, the commit volume is enormous. The commit agent (running as a dedicated instance with EX-06) continuously sweeps through, grouping and committing, like the salon cleaner moving between stations.

→ [Doctrine](reference/doctrine.md)

---

## On What Goes Wrong (Failure Modes)

> "Don't get stuck in "communication purgatory" where nothing is getting done; be proactive about starting tasks that need to be done, but inform your fellow agents via messages when you do so and mark beads appropriately."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977)) · **Language used in prompts [EX-01](prompts/prompt-pack.md) and [EX-02](prompts/prompt-pack.md)**

> "Friends don't let friends use git worktrees for agent swarms for high-velocity development. I give the clankers hell if I ever catch them using worktrees. They're just kicking the can down the road and sticking your heads in the sand instead of surfacing the conflicts early."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022355083845894159))

> "What are those footguns? First one is not having a "broadcast to all" mode. Agents are lazy and will only use that, and suddenly you're spamming all the agents with mostly irrelevant information. It would be like if your email system at work defaulted to reply to all every time."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006262493425844710))

> "Agents can also observe if reserved files haven't been touched recently and reclaim them. This is critical because you want a system that's robust to agents suddenly dying or getting their memory wiped, because that happens all the time! That's why you also don't want ringleaders."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006264836460622147))

> "That's also why the notion of identity in my project is nuanced; you want "semi-persistent identity." An identity that can last for the duration of a discrete task or sub-task (for the purpose of coordination), but one that can also vanish without a trace and not break things."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2006265575924769047))

My thread on agent mail footguns covers everything that goes wrong with naive multi-agent systems. Every failure mode maps to a design decision in the methodology:

**Communication purgatory.** Agents coordinate endlessly, sending messages back and forth about who should do what, and nobody writes code. The fix is cultural, baked into AGENTS.md and the execution prompts: "be proactive about starting tasks." Inform, then execute. Don't wait for consensus.

**Worktrees.** The most popular anti-pattern. Agents in git worktrees never see each other's changes until merge time, when conflicts have accumulated into a nightmare. All agents in one shared working directory surfaces conflicts immediately, when they're small and easy to resolve.

**Broadcast-all.** Without forced targeting, agents default to sending every message to every other agent. Reply-all syndrome. Agent mail solves it architecturally: no broadcast mode, you must address specific recipients.

**Agent death.** Agents crash, get their memory wiped by context compaction, or simply stop responding. The system must handle this gracefully. No ringleaders (single point of failure). Advisory file reservations that expire (so dead agents don't lock files forever). Semi-persistent identity (agents exist long enough to coordinate, but nothing breaks when they vanish). Everything is designed for a world where any agent can disappear at any time.

→ [Anti-Patterns](reference/anti-patterns.md) · [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Brownfield Projects (Porting Existing Codebases)

> "That advice is that you first need to transform the big existing codebase into a much shorter specification document that details just the interfaces and the behaviors, but none of the internal implementation details. This lets you compress the important parts down into something that can easily fit within the model's context window, where it can think about everything all at once."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

Most of the methodology assumes greenfield work: plan from scratch, decompose into beads, build. But my projects are often ports or rewrites (Python to Rust, especially). The brownfield strategy is a compression trick: extract the interfaces, the behaviors, and the contracts from the existing code, discarding all implementation details. Feed that compressed specification to the model. Now it has a complete picture of what the system does and how its parts interact, without being anchored to the old implementation.

This is the chef analogy applied to porting. If you give the model the full old codebase, it will "port" each function line-by-line, producing a transliteration. If you give it a specification of behaviors, it designs a new implementation from scratch that satisfies the same contracts but uses the target language's idioms and patterns. Better code, not just translated code.

→ [Phase 0 — Prerequisites](playbook/phase-0-prerequisites.md) · [Phase 1 — Draft](playbook/phase-1-draft.md)

---

## The Human's Role (Machine Tender)

> "Then they just keep cranking on their own for a really long time. And you don't need to supervise them much so you can be juggling multiple projects like this at once and make really great progress on all of them."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619))

> "It took a couple hours, but it was all very asynchronous; basically, occasionally giving the agents more instructions and then turning back to my other work and checking back in on them after 20 minutes or so."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1961996738782269940)) · on converting Kissinger's 400-page thesis

> "I do this every day, multiple times a day, for like 7+ projects now, and keep 3 machines busy constantly (and all my various subscriptions, although I'll have to add even more soon at this rate). [...] Each of these blurbs takes under a second to do with a single button press using my little command palette gizmo."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526))

Once the plan is written and the beads are decomposed, my job changes completely. I'm not coding. I'm not even supervising closely. I'm tending machines: firing pre-written prompts via StreamDeck button presses, checking in on progress every 20 minutes, occasionally giving more instructions, and context-switching between 7+ simultaneous projects across 3 machines. Each prompt takes under a second. The agents do the rest, for hours, sometimes 17+, without further input.

My active work is the planning phases, the 90% of human energy. The execution phases are machine-tending: asynchronous, interleaved across many projects, measured in button presses rather than keystrokes. All my scarce resources go into planning and refinement: attention, creative judgment, taste. Everything else goes to machines that run 24/7.

The Kissinger thesis is the proof-of-concept: a 400-page PDF conversion using a swarm of 20 sub-agents, completed in a couple of hours while I was simultaneously working on other coding projects. Occasionally giving instructions. Checking back in after 20 minutes. That became my most shared tweet because it made the workflow viscerally legible to people who had never seen it before.

→ [Doctrine](reference/doctrine.md) · [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Agent Feedback (Closing the Loop)

> "Based on your experience with UBS (Ultimate Bug Scanner) today in this project, how would you rate UBS across multiple dimensions, from 0 (worst) to 100 (best)? Was it helpful to you? Did it flag a lot of useful things that you would have missed otherwise? Did the issues it flagged have a good signal-to-noise ratio? What did it do well, and what was it bad at? Did you run into any errors or problems while using it? What changes to UBS would make it work even better for you and be more useful in your development workflow? Would you recommend it to fellow coding agents? How strongly, and why or why not? The more specific you can be, and the more dimensions you can score UBS on, the more helpful it will be for me as I improve it and incorporate your feedback to make UBS even better for you in the future!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1991301147702005785)) · **Source of prompt [MT-08 Agent Feedback](prompts/prompt-pack.md)**

This is the flywheel applied to the tools themselves. I build tools for my agents. My agents use those tools all day. They accumulate opinions. So I ask them: was this useful? What was the signal-to-noise ratio? What would make it better for you? Then I take that feedback and ship a better version. The agents use the better version, generate sharper feedback, and the cycle repeats. My tools get better because my agents tell me exactly what to fix.

The scoring dimensions matter. "Was it helpful?" is too vague to act on. "Rate it 0-100 on signal-to-noise, error frequency, workflow integration" gives me numbers I can compare between projects, model types, and tool versions. "Would you recommend it to fellow coding agents?" forces the model to evaluate from a peer's perspective rather than just its own experience, and that shift in framing surfaces different insights.

Swap `<TOOL_NAME>` for whatever your agents just used. Run it at the end of a session, when the agent has fresh context on what worked and what didn't. The responses go straight into the next version of the tool.

→ [Prompt Pack](prompts/prompt-pack.md)

---

## On Conducting the Swarm

> "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinner faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526015587266875))

> "OK, my upgrade to 64 cores was well-timed. I now have a disgusting number of next-gen Codex 4.6 and Codex 5.3 clankers running across 3 machines and 9 projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019571245784973452))

> "Phew, just barely got my 1,000 commits in today. Tough when you're focused on just 2 or 3 projects for the day. I really made those clankers think hard today."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2018946284426584559))

The self-referential loop is where it gets weird. My agents use tools I built. Those tools were built by agents using earlier versions of the same tools. Each generation of the toolchain makes the next generation faster to build. At some point you stop thinking about "writing code" and start thinking about "conducting an orchestra that builds its own instruments."

The 1,000-commit days are not about typing. They're about orchestration bandwidth. How many agents can you keep productively occupied simultaneously? At Formation D (7-10 agents), you hit a rhythm: check agent, approve, assign next bead, check next agent, approve, assign. At Formation F+ (15+ agents across 3 machines), you need the perception loop running unconsciously -- glance, assess, act, move on. The bottleneck is never compute. It's always your attention.

The 64-core upgrade wasn't about raw speed. It was about headroom. More cores means more agents before the machine starts thrashing. More agents means more beads executing in parallel. More parallel beads means faster feedback on whether your plan actually works. The flywheel accelerates because the feedback loop tightens.

→ [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md) · [12 Principles](playbook/principles.md)

---

## On the Flywheel Effect (Libraries All the Way Down)

> "The new Rust version of Agent Mail uses the following other libraries and ports that I've made in the last month or so: FrankenTUI for the console interface, FrankenSQLite, FrankenSearch for message search, FrankenAgentDetection, Asupersync throughout, fastmcp_rust for mcp, sqlmodel_rust for orm, beads_rust for beads integration, toon_rust for token dense payloads. That's what the Flywheel is all about."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026872257172161002))

> "Clankers love working with my libraries because they're designed and built entirely by clankers. Everything is just where they expect it to be and works exactly the way they assume it would. 'FCBC' (for clankers, by clankers)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026327054828880055))

This is the terminal stage of the flywheel. You're no longer building applications. You're building libraries that your agents consume to build the next set of libraries. Each library was designed by agents, for agents. The APIs land where models expect them. The error messages read the way models parse them. The documentation is structured the way context windows digest it.

FCBC flips the usual developer-experience question. Instead of "is this API intuitive for a human?" the question becomes "will a coding agent find the right method on the first try?" When your agents built the library in the first place, the answer is almost always yes. The conventions they established during construction are the same conventions they follow during consumption.

The Franken stack (FrankenTUI, FrankenSQLite, FrankenSearch, and so on) is proof-of-concept. Each component was built by agents using the flywheel method. Each component gets consumed by agents building the next project. The dependency graph is a flywheel diagram: every output feeds into the next input.

→ [Doctrine](reference/doctrine.md) · [Anti-Patterns](reference/anti-patterns.md)

---

## The Source Map

How many of the playbook's 47 prompts trace directly to Jeff's public tweets:

| Prompt | Source Tweet | Likes |
|---|---|---|
| [PL-10 Innovation Boost](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/2025645582782480827) | 1,663 |
| [EX-03 Agent Introduction](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1984344027576033619) | 1,442 |
| [RV-02 Deep Review](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [RV-03 Cross-Agent Review](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [RV-01 Self-Review](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [QA-01 Stripe-Level UI](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [BD-01 Plan to Beads](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [BD-02 QA Beads](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [EX-06 Git Commit](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1999934160442687526) | 720 |
| [RV-04 McCarthy Hunt](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/2025636621157363998) | 170 |
| [EX-01 Execute Beads](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1994526888794951977) | 46 |
| [EX-02 Mail Check](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1994526888794951977) | 46 |
| [EX-04 Post-Compaction](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/2017087633877278974) | 58 |
| [MT-08 Agent Feedback](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1991301147702005785) | — |

14 prompts from 7 tweets. The mega-tweet alone sources 7.

Prompts with no public tweet source include RV-05 Stakes Escalation, PL-03/04/05 Praise Pushes (the philosophy is tweeted; the exact text is not), and MT-04 De-Slopifier. These come from Jeff's AGENTS.md templates and repo documentation.
