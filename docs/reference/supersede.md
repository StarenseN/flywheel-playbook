---
icon: lucide/history
---

# Supersede Log

How this playbook handles its own obsolescence — and the full history of positions that were replaced.

---

!!! note "Supersede policy — in force since the 2026-09-10 pass"
    Model- and tool-related practices evolve fast. **Everything in this playbook is dated. When two passages contradict each other, the more recent position wins** and becomes the reference position of the body of the site; the older position is not deleted but moved here, with its date, the replacement date, and the reason for the change when it is discernible. Timeless principles are kept unless they explicitly contradict a newer position. Dated quotes (e.g. in [Jeff Teaches](../jeff-teaches.md)) are historical artifacts: they do not necessarily represent the current position. **Every future update of this site must follow this rule**: a new contradicting fact requires updating the body AND logging the old position here.

Nothing below is a current recommendation. This is the trace of settled evolutions, kept so you can tell history from doctrine.

## Superseded positions

| # | Topic | Old position (date) | Current position (date) | Reason |
|:--|:------|:--------------------|:------------------------|:-------|
| 1 | Swarm orchestrator | Big model orchestrates (Sol orchestrates, Terra executes — before 2026-07-13) | Small model (5.6 Terra) orchestrates, big models execute, task allocation by `bv` graph analysis (2026-07-13) | Field experience: logistics (keeping agents busy/unblocked) doesn't need frontier intelligence; the "how" stays with the big models. |
| 2 | Refutation partner in cross-review | Opus 4.8 — automatically deferred to GPT opinions | Fable, which refutes Sol without deference ("now I am the master") (2026-07-09) | New model release better suited to the role. |
| 3 | Beads original (Steve Yegge) | Exploring beads as-is, with invasive git hooks (2026-07-09) | beads_rust, in-house Rust port without git hooks, `issues.jsonl` committed in the repo (2026-08-21/27) | In-house rewrite: control, memory-safe Rust. |
| 4 | MCP Agent Mail Python | Original Python, "battle-tested" (since Oct 2025) | mcp_agent_mail_rust, Rust rewrite (2026-07-18) | Memory-safe/performance rewrite, FrankenSuite coherence. |
| 5 | CASS search index: Tantivy | Tantivy | Quill (FrankenSearch), in-house lexical engine (decided 2026-07-17; confirmed 2026-08-19/09-05) | −41 dependency crates, refusal of the "generality tax", Apple Silicon / AMD many-core specific optimizations. |
| 6 | Go/Charm TUI stack (ntm, beads_viewer in Go) | Go TUIs | FrankenTUI (Rust, wasm-compilable) (2026-08-26/09-06) | "Garbage collection, the bane of Go"; FrankenTUI reached maturity. |
| 7 | Terminals by role | Ghostty (local Mac), Rio (maintenance), FrankenTerm (elsewhere) (2026-07-13) | FrankenTerm as the daily terminal (2026-07-29) | FrankenTerm maturity: survives crashes, holds dozens of multi-day sessions where tmux/zellij/WezTerm fail. |
| 8 | Stream Deck "Proceed" button | Single "Proceed" key (2026-07-20) | "keep cranking away" (2026-08-09) → random rotation of ~20 encouragement messages (2026-08-12) | Measured wording effectiveness; rotation avoids repetition the agents perceive as insincere. |
| 9 | Swarm supervision from iPhone via Claude Code's built-in remote control | Remote-controlling agents from the phone (2026-07-10) | Refusal to remote-control agents from the phone; mobile app used, but not to drive agents (2026-07-18) | Not stated in the notes (prudence / field experience likely). |
| 10 | Gemini Flash "extremely bad" | "Fable and Sol make Gemini look mentally disabled in comparison"; Gemini 3.6 Flash/Antigravity "worse than the 4th best Chinese model" (2026-07-23) | Gemini 3.7 Flash (nearly free) sufficient for frontend, ThreeJS, even 5,000x optimizations with the right skills (2026-08-26/29) → Gemini 3.8 Flash for mechanical tasks and bulk PDF conversion, best cost/quality (2026-09-02/09) | New model versions + repeated finding that skill/prompt makes the difference more than the model. |
| 11 | Kimi K3 as front-line reviewer | Blending/cross-review partner (FrankenGraphDB plan review, 2026-07-16; "Sol Max > Kimi K3 > the rest", 2026-07-29) | Relegated to "less important" work via omp (2026-08-19) | Mode-collapse episodes observed (bizarre franglais, 2026-08-08) — code stays good, but trust is downgraded; still useful for architecture diversity. |
| 12 | Opus 5 "spectacular" | Initial enthusiasm, "xhigh across the board" (2026-07-24/25) | Trust withdrawn for mission-critical work: prudent rollout, confined to implementation on detailed beads and release fleets (2026-07-27/28; detailed epistemological critique 2026-08-25) | Opus 5 agents wrecked complex projects; poor "agent epistemology and humility". |
| 13 | casr, dedicated cross-harness resume tool | Standalone tool (2026-07-19) | Function absorbed by cass's canonical format, which resumes a Codex session in Claude Code and vice versa (2026-08-03) | Tooling consolidation. |
| 14 | Free stealth model "ox Alpha" via OpenRouter | Free-token recipe, 36B tokens in one week (2026-08-21) | Offer ended (2026-08-26, "nooooooooo my sweet free tokens… it was a good run") | Provider's free period ended. Historical recipe, not replicable. |

## Apparent contradictions examined and harmonized WITHOUT supersede

Not every tension is an evolution. These five were checked and reconciled:

- **"I never want to switch to the dumber model"** (2026-07-22) vs frontier-plan / cheap-exec tiering (2026-08-29): the rule targets mid-task downgrades in Codex, not deliberate per-role tiering. Both coexist.
- **Local models**: rejected for agentic work ("sorry, but they suck", 2026-08-27) but kept and actively developed as specialized tools called by agents (OCR/TTS/Whisper, 2026-07-06 → 2026-09). A stable role distinction.
- **Terminal**: "the terminal is NOT the right interface for coding with agents" (2026-08-06, for the human) vs "terminal based interfaces work well" (2026-08-16, when the user is an agent). Harmonized by audience.
- **Subagents**: "strictly worse and less capable" as swarm labor (2026-08-11), but kept for intra-harness adversarial review rounds (2026-07-11/15; up to 96 subagents in Codex config, 2026-09-04). The ban targets swarm labor, not review.
- **Worktrees**: constant refusal without oscillation (2025-12-31 → 2026-07-28 → 2026-08-24). The only change is a cleanup skill (`git-worktree-branch-rationalization`) because agents create them despite the ban. No supersede.
