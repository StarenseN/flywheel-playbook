---
icon: lucide/route
---

# Prompt Dispatch Table

When you're in situation X, use prompt Y.

---

| Situation | Prompt | Phase |
|:----------|:-------|:------|
| Starting from scratch | P01 (First Principles) | 1 |
| First draft of plan exists | P30-P32 (Praise Rounds) | 2 |
| Plan needs analytical critique | P04 (Plan Critique) | 2 |
| Multiple models produced competing plans | P15 (Multi-Model Synthesis) | 2 |
| Two models should compete on a problem | P17 (Dueling Wizards) | 2 |
| Need a single transformative addition | P26 (Plan Innovation Boost) | 2 |
| Need honest project assessment | Project Opinion Elicitor | 2 |
| Integrating critique feedback | Integrate Critique prompt | 2 |
| Ready to stress-test plan against failure | Premortem Planner | 2 |
| Converting plan to beads | P05 (Plan to Beads) | 4 |
| Reviewing bead quality | P06 (QA the Beads) | 5 |
| Picking next bead to work on | P24 (Use BV) | 6 |
| Spawning a fresh agent | P18 (Agent Introduction) | 6 |
| Ongoing bead execution | P08 (Execute Beads) | 6 |
| Agent mail check + continue | P09 (Anti-deadlock) | 6 |
| After context compaction | P20 (Refresh) | 6 |
| Full autonomous push | P25 (Do All Of It) | 6 |
| Committing code (commit agent only) | P11 (Git Commit) | 6 |
| After completing a bead | P02b (Self-Review) | 7 |
| Numbered review session | P02 (Deep Review) | 7 |
| Reviewing other agents' code | P03 (Cross-Agent Review) | 7 |
| Reviews seem too comfortable | P27 (McCarthy Bug Hunt) | 7 |
| Need the model to care more | P28 (Stakes Escalation) | 7 |
| Security-focused review | P29 (CVE Testing) | 7 |
| Hunting stubs and placeholders | Stub Eliminator | 7 |
| UI/UX polish | P12 (Stripe-Level UI) | 7 |
| Comprehensive testing | P13 (E2E Pipeline Validator) | 7 |
| UX audit | P19 (Audit UX) | 7 |
| Random codebase exploration | P23 (Random Inspect) | 7 |
| Root-cause bug fixing | P22 (Fix Bug) | 7 |
| Full codebase scan | P21 (UBS Scan) | 7 |
| Post-deploy verification | Deployment Verifier | 7 |
| Performance optimization | Deep Performance Audit | 8 |
| Generating improvement ideas | P16 (Idea Wizard 30 to 5) | Any |
| Need exceptional ideas only | 100-to-10 Filter | Any |
| Finding system weaknesses | P14 (System Weaknesses) | Any |
| Onboarding to unfamiliar project | P10 (Deep Project Primer) | Any |
| Building agent-friendly CLIs | CLI Error Tolerance | Any |
| File/folder restructuring | Code Reorganizer | Any |
| Updating documentation | README Reviser | Any |
| Cleaning AI-generated text | De-Slopifier | Any |
| Pre-integration dependency study | Dependency Analysis | Any |
