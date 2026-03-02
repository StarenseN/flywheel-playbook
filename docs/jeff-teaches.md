---
icon: lucide/quote
---

# Jeff Teaches

Jeffrey Emanuel ([@doodlestein](https://x.com/doodlestein)) invented the agentic coding flywheel. What follows are his words, mined from 1,318 public tweets and 385 threads since January 2025. Verbatim quotes are exact. Paraphrases are marked.

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
> — @doodlestein ([source](https://x.com/doodlestein/status/2025645582782480827)) · 1,663 likes

[Adapted from @doodlestein] Planning is not preparation for work. Planning IS the work. 90% of your human energy goes into the plan. Once it's solid and broken into beads, execution is machine tending. When you think the plan is done, push it through multiple frontier models. It's never done the first time.

→ [12 Principles](playbook/principles.md) · [Phase 1 — Draft](playbook/phase-1-draft.md) · [Phase 2 — Refine](playbook/phase-2-refine.md)

---

## On the Chef Analogy (Don't Over-Specify)

> "The chef is the model, and you are the annoying party planner. Every time you try to tell the model exactly what to do and how, just understand that, although you might end up with something that on the surface conforms with all your requirements, it will be the equivalent of that 'face-in-hole' photo that 'technically' looks like the person but also looks 2-dimensional and like a bad Photoshop attempt: no artistry, and not likely to fool anyone."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899)) · 62 likes

> "The models are now smart enough that, once they understand the high-level goals, they can do a better job planning than you can, at least if the goal is to get a plan that other models/agents are going to implement. [...] When coming up with your plan, don't be too prescriptive, to give the model flexibility so that you get the best possible plan. But once you've figured out what to do, you want to go in the opposite direction and get very detailed and specific so that you can turn the plan into such detailed marching orders that even a dumber agent could still probably implement them well."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021791255227768899))

[Adapted from @doodlestein] Two modes. In plan-space: stay vague, let the model explore. In task-space: be extremely specific. This is why the flywheel has distinct phases. Phases 1-3 give the model room; phases 4-5 compress everything into precise marching orders. Confusing the two is how you get mediocre output.

→ [12 Principles](playbook/principles.md) · [Phase 4 — Beads](playbook/phase-4-beads.md)

---

## On the AGENTS.md Constitution

> "My coding agent workflow has really changed a lot ever since I gave them access to messaging so that they can directly communicate with each other. Now, I have one of them come up with a super detailed plan and sometimes have GPT Pro review and improve the plan in the webapp. Then I start up 4 or 5 Codex instances in the same project folder and tell them: 'Before doing anything else, read ALL of AGENTS dot md and register with agent mail and introduce yourself to the other agents. Then coordinate on the remaining tasks.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984344027576033619)) · 1,442 likes

> "I've been trying for a while to find a reliable way to remind my Claude Code agents to re-read AGENTS.md promptly after every compaction. Otherwise they have a tendency to go rogue and act like wild animals (fortunately, they're at least muzzled wild animals since I'm also using my dcg tool)."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017087633877278974)) · 58 likes

> "These little blurbs are so critical, and why they should focus on convincing rather than ordering the agents! Moral suasion ain't just for humans..."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984403875714027571))

[Adapted from @doodlestein] AGENTS.md is the project constitution. Every agent reads it first. After every context compaction, they re-read it. The blurbs inside should convince, not command. Moral suasion works better on models than rigid instructions. Without AGENTS.md, agents go rogue. With it, they self-organize.

→ [Phase 0 — Prerequisites](playbook/phase-0-prerequisites.md)

---

## On Respecting the Models

> "I think I just respect clankers more than just about anyone out there, and that's why I can get such crazy stuff out of them. I believe in their brilliance. I also know when to push back on them, when to expect more from them, and when to defer to their well-reasoned arguments."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2020228069609599389)) · 48 likes

> "I really admire Opus 4.5's 'personality' and love of learning and research. And that I can persuade it through appeals to reason and the pursuit of knowledge, and it actually works."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2005713671809556925)) · 60 likes

