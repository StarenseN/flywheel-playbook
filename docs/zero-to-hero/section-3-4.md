---
icon: lucide/trending-up
---

## 3.4 Scaling Patterns

### 3.4.1 Formation Taxonomy

The number of agents you run is not a dial you turn to "fast." It is a formation decision that depends on project size, bead graph shape, budget, and hardware. Jeff uses a six-tier formation taxonomy:

| Formation | Agents | Typical Use | Model Mix (example) |
|-----------|--------|-------------|---------------------|
| **A** | 1 | Single-bead work, prototyping, learning the methodology | 1 CC (Opus) |
| **B** | 2--3 | Small projects, 30-50 beads | 2 CC + 1 Codex |
| **C** | 4--6 | Standard projects, 50-100 beads (Jeff's typical) | 3 CC + 2 Codex + 1 Gemini |
| **D** | 7--10 | Heavy projects, 100-200 beads | 5 CC + 3 Codex + 1 Gemini + 1 commit |
| **E** | 11--15 | Major systems (frankensqlite scale) | 6 CC + 4 Codex + 2 Gemini + 1 commit + 1-2 review |
| **F+** | 15+ | Full-scale swarms (frankenglibc) | 8+ CC + 8+ Codex + Gemini, across 3 machines |

Source: ACFS methodology documentation (FLYWHEEL.md).

The commit agent is always a singleton. Review agents scale with the coder count: 1 reviewer per 4-5 coders. The "reality checker" role (an agent that validates against real data rather than tests) is optional, needed only for data-intensive projects.

> *"I have 16 GPT Pro accounts now. But they're both great and highly complementary. Claude has more of a 'can do' attitude. I also use Gemini for code review. More brains is better than one."*
> -- Jeffrey Emanuel

By February 2026, Jeff's operational formation was 3 Opus agents (Claude Code) + 3 Codex agents + 3 Gemini agents per project, sometimes running across 3 machines simultaneously:

> *"8 Codex + 8 Claude Code per project across 3 machines = ~50+ agents"*
> -- FLYWHEEL.md formation scale table (reconstructed from tweets)

### 3.4.2 Model Mix Ratios

Jeff's observed model allocation as of late February 2026, per his own statements:

| Model | Share of Work | Strengths |
|-------|--------------|-----------|
| **Codex** | ~50-60% | Follows instructions precisely, thorough, catches things Claude misses |
| **Claude (Opus)** | ~10-30% | "Can do" attitude, strong architecture reasoning, only model that reliably handles git hooks |
| **Gemini** | ~20% | Review duties, fresh-eyes perspective that makes both Codex and Claude look incomplete |

> *"Gemini earns 20% of mix right now, for my workload. Claude between 10-30% depending on what I am doing. Codex the rest."*
> -- Jeffrey Emanuel

> *"I thought codex was a thorough, judicious, genius engineer, and claude was competent but a little behind. After bringing gemini into the fold, codex looks lazy and claude seems totally inept."*
> -- Jeffrey Emanuel

**Behavioral differences:** Claude self-corrects iteratively (fix, test, fix, test in rapid cycles). Codex front-loads its thinking (long pause, then dumps a large coherent change). During multi-model competition (PL-07), Jeff asks each model for two documents: a revised plan AND an argumentative narrative against the other's approach. This "two-document debate protocol" surfaces disagreements that a simple review would miss.

**Model-role matrix (operational rules):**

| Task | Recommended Models | Avoid |
|------|-------------------|-------|
| Coding (EX-01) | Codex, Claude | Gemini (shell restriction issues) |
| Review (RV-02) | Gemini, Claude | -- |
| Commit (EX-06) | Claude ONLY | Codex, Gemini (git issues) |
| QA / Reality Check | Codex, Gemini | -- |
| Alien artifacts (PL-13) | Gemini, o3 | — | Formal reasoning strength |

Source: ACFS methodology documentation (ACFS_WIZARD.md).

### 3.4.3 Cost Modeling

**Subscription-based cost (Jeff's actual stack, Feb 2026):**

| Item | Monthly Cost |
|------|-------------|
| Claude Max x2 (2 accounts) | $400 |
| ChatGPT Pro (16 accounts at scale) | $3,200+ |
| Gemini (included in Google account) | $0 |
| VPS (Contabo, 64GB RAM) | $40-56 |
| Dedicated bare metal x2 (32-core AMD, 256GB RAM each) | $680 |
| **Total (full scale)** | **~$4,300+/month** |
| **Total (starter, 1 of each)** | **~$440-456/month** |

The starter cost for a solo practitioner: VPS ($40-56) + Claude Max ($200) + ChatGPT Pro ($200) = ~$440-456/month.

> *"Before claude code max i was spending ~$100 a day on sonnet"*
> -- Jeffrey Emanuel

> *"Opus basically compresses a whole engineering team into a context window. When velocity 10x's, the $200 feels less like cost and more like leverage."*
> -- Jeffrey Emanuel

> *"My token usage cost on some of these tools far exceeds the subscription cost. Makes me think the cost of tokens are subsidized by the model providers to a significant degree."*
> -- Jeffrey Emanuel

**Cost optimization levers:**

1. **Use flat-rate subscriptions (Max/Pro) instead of API billing.** Jeff's entire methodology is built on subscription tiers. At 50+ agents, per-token pricing would be ruinous.
2. **Use Sonnet/Codex for bulk execution, Opus/Gemini for review.** The cheaper, faster models handle the volume; the expensive models handle the judgment.
3. **Kill stalled agents early.** An agent burning tokens in a loop produces no value. Monitor via `git log --since="45 min ago"` and kill anything with 0 commits in that window.
4. **Context compaction is not free.** Every compaction event degrades quality. For long sessions, terminate and restart rather than accumulating compactions.

### 3.4.4 Hardware Scaling

**Single-machine ceiling:** Jeff routinely runs 50+ agents on one machine.

> *"I am often running 50+ agents on a single machine, and if you don't have something like that set up, the machine quickly becomes an unresponsive mess."*
> -- Jeffrey Emanuel

**Hardware progression (from Jeff's tweets):**

| Stage | Hardware | Agent Ceiling |
|-------|----------|---------------|
| Starter | VPS, 64GB RAM, 8-16 cores | 5-10 agents |
| Intermediate | Workstation, AMD 5975wx (32 cores) | 20-30 agents |
| Advanced | 64-core workstation + VPS fleet | 40-50 agents |
| Jeff's Feb 2026 | 3 machines (64-core primary + 2x bare metal Contabo) | 50-80+ agents |

> *"I have two dedicated bare metal servers for around $340/month each from Contabo, hosted in the US. They each have a 32-core AMD CPU and 256gb of RAM and NVME SSD."*
> -- Jeffrey Emanuel

**Scaling infrastructure (tools Jeff built to cope):**

| Problem | Tool | Purpose |
|---------|------|---------|
| Rust builds crush the primary machine | **RCH** (Remote Compilation Helper) | Offloads `cargo build` to a fleet of dedicated VPS machines |
| Disk fills up from build artifacts | **SBH** (Storage Ballast Helper) | Intelligently clears junk so RCH keeps working |
| Zombie/runaway processes | **PT** (Process Triage) | Bayesian zombie process detection |
| RAM/CPU exhaustion from swarm | **SRPS** (System Resource Protection) | Auto-deprioritize background processes |
| Temp files and stuck tests | Dedicated maintenance agent | Has SSH to all machines, clears junk and kills runaways |

> *"Running big agent swarms can be hazardous to your machine's health. It has become such a problem for me that I've been forced to create a suite of programs to help me cope with this, including: (1) remote_compilation_helper for offloading builds to a fleet of dedicated VPS machines (this is the single most critical one). (2) storage_ballast_helper for intelligently clearing out junk so you avoid filling up your primary machine. Without this, rch stops working after a few hours. (3) process_triage for helping to diagnose messed up runaway or zombie processes."*
> -- Jeffrey Emanuel

> *"It's so incredibly useful to have a dedicated agent with knowledge of and ssh access to all your machines, that is highly skilled in the art of maintaining machines that are used for big agent swarms. Basically, clearing out junk temp files and killing stuck tests and runaways."*
> -- Jeffrey Emanuel

---

!!! tip "Related pages on this site"

    - [Dispatch Reference](../reference/dispatch.md)

