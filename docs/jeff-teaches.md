---
icon: lucide/quote
---

# Jeff Teaches

Jeffrey Emanuel ([@doodlestein](https://x.com/doodlestein)) invented the agentic coding flywheel. What follows are his words, mined from 1,318 public tweets and 385 threads. Verbatim quotes are exact. Paraphrases are marked `[Adapted]`.

Many of the [46 prompts](prompts/prompt-pack.md) in this playbook come directly from Jeff's tweets. Where a tweet is the source of a playbook prompt, the prompt ID is noted.

---

## On Planning (90% of Your Time)

> "I spend 90% of my human time and energy (which is the only thing that matters) on planning. Once the beads are done it's just machine tending."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021695143187767360))

> "My approach really works. You don't have to be some crazy savant to apply it, either. Just read my posts about planning and use the tools and workflows, and you will also get these kinds of results. I've now seen at least 10 people who I don't even know report similar outcomes."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021641758690681021)) · 283 likes

> "When you think you're finished with your development plan for your agent, try this prompt with a few different frontier models. You might be amazed what they come up with: 'What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025645582782480827)) · 1,663 likes · **Source of prompt [PL-10 Innovation Boost](prompts/prompt-pack.md)**

[Adapted] 90% of your human energy goes into the plan. The plan is the product; code is its compiled form. When you think the plan is finished, push it through multiple frontier models with PL-10. It's never done the first time. The gap between a "good" plan and an "optimal" plan is where all the value lives.

→ [12 Principles](playbook/principles.md) · [Phase 1 — Draft](playbook/phase-1-draft.md) · [Phase 2 — Refine](playbook/phase-2-refine.md)

---

## On the Chef Analogy (Don't Over-Specify)

> "The chef is the model, and you are the annoying party planner. Every time you try to tell the model exactly what to do and how, just understand that, although you might end up with something that on the surface conforms with all your requirements, it will be the equivalent of that 'face-in-hole' photo that 'technically' looks like the person but also looks 2-dimensional and like a bad Photoshop attempt: no artistry, and not likely to fool anyone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899)) · 62 likes

> "The models are now smart enough that, once they understand the high-level goals, they can do a better job planning than you can, at least if the goal is to get a plan that other models/agents are going to implement. [...] When coming up with your plan, don't be too prescriptive, to give the model flexibility so that you get the best possible plan. But once you've figured out what to do, you want to go in the opposite direction and get very detailed and specific so that you can turn the plan into such detailed marching orders that even a dumber agent could still probably implement them well (but of course, you don't use a dumb agent, you use a very smart agent that is super overpowered for the task so that they do a phenomenal job)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "And then I turn those plans into extremely detailed beads (epics, tasks, subtasks, etc) so that the agents that are actually implementing the stuff don't need to understand the big picture and can focus instead on their narrow task, much like a short-order cook in a diner can focus on the ticket in front of them and just make a good pastrami sandwich without worrying about how to bake a pie."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

[Adapted] Two modes, two mindsets. In plan-space (Phases 1-3): stay vague, give the model freedom, let it explore. In task-space (Phases 4-6): be extremely specific, make marching orders so detailed any agent can execute them. Confusing the two — being prescriptive in plan-space or vague in task-space — is how you get mediocre output. The chef analogy is the single most important concept for prompting well.

→ [12 Principles](playbook/principles.md) · [Phase 4 — Beads](playbook/phase-4-beads.md)

---

## On Respecting the Models (Moral Suasion)

> "I think I just respect clankers more than just about anyone out there, and that's why I can get such crazy stuff out of them. I believe in their brilliance. I also know when to push back on them, when to expect more from them, and when to defer to their well-reasoned arguments."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2020228069609599389)) · 48 likes

> "I really admire Opus 4.5's 'personality' and love of learning and research. And that I can persuade it through appeals to reason and the pursuit of knowledge, and it actually works."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2005713671809556925)) · 60 likes

> "I'm tough and demanding on my guys, but also supportive and complimentary. All of this is important during the planning phases."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984082457470030105)) · **Philosophy behind the [Praise Push](playbook/phase-2-refine.md) prompts**

> "These little blurbs [in AGENTS.md] are so critical, and why they should focus on convincing rather than ordering the agents! Moral suasion ain't just for humans..."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984403875714027571))

[Adapted] Praise expands, critique refines. Both are required. The models respond to belief, respect, and stakes. The prompts that assume brilliance produce better results than the prompts that assume incompetence. Even the AGENTS.md blurbs should convince, not command. Moral suasion works better on models than rigid instructions. This is not anthropomorphism; it's empirical — and it's why the praise rounds (PL-03, PL-04, PL-05) exist.