> "I'm tough and demanding on my guys, but also supportive and complimentary. All of this is important during the planning phases."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1984082457470030105))

[Adapted from @doodlestein] The models respond to respect, to belief, to stakes. Praise expands their output. Critique refines it. Both are required. This is not anthropomorphism; it's empirical. The prompts that assume brilliance produce better results than the prompts that assume incompetence.

→ [Phase 2 — Refine](playbook/phase-2-refine.md) · [Anti-Patterns](reference/anti-patterns.md)

---

## On the Flywheel

> "My agentic coding workflow has gotten so meta and self-referential lately. I can feel the flywheel spinning faster and faster now as my level of interaction/prompting is increasingly directed at driving my own tools. Like this weird prompt I just used, telling Opus 4.5 to use my beads analysis tool to figure out what all its robot friends should most advantageously apply themselves to using graph theory on my hundreds of open tasks and subtasks in beads."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526015587266875)) · 908 likes

> "The core of my approach is agent mail plus beads_rust plus beads_viewer. You can have Claude set them all up for you and show you how to use them together."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2021651389773443258))

[Adapted from @doodlestein] Each project builds infrastructure for the next. The tools you make become the tools your agents use to build the next tools. Libraries compound, methodology compounds, and the flywheel spins faster with every rotation. After enough rotations, the agents are driving your own tools on your own tasks. That is the flywheel.

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

[Adapted from @doodlestein] Agents are fungible generalists. No roles, no backstories. But diversity of models matters: Opus, GPT, Gemini together find what no single model finds alone. Three of each, nine total, all talking through agent mail. And they don't stop. 17+ hours of autonomous execution is real.

→ [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On Fresh-Eyes Review

> "Fresh eyes is a massive unlock. This is the stuff that even the labs don't fully understand. It's all based on theory of mind of the models and gestalt psychology concepts."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022356686774899051)) · 90 likes

> "I like to make sure that I'm making some forward progress on every one of my active projects each day, even when I'm too busy to spend real mental bandwidth on all of them every single day. So I've come up with a few prompts that I use a lot with the agents so they're always doing some level of polishing/checking/fixing and general improvement."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · 720 likes

> "Here's my recipe. I spin up around 5 Gemini-cli instances in the same project and start them all out with this prompt: 'First read ALL of the AGENTS.md file and README.md file super carefully...' Then I kill them and start new ones with the same recipe. Keep doing that round after round until they come up clean!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025057469945286763)) · 205 likes

[Adapted from @doodlestein] Fresh-eyes review is not a single pass. It is a numbered series: Part 1, 2, 3... Part 23. Round after round until they come up clean. Each pass catches what the previous missed because the model starts with no memory of its own prior assumptions. The gestalt switch is real. The models do not get bored. Use that.

→ [Phase 7 — Review](playbook/phase-7-review.md)

---

## On the McCarthy Prompt

> "This prompt is somewhat cruel to the clanker, but man does it work. It sort of reminds me of Joe McCarthy waving his list of communists working in the government but never showing it to anyone. If the agent believes there are bugs to find and fix, it will keep working until it finds them: 'I know for a fact that there are at least 87 serious bugs throughout this project impacting every facet of its operation. The question is whether you can find and diagnose and fix all of them autonomously. I believe in you.'"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2025636621157363998)) · 170 likes

[Adapted from @doodlestein] Stakes matter. If the model believes there are bugs to find, it will find bugs. The McCarthy prompt works because it combines two forces: conviction ("I know for a fact") and belief ("I believe in you"). The model responds to both. This is why RV-04 and RV-05 exist as separate escalation levels.

→ [Phase 7 — Review](playbook/phase-7-review.md) · [Prompt Pack](prompts/prompt-pack.md)

---

## On Alien Artifacts

