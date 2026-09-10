---
icon: lucide/cpu
---

# Model Roster — Position at 2026-09

Which model plays which role in the flywheel. Dated on purpose.

!!! warning "Most perishable page on this site"
    Model assignments rot faster than anything else in this methodology. This table reflects @doodlestein's position **at 2026-09**, mined from his threads of July–September 2026. Check the date before you apply any row. When a row here gets contradicted by a newer position, the old one moves to [Supersede Log](supersede.md) — it is never silently edited.

---

## The role grid at 2026-09

| Role | Model(s) at 2026-09 | Since | Why |
|:-----|:--------------------|:------|:----|
| Swarm logistics orchestration | Small model (5.6 Terra) | 2026-07-13 | Logistics — keeping agents busy and unblocked — does not need frontier intelligence. The smarts live in task allocation by `bv` graph analysis, not in the orchestrator. The "how" stays with the big models. |
| Execution on detailed beads | Big models: Fable / Sol or the frontier equivalents of the moment | 2026-07-13 | The workhorse tier. Opus 5 xhigh ("dogged", fast), or cheap models (GLM-5.3 Flash via OpenRouter) when beads are fully self-contained (2026-07-25, 2026-08-29). |
| Planning / plan review / plan revision | Fable Max (browser) + GPT Pro (web app) | 2026-07-25, canonicalized 2026-08-17 | "No substitute for parameter count" for long-tail knowledge. The `/planning-workflow` skill formalizes the pipeline: local draft → fresh-session revision rounds by GPT Pro → beads only once Pro validates. |
| Adversarial plan audit | Sol Ultra (Codex, Ultra mode) + Grok Heavy as external validation | 2026-07-15/16 | 5h+ to audit a plan, 4h+ to revise it. Accepted wall-clock cost: you only do it once per project. |
| Cross-review (model blending) | Always models from **different architecture families** (Fable + Sol + K3) | 2026-07-25 | "The best results by far come from combining models... always going to work way better than using any model in isolation, or even groups of the same model." Architecture and training-data diversity is what catches what the others missed. |
| Mission-critical work | Frontier models only — **Opus 5 removed from this tier** | 2026-07-27/28 | Opus 5 agents wrecked complex projects; poor "agent epistemology and humility" (detailed critique 2026-08-25). Opus 5 is now confined to implementation on detailed beads and release fleets. |
| Less important work | K3 (via omp), Gemini 3.7/3.8 Flash (via agy) | 2026-08-19 | K3 was relegated after mode-collapse episodes (bizarre franglais, 2026-08-08) — the code stays good, but trust was downgraded. Still useful for architecture diversity in cross-review. |
| Mechanical tasks / bulk PDF conversion | Gemini Flash 3.8 | 2026-09-02/09 | Best cost/quality for commits, perf remediation, PDF conversion at scale. Supersedes the July verdict that called Gemini "extremely bad" — new model versions plus the repeated finding that skill and prompt matter more than the model. |
| Best value for money | Grok 4.6 via `grok build` ($300/mo) | 2026-08-19 | All of his git commits at one point. Not frontier, but the price/performance ratio is unmatched. |

## Rules that survive model turnover

- **Keep frontier models in their native harness.** Sol is co-trained (RL) with Codex; pulling it out of its harness loses the synergy. Never risk Claude Max accounts on third-party harnesses (ban risk) (2026-07-16).
- **Roll out new models prudently.** Deploy on the projects "more tolerant of stupidity" first. Default skepticism toward any model not sold as "best". Beads + git tracking is what lets you repair the damage (2026-07-27).
- **Local models are out for agentic work** ("sorry, but they suck", 2026-08-27) but stay in the stack as specialized tools agents call — OCR/TTS/Whisper via FrankenOCR/FrankenTTS/FrankenWhisper, no GPU (2026-07-06 → 2026-09). A role distinction, not a contradiction.
- **Never downgrade mid-task.** "I never want to switch to the dumber model" (2026-07-22) targets in-task downgrades in Codex — it does not contradict deliberate frontier-plan / cheap-exec tiering (2026-08-29). The two coexist.

## Where this leaves the playbook

The playbook's prompts are model-agnostic by design. Wherever a prompt notes a preferred model, the note is dated and points back here. The [dispatch table](dispatch.md) routes situations to prompts; this page routes roles to models.