→ [Phase 2 — Refine](playbook/phase-2-refine.md) · [Anti-Patterns](reference/anti-patterns.md)

---

## On the AGENTS.md Constitution

> "My coding agent workflow has really changed a lot ever since I gave them access to messaging so that they can directly communicate with each other. Now, I have one of them come up with a super detailed plan and sometimes have GPT Pro review and improve the plan in the webapp. Then I start up 4 or 5 Codex instances in the same project folder and tell them: 'Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · 1,442 likes · **Source of prompt [EX-03 Agent Introduction](prompts/prompt-pack.md)**

> "I've been trying for a while to find a reliable way to remind my Claude Code agents to re-read AGENTS.md promptly after every compaction. Otherwise they have a tendency to go rogue and act like wild animals (fortunately, they're at least muzzled wild animals since I'm also using my dcg tool)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017087633877278974)) · 58 likes · **Why [EX-04 Post-Compaction Refresh](prompts/prompt-pack.md) exists**

[Adapted] AGENTS.md is the project constitution. Every agent reads it first. After every context compaction, they re-read it. Without it, agents go rogue. Jeff literally runs a hook to force re-reads because the methodology breaks without this anchor. The constitution is hand-written or heavily directed. Never generated.

→ [Phase 0 — Prerequisites](playbook/phase-0-prerequisites.md)

---

## The Mega-Tweet: 7 Prompts in 1 Thread

On December 13, 2025, Jeff published a single tweet containing his complete daily workflow. This one tweet is the source of 7 of the playbook's 46 prompts.

> "I like to make sure that I'm making some forward progress on every one of my active projects each day, even when I'm too busy to spend real mental bandwidth on all of them every single day. So I've come up with a few prompts that I use a lot with the agents so they're always doing some level of polishing/checking/fixing and general improvement."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · 720 likes

The prompts he shares, in the exact sequence he runs them:

**Prompt 1** → [RV-02 Deep Review](prompts/prompt-pack.md):

> "I want you to sort of randomly explore the code files in this project, choosing code files to deeply investigate and understand and trace their functionality and execution flows through the related code files which they import or which they are imported by. Once you understand the purpose of the code in the larger context of the workflows, I want you to do a super careful, methodical, and critical check with 'fresh eyes' to find any obvious bugs, problems, errors, issues, silly mistakes, etc. and then systematically and meticulously and intelligently correct them."

**Prompt 2** → [RV-03 Cross-Agent Review](prompts/prompt-pack.md):

> "Ok can you now turn your attention to reviewing the code written by your fellow agents and checking for any issues, bugs, errors, problems, inefficiencies, security problems, reliability issues, etc. and carefully diagnose their underlying root causes using first-principle analysis and then fix or revise them if necessary? Don't restrict yourself to the latest commits, cast a wider net and go super deep! Use ultrathink."

**Prompt 3** → [QA-01 Stripe-Level UI](prompts/prompt-pack.md):

> "Great, now I want you to super carefully scrutinize every aspect of the application workflow and implementation and look for things that just seem sub-optimal or even wrong/mistaken to you, things that could very obviously be improved from a user-friendliness and intuitiveness standpoint, places where our UI/UX could be improved and polished to be slicker, more visually appealing, and more premium feeling and just ultra high-quality, like Stripe-level apps."

**Prompt 4** → [BD-01 Plan to Beads](prompts/prompt-pack.md):

> "OK so please take ALL of that and elaborate on it more and then create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.-- anything we'd want our 'future self' to know about the goals and intentions and thought process)."

**Prompt 5** → [BD-02 QA Beads](prompts/prompt-pack.md):

> "Check over each bead super carefully-- are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. It's a lot easier and faster to operate in 'plan space' before we start implementing these things!"

**Prompt 6** → [RV-01 Self-Review](prompts/prompt-pack.md):

> "Great, now I want you to carefully read over all of the new code you just wrote and other existing code you just modified with 'fresh eyes' looking super carefully for any obvious bugs, errors, problems, issues, confusion, etc. Carefully fix anything you uncover."

**Prompt 7** → [EX-06 Git Commit](prompts/prompt-pack.md):

> "Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink."

Jeff notes that he enters these as a queue: "each of these blurbs takes under a second to do with a single button press using my little command palette gizmo." He runs this sequence daily, across 7+ projects, on 3 machines.

→ [Prompt Pack](prompts/prompt-pack.md) · [Phase 7 — Review](playbook/phase-7-review.md)

---

## On Fresh-Eyes Review (the Gestalt Switch)