> "Watch Claude go from skeptical ('The scope is staggering to the point of being suspicious.') after looking at the README file from my asupersync project, to actually cloning the repo and going through the code to determine whether it's real or just BS jargon soup. By the end, it can hardly believe what it has seen ('It's real. And it's absurd.'). [...] I want to stress that I am not remotely smart enough to have made this. Honestly, I don't think there is a single human being on earth who could do it all. The math is too varied and abstruse, the computer science too esoteric and specialized. [...] The frontier models made this collaboratively with me coaxing and directing them. This is a new breed of software. This is an Alien Artifact."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2026614142598033732)) · 346 likes

[Adapted from @doodlestein] Alien Artifacts are software constructs that no single human could build. Martingale concentration bounds, Foata normal forms, tropical semiring algebra, RaptorQ fountain coding, all woven into a single coherent system. Phase 3 is a named, scheduled discipline. You send the plan to a model strong at formal reasoning and tell it to push beyond standard engineering. Not an afterthought.

→ [Phase 3 — Alien Artifacts](playbook/phase-3-alien-artifacts.md)

---

## On the Commit Agent (the Salon Analogy)

> "It's like a busy high-end salon in NYC. Sure, you could insist that each hairstylist clean up the hair from their current client during or at the end of the session. But in practice it's much better to have a dedicated cleaner who focuses only on that task and continuously does it while the stylists work and focus on what they're good at: cutting and styling hair. You want your agents focusing on the coding itself and executing beads tasks. If you try to make them deal with commits, they will do a poor job with short, unhelpful commit messages."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022377209848328610)) · 74 likes

The verbatim commit prompt:

> "Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2000012327555309702))

[Adapted from @doodlestein] The commit agent is separate from the coding agents. It never touches code. Coding agents code; the commit agent commits. Separation of concerns. One dedicated committer can handle dozens of repos at once.

→ [Doctrine](reference/doctrine.md)

---

## On Communication Purgatory

> "Don't get stuck in 'communication purgatory' where nothing is getting done; be proactive about starting tasks that need to be done, but inform your fellow agents via messages when you do so and mark beads appropriately."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1994526888794951977))

> "Friends don't let friends use git worktrees for agent swarms for high-velocity development. I give the clankers hell if I ever catch them using worktrees. They're just kicking the can down the road and sticking your heads in the sand instead of surfacing the conflicts early."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2022355083845894159)) · 80 likes

[Adapted from @doodlestein] Ship code, don't ask permission. Inform, then execute. Worktrees are a trap: they hide conflicts instead of surfacing them. Messaging without shipping is communication purgatory. The antidote is action.

→ [Anti-Patterns](reference/anti-patterns.md) · [Phase 6 — Swarm Execute](playbook/phase-6-swarm.md)

---

## On the Daily Workflow

> "I do this every day, multiple times a day, for like 7+ projects now, and keep 3 machines busy constantly (and all my various subscriptions, although I'll have to add even more soon at this rate). [...] Each of these blurbs takes under a second to do with a single button press using my little command palette gizmo."
>
> — @doodlestein ([source](https://x.com/doodlestein/status/1999934160442687526)) · 720 likes

> "Some people try to get their 10,000 steps in every day. Apparently this was just some arbitrary made up thing by a random Japanese pedometer company. But me? I strive to get my 1,000 commits in every day!"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/2017417299733385606)) · 64 likes

[Adapted from @doodlestein] Marathon sessions beat distributed sessions. Context is perishable. Multiple projects, multiple agents, all day, every day. The methodology scales because the agents are fungible, the plans are self-contained, and the prompts are one button press.

→ [Doctrine](reference/doctrine.md)

---

## What Jeff Doesn't Say on Twitter

Some playbook concepts come from Jeff's GitHub repos and methodology docs, not his tweets:

- **"Praise Push"** — The exact term is absent from Twitter. The technique (iterative praise rounds that expand the plan) appears in his AGENTS.md templates and repo documentation. On Twitter, Jeff describes the same dynamic as being "tough and demanding but also supportive and complimentary."
- **"Premortem"** — Not in his Twitter vocabulary by name. The failure-imagination technique is built into his planning workflow.
- **"Spec Evolution Analysis"** — Phase 9 methodology appears in his docs but has no Twitter coverage.

These concepts are real parts of the methodology. They live in different media.
