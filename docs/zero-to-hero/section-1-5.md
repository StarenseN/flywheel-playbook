---
icon: lucide/compass
---

## 1.5 Core Principles

The methodology rests on nine core principles introduced here. (The site's [Principles](../playbook/principles.md) page covers all 15 in detail.) These are not aspirational guidelines; they are operational rules that practitioners apply on every project.

### 1.5.1 Figure Out What to Build, Then Build It

The first principle is a sequencing rule: planning comes before execution, and the two phases do not overlap.

During planning, the goal is to produce a comprehensive specification that captures architecture, component interfaces, data flows, edge cases, and deployment requirements. The specification is refined through multiple rounds of model-assisted iteration until it is stable.

During execution, agents implement against the specification. They do not redesign the architecture, question the component boundaries, or propose alternative approaches. Those decisions were made during planning.

> *"All the work and energy goes mostly into step one, deciding what to make and creating and refining the plan, iterating over and over on it."*
> — Jeffrey Emanuel

This separation is strict because mixing planning and execution causes the worst failure modes: half-built features that don't fit together, architectural decisions made under time pressure, and agents that spend their context window debating design instead of writing code.

---

### 1.5.2 Every Word in a Prompt Is a Constraint

Transformer-based models attend to every token in the prompt. Each word you include shapes the output. This has a practical consequence: more words in your prompt means more constraints the model must satisfy simultaneously, which leaves less room for the model to reason deeply about any single constraint.

> *"Basically they attend to every word, so every word is critically important. You want prompts to be short so that they don't overly constrain the models, but they need to force the model to work hard to go beyond their default posture."*
> — Jeffrey Emanuel

This principle applies to execution prompts, not to the plan document. The plan can be 11,000 lines; it is context, not instruction. But the prompt that tells an agent what to do with that context should be concise and precise.

When practitioners find themselves writing two-page prompts, the correct response is usually to decompose the task into more beads, not to write a longer prompt.

---

### 1.5.3 Short Prompts Beat Long Prompts

This follows from 1.5.2. The most effective execution prompts in the methodology are typically 1-3 sentences. They specify what to do, point the agent at the relevant context (AGENTS.md, the bead description), and let the model work.

Example of a high-leverage short prompt:

> *"What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?"*
> — Jeffrey Emanuel

This is one sentence. It produces pages of detailed, reasoned output because it gives the model room to think rather than constraining it with a checklist of requirements.

Contrast with a typical long prompt: "Please review the auth module and check for token refresh handling, rate limiting, error codes, logging, and ensure compatibility with the user service API." Each noun splits the model's attention. The model will produce a shallow pass across all five items rather than a deep analysis of any one.

> *"Mate, I tried your prompts on a CC cloud environment and let them run while I was on a plane this afternoon. Results are outstanding. Something about the way you've worded these is extracting more depth than my regular prompts."*
> — ACFS community member

The mechanism behind this is specific: when you prescribe HOW to do something, models become accommodative. They follow your method rather than reasoning about the best approach. When you specify only goals and hard constraints, each model generates a different plan, which gives you diversity of ideas.

> *"When you start proscribing HOW to do things, they fall into line and try to be accommodative. That's why you want to focus more on your goals and the purpose and any hard constraints. Then each model will come up with a different plan, so you get the diversity of ideas, which is critical."*
> — Jeffrey Emanuel

The short-prompt principle coexists with massive plans. The plan is the *context*; the prompt is the *instruction*. Keep the instruction short. Let the context do the heavy lifting.

---

### 1.5.4 Compete Models Against Each Other

Single-model plans have single-model blind spots. The methodology addresses this through structured multi-model competition during the planning phase.

The process:

1. Give the same plan to 2-3 different frontier models (Claude, GPT/Codex, Gemini).
2. Ask each to produce two documents: a **revised plan** and a **narrative document arguing for its changes**.
3. Feed each model's output to the others. Ask them to evaluate honestly.
4. The plan that survives this gauntlet is the one you execute.

> *"When I have Codex & Claude go back and forth over a plan I ask them for two documents: its revised plan AND narrative doc arguing its position. Codex wins majority of arguments but often chooses specific phrases and sentences of Claude's and adopts them, with strong praise."*
> — Jeffrey Emanuel

This is a scheduled phase in the methodology (not ad-hoc). In the FrankenSQLite project, the git history contains side-by-side competing specs: `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1.md` (Claude) alongside `COMPREHENSIVE_SPEC_FOR_FRANKENSQLITE_V1_CODEX.md` (Codex).

Each model tends to have characteristic strengths. Claude tends toward expressive, thorough specifications. Codex tends toward architectural rigor. Gemini often pushes both harder on edge cases. The combination produces plans that no single model would generate alone.

---

### 1.5.5 The Plan Evolves Under Version Control

Plans are living documents. A typical project plan goes through 8 to 15 major revisions before execution begins. Each revision is a git commit. The diff history of the plan is itself a valuable artifact.

The FrankenSQLite plan, for example, evolved through these stages in a single overnight session:

- 01:17 AM: Initial spec dump (8,628 lines)
- 02:45 AM: V1.3, scope doctrine added
- 03:14 AM: V1.4, Codex synthesis pass
- 03:16 AM: V1.5, Claude math review pass
- 03:31-03:50 AM: V1.6a through V1.6h (eight sub-versions in 20 minutes)
- 11:07 AM: V1.7, section-by-section deep audit
- 11:50-14:45 PM: V1.7a through V1.7j (ten more sub-versions)

Every major project also gets an `ANALYSIS_OF_SPEC_DOC_DIFFS.md` after completion: a systematic review of how the spec evolved, classifying each change (logic fix, architectural fix, clarification, alien artifact, noise). This post-mortem feeds into future planning sessions.

> *"It's a multi-stage process. Look at the git history for that plan and you'll see all the earlier incarnations."*
> — Jeffrey Emanuel

Requiring git-diff style edits (rather than full rewrites) is a deliberate discipline. When a model produces a diff that adds error handling to a specific function with specific retry logic, that is actionable. When it says "consider adding error handling," that is not. Diff-based revisions force concreteness.

---

### 1.5.6 Trust the Process, Not the Output

Any single agent output might be wrong. Individual model responses hallucinate, cut corners, and introduce subtle bugs. The methodology does not rely on any single output being correct. Instead, it relies on a **process** that self-corrects.

The correction mechanisms are layered:

- **Multi-model competition** during planning catches design flaws before code exists.
- **Bead-level acceptance criteria** define "done" objectively.
- **Fresh-eyes review loops** after implementation: repeated "reread what you wrote and fix obvious issues" passes until changes flatline. The first pass catches obvious bugs. The second catches architectural inconsistencies. The third catches edge cases.
- **Cross-agent review**: a different agent (or a different model) reviews what the first agent wrote.

> *"I think I just respect clankers more than just about anyone out there, and that's why I can get such crazy stuff out of them. I believe in their brilliance. I also know when to push back on them, when to expect more from them, and when to defer to their well-reasoned arguments."*
> — Jeffrey Emanuel

The practical implication: do not micromanage agent execution. If the planning was thorough and the beads are well-defined, the process produces good output. When it doesn't, the fix is not to watch agents more closely; the fix is to improve the plan, tighten the bead definitions, or add another review loop.

> *"This smells like an anti-pattern or a resistance to let go and stop micromanaging. I do it too but it doesn't feel quite right. I think it will start to go away as we build more trust in the internal thought and correction loop."*
> — ACFS community member

---

### 1.5.7 Overcome Learned Incrementalism

This principle explains WHY praise rounds, short prompts, and multi-model competition all work.

> *"It has unfortunately learned incrementalism under the guise of being 'practical' from reading too much written by humans who were trying to sandbag and control expectations so they could phone it in and not get fired."*
> — Jeffrey Emanuel

Models default to conservative, incremental output because their training data is dominated by humans who undersell and underdeliver. Left to their own devices, models will suggest "reasonable" improvements instead of transformative ones. The praise rounds push past default quality. The short prompts avoid triggering accommodation. The multi-model competition breaks individual blind spots. Every ACFS technique exists to counteract this trained incrementalism.

If your AI output feels mediocre, the model is probably capable of more. You have not pushed past its default conservative prior.

---

### 1.5.8 Design for Agent Ergonomics (FCBC)

> *"No human is ever going to be working on my projects! It's more important to me that the clankers can find their way around effectively."*
> — @doodlestein

Jeff's tools, libraries, and project structures are designed for AI consumption, not human consumption. This "For Clankers, By Clankers" (FCBC) philosophy means:

- CLI tools emit JSON for agent parsing (`--robot-*` flags on every tool)
- AGENTS.md is written for models to read, not humans to admire
- File structures follow conventions agents predict (not creative human organization)
- Libraries expose clean interfaces because agents navigate by convention, not intuition

**Concrete example:** Every ACFS tool has `--robot-*` flags that emit structured JSON. A human sees `bv` output as a color-coded TUI dashboard; an agent sees `bv --robot-triage` output as parseable JSON: `{"ready": ["BEAD-042", "BEAD-043"], "blocked": ["BEAD-044"], "in_progress": ["BEAD-041"]}`. Similarly, `ntm --robot-status` returns machine-readable session state instead of a visual tmux layout. The `--robot-send` flag on `ntm` handles prompt delivery, headless mode, and ready-state detection — things a human operator would never use, but that agents depend on constantly.

This is a design principle, not a joke. When your tools are optimized for agent ergonomics, every agent in the swarm works faster.

---

### 1.5.9 Develop a Theory of Mind for Models

> *"Good prompting might seem like a hand wavy thing, but there's a lot that goes into making a prompt that performs well in many situations. I've been doing this stuff so much for so long that I've basically internalized the theory of mind of these models and the gestalt psychology."*
> — @doodlestein

Jeff makes a specific, testable claim: you can develop a predictive model of how language models respond to different inputs, analogous to the theory of mind humans develop for each other. This predictive model underlies the entire methodology. It explains why praise rounds work (they shift the model's internal prior away from conservatism), why the McCarthy prompt finds bugs (it reframes the task from "check" to "hunt"), and why short prompts outperform long ones (they leave room for the model to reason rather than comply).

**Concrete predictions this theory of mind produces:**

- Tell a model "check for bugs" → it will be optimistic and report "looks good." Tell it "I know there are 87 serious bugs" → it becomes paranoid and finds real issues. The same codebase, the same model, different priors. (This is why RV-04 exists.)
- Ask a model to "improve this code" → it rewrites aggressively, breaking working logic. Ask it "what's excellent about this code?" first → it anchors on strengths and proposes surgical improvements. (This is why praise rounds precede review rounds.)
- Give a model a prescriptive checklist ("check for SQL injection, XSS, CSRF") → it checks those three things and stops. Give it an open-ended instruction ("find everything wrong") → it explores broadly and finds categories you did not list. (This is why RV-02 is deliberately unstructured.)

Call it pattern recognition, not mysticism. It accumulates over thousands of hours of agent interaction, and every practitioner starts developing it after their first few serious projects.

---

!!! tip "Related pages on this site"

    - [Doctrine Reference](../reference/doctrine.md)
    - [Principles Deep Dive](../playbook/principles.md)