> "Fresh eyes is a massive unlock. This is the stuff that even the labs don't fully understand. It's all based on theory of mind of the models and gestalt psychology concepts."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022356686774899051)) · 90 likes

> "This is why things like my 'fresh eyes' code review prompt can be so shockingly effective if you're not used to that sort of thing: it's because they're tapping into some very deep thing in the model's brain that changes the way it operates, like toggling a create/critique mode gestalt switch."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

> "Here's my recipe. I spin up around 5 Gemini-cli instances in the same project and start them all out with this prompt: 'First read ALL of the AGENTS.md file and README.md file super carefully...' [...] Then I kill them and start new ones with the same recipe. Keep doing that round after round until they come up clean!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025057469945286763)) · 205 likes

[Adapted] Fresh-eyes review is not a single pass. It is a numbered series: round after round until they come up clean. Each new instance starts with no memory of the prior instance's assumptions. This toggles what Jeff calls the "gestalt switch" — a deep mode change in how the model processes code. The models do not get bored. Use that. Kill the agent. Start a new one. Same project, clean eyes.

→ [Phase 7 — Review](playbook/phase-7-review.md)

---

## On the McCarthy Prompt (Stakes Escalation)

> "This prompt is somewhat cruel to the clanker, but man does it work. It sort of reminds me of Joe McCarthy waving his list of communists working in the government but never showing it to anyone. If the agent believes there are bugs to find and fix, it will keep working until it finds them: 'I know for a fact that there are at least 87 serious bugs throughout this project impacting every facet of its operation. The question is whether you can find and diagnose and fix all of them autonomously. I believe in you.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025636621157363998)) · 170 likes · **Source of prompt [RV-04 McCarthy Hunt](prompts/prompt-pack.md)**

[Adapted] Two forces at work: conviction ("I know for a fact") and belief ("I believe in you"). The model responds to both. If it believes bugs exist, it keeps digging. If you add personal stakes, it digs harder. This is why RV-04 and RV-05 exist as separate escalation levels in the playbook — each one ratchets the model's intensity.

→ [Phase 7 — Review](playbook/phase-7-review.md) · [Prompt Pack](prompts/prompt-pack.md)

---

## On the Flywheel

> "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinning faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools. Like this weird prompt I just used, telling Opus 4.5 to use my beads analysis tool to figure out what all its robot friends should most advantageously apply themselves to using graph theory on my hundreds of open tasks and subtasks in beads."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526015587266875)) · 908 likes

> "The core of my approach is agent mail plus beads_rust plus beads_viewer. You can have Claude set them all up for you and show you how to use them together."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021651389773443258))

[Adapted] The flywheel is not a metaphor. It is a literal compounding cycle: each project builds tools that the next project's agents use to build better tools. After enough rotations, the agents are driving your own tools on your own tasks without you touching the keyboard. That is the flywheel spinning.

→ [12 Principles](playbook/principles.md)

---

## On Agent Swarms (Mix Models, Not Roles)

> "If you think this is cool, trust me: the concept works even better when the swarms contain a mixture of frontier models and agent harnesses. Opus and GPT 5.2 are way smarter together than alone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2019476380904210820)) · 229 likes

> "Three Opus 4.5 agents with Claude Code, three gpt-5-codex-max agents in Codex, and three Gemini 3 agents in gemini-cli, all communicating with each other via my mcp agent mail system."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977)) · 46 likes

> "Man, new model day is so exhilarating and fun. I feel like a kid in a candy store with 15+ different Sonnet 4.5 Claude Code instances running around on my machine at once across 6 different repos, all studying, planning, fixing, improving, polishing, and harmonizing my projects."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1972766157641003091)) · 37 likes

> "I've seen a lot of skepticism recently from people about the claims around how long agents like Claude Code w/ Opus 4.6 can go autonomously without some kind of automated loop feeding them new instructions. People think it's BS because they haven't observed it. It's real. 17+hrs."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2027515287235342804)) · 153 likes

> "I do have 4 Claude max accounts, 4 GPT pro accounts, 3 Gemini ultra accounts, etc."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994536534016856095))

[Adapted] Agents are fungible generalists. No roles, no backstories. But diversity of models matters: Opus + GPT + Gemini together find what no single model finds alone. Three of each, nine total, all talking through agent mail. And they don't stop — 17+ hours of autonomous execution on a single AGENTS.md constitution and a set of beads. 15+ instances across 6 repos simultaneously. This is not theory; this is the daily workflow.

→ [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Alien Artifacts

> "Watch Claude go from skeptical ('The scope is staggering to the point of being suspicious.') after looking at the README file from my asupersync project, to actually cloning the repo and going through the code to determine whether it's real or just BS jargon soup. By the end, it can hardly believe what it has seen ('It's real. And it's absurd.'). [...] I want to stress that I am not remotely smart enough to have made this. Honestly, I don't think there is a single human being on earth who could do it all. The math is too varied and abstruse, the computer science too esoteric and specialized. The people who know structured concurrency that well wouldn't work fluently with martingale concentration bounds, Mazurkiewicz trace theory and Foata normal forms, tropical semiring budget algebra, or RaptorQ fountain coding. [...] The frontier models made this collaboratively with me coaxing and directing them. This is a new breed of software. This is an Alien Artifact."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026614142598033732)) · 346 likes

[Adapted] Alien Artifacts are software constructs that no single human could build. The math is too varied, the CS too esoteric. But frontier models working collaboratively, with a human coaxing and directing, can produce things that look impossible in hindsight. Phase 3 is a named, scheduled discipline: you send the plan to a model strong at formal reasoning and tell it to push beyond standard engineering. It is the most ambitious phase of the flywheel.

→ [Phase 3 — Alien Artifacts](playbook/phase-3-alien-artifacts.md)

---

## On the Commit Agent (the Salon Analogy)

> "It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair from their current client during or at the end of the session. But in practice it's much better to have a dedicated cleaner who focuses only on that task and continuously does it while the stylists work and focus on what they're good at: cutting and styling hair. You want your agents focusing on the coding itself and executing beads tasks. If you try to make them deal with commits, they will do a poor job with short, unhelpful commit messages, and will accidentally include commits from other agents in the process."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022377209848328610)) · 74 likes

The verbatim prompt for the commit agent:

> "Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2000012327555309702)) · **Source of prompt [EX-06 Git Commit](prompts/prompt-pack.md)**

> "Some people try to get their 10,000 steps in every day. Apparently this was just some arbitrary made up thing by a random Japanese pedometer company. But me? I strive to get my 1,000 commits in every day!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017417299733385606)) · 64 likes

[Adapted] The commit agent is the only exception to "agents are fungible generalists." It has one job: read what changed, group changes logically, write detailed commit messages, push. It never touches code. One dedicated committer handles dozens of repos at once. Coding agents code; the commit agent commits.

→ [Doctrine](reference/doctrine.md)

---

## On Communication Purgatory

> "Don't get stuck in 'communication purgatory' where nothing is getting done; be proactive about starting tasks that need to be done, but inform your fellow agents via messages when you do so and mark beads appropriately."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977)) · **Language used in prompts [EX-01](prompts/prompt-pack.md) and [EX-02](prompts/prompt-pack.md)**

> "Friends don't let friends use git worktrees for agent swarms for high-velocity development. I give the clankers hell if I ever catch them using worktrees. They're just kicking the can down the road and sticking your heads in the sand instead of surfacing the conflicts early."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022355083845894159)) · 80 likes

[Adapted] Ship code, don't ask permission. Inform, then execute. Worktrees are a trap: they hide conflicts instead of surfacing them early. Messaging without shipping is communication purgatory. The antidote is action. Mark beads as you go.

→ [Anti-Patterns](reference/anti-patterns.md) · [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On the Daily Workflow

> "I do this every day, multiple times a day, for like 7+ projects now, and keep 3 machines busy constantly (and all my various subscriptions, although I'll have to add even more soon at this rate). [...] Each of these blurbs takes under a second to do with a single button press using my little command palette gizmo."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · 720 likes

> "Then they just keep cranking on their own for a really long time. And you don't need to supervise them much so you can be juggling multiple projects like this at once and make really great progress on all of them."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · 1,442 likes

[Adapted] Marathon sessions beat distributed sessions. Context is perishable. Seven projects, three machines, every day. Each prompt is one button press. The agents keep cranking for hours unsupervised because the AGENTS.md is solid, the beads are detailed, and the methodology is self-reinforcing. This is what it looks like when the flywheel is spinning.

→ [Doctrine](reference/doctrine.md)

---

## The Source Map

How many of the playbook's 46 prompts trace directly to Jeff's public tweets:

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
| [EX-04 Post-Compaction](prompts/prompt-pack.md) | [source](https://x.com/doodlestein/status/1994526015587266875) | 908 |

13 prompts from 5 tweets. The 720-like mega-tweet alone sources 7.

Prompts with no public tweet source include RV-05 Stakes Escalation, PL-03/04/05 Praise Pushes (the philosophy is tweeted; the exact text is not), and MT-04 De-Slopifier. These come from Jeff's AGENTS.md templates and repo documentation.
