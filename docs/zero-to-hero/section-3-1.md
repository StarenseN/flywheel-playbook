---
icon: lucide/wrench
---

## 3.1 Tool Reference

The flywheel is built from approximately 20 interconnected tools. No single tool is transformative on its own; the compounding effect comes from how they reinforce each other. NTM spawns agents that use Agent Mail to coordinate. BV tells agents what to work on. CASS and CM give agents memory across sessions. DCG and SLB prevent disasters, which is what allows "vibe mode" (dangerous flags enabled) to be safe.

> *"The magic isn't in any single tool. It's in how they work together. Using three tools is 10x better than using one."*
> -- Jeffrey Emanuel

The tools are organized into tiers based on how central they are to the workflow.

---

### 3.1.1 Tier 1 -- Core (Always Running)

These are the tools that define the flywheel. Every ACFS project uses all of them.

#### Claude Code (CC)

**Role:** Primary AI coding agent. The workhorse of the flywheel.

Claude Code is the CLI interface to Claude models. It runs inside tmux panes managed by NTM, reads AGENTS.md for project context, communicates with other agents via Agent Mail, and executes beads as directed by BV. In Jeff's typical formation, 5-6 Claude Code instances (running Opus) handle primary implementation work, plus one dedicated commit agent running P11 only.

Key characteristics:
- Runs in tmux (managed by NTM), persistent across SSH disconnects
- Reads `AGENTS.md` and `CLAUDE.md` for project-specific instructions
- Supports MCP tools (Agent Mail, CASS skills, etc.) via `settings.json` hooks
- PreToolUse hooks enable DCG (destructive command guard) and RCH (remote compilation)
- "Vibe mode" = passwordless sudo + dangerous flags enabled, guarded by DCG

**Forensic signal in Jeff's repos:** Named agents in commits (RedSnow, BrownLake, PurpleBear), multiple commits in the same 1-5 minute window confirming parallel execution.

---

#### Beads Rust (br / bd)

| | |
|---|---|
| **Stars** | 623 |
| **Language** | Rust |
| **GitHub** | [beads_rust](https://github.com/Dicklesworthstone/beads_rust) |

**Role:** Local-first issue tracker. The execution graph for all agent work.

Beads is not a todo list. It is a dependency-aware task graph stored in SQLite (primary) with JSONL for git portability. Every task has: scope, acceptance criteria, context, rationale, dependencies (blocks/related/parent-child), constraints, and priority (P0-P4). Agents never refer back to the plan during execution; the beads are self-contained.

**Key commands:**
```
br create "title" --priority P1 --parent <epic-id>    # Create a bead
br list --status open --sort priority                   # List open beads
br close <id>                                           # Mark complete
br stats                                                # Velocity metrics
br dep add <id> --blocks <other-id>                     # Add dependency
bd ...                                                  # Alias (backward compat)
```

**Scale:** Jeff has hit 693 beads/day at peak velocity. A typical project has 500-2000 beads created during P05 (Plan to Beads).

**Forensic signal:** `beads.db` or `beads.jsonl` in repo root. Bead IDs in commit messages (`[bead-NNN]` or `bd-xxxxx`). High-impact beads completed before low-impact ones (PageRank ordering from BV).

---

#### Beads Viewer (bv)

| | |
|---|---|
| **Stars** | 891 |
| **Language** | Go |
| **GitHub** | [beads_viewer](https://github.com/Dicklesworthstone/beads_viewer) |

**Role:** Graph-theory triage engine for beads. Tells agents what to work on.

BV treats the bead graph as a DAG and applies nine graph metrics including PageRank and betweenness centrality to determine which beads are highest-impact and ready for execution. Its robot protocol (`--robot-*` flags) produces JSON output that agents consume directly.

**Key commands:**
```
bv --robot-triage          # JSON output: highest-impact actionable beads, PageRank-sorted
bv --robot-insights        # Analysis of graph structure, bottlenecks
bv --robot-timeline        # Projected completion timeline
bv --robot-next            # Single highest-priority ready bead
bv -export-pages /tmp/bv   # Static HTML site of the bead graph
```

**How agents use it:** P08 (Execute Beads) and P24 (Use BV) both direct agents to call `bv --robot-triage` to pick their next work item. This is how coordination happens without a human assigning tasks.

> *"They follow the plan as embodied in the beads task structure, and they use my bv tool to pick the best bead to work on next."*
> -- Jeffrey Emanuel

**Forensic signal:** High-impact beads completed before low-impact ones. "bv" referenced in AGENTS.md. PageRank-consistent commit ordering.

---

#### NTM -- Named Tmux Manager

| | |
|---|---|
| **Stars** | 160 |
| **Language** | Go |
| **GitHub** | [ntm](https://github.com/Dicklesworthstone/ntm) |

**Role:** Agent cockpit. Spawns, monitors, and manages agent sessions.

NTM is the control plane for the agent swarm. It manages named tmux panes where Claude Code, Codex, and Gemini CLI instances run. 80+ commands cover session management, prompt broadcasting, file conflict detection, and context rotation. Sessions survive SSH disconnects.

**Key commands:**
```
ntm --robot-spawn=<project> --spawn-cc=4 --spawn-cod=4 --spawn-gmi=2
                                          # Spawn 10 agents: 4 CC, 4 Codex, 2 Gemini
ntm --robot-send=<session> --msg='<prompt>'
                                          # Send a prompt to a specific agent
ntm --robot-terse                         # Compact status of all sessions
ntm --robot-status --json                 # Full JSON status
```

**Naming convention:** NTM auto-generates color+noun names for agents: RedSnow, BrownLake, PurpleBear, PinkMountain, GreenRiver, BluePeak. These names appear in Agent Mail messages and sometimes in commit attribution.

**The human steering pattern:** Jeff monitors agents via NTM status and Agent Mail. When he sees a stall, he sends P09 (Anti-deadlock). When he sees a bug, P02 (Deep Review). When context compaction fires, P20 (Refresh). All prompts are sent verbatim via Stream Deck buttons.

> *"I don't even need to look at the button labels anymore."*
> -- Jeffrey Emanuel, on his Stream Deck + NTM workflow

**Forensic signal:** Named agents in commits. Multi-agent commits in the same minute window. "ntm" referenced in AGENTS.md.

---

#### CASS -- Coding Agent Session Search

| | |
|---|---|
| **Stars** | 524 |
| **Language** | Rust |
| **GitHub** | [coding_agent_session_search](https://github.com/Dicklesworthstone/coding_agent_session_search) |

**Role:** Unified search across ALL agent sessions. The memory backbone.

CASS indexes 11 agent formats: Claude Code, Codex, Cursor, Gemini, ChatGPT, Cline, Aider, Pi-Agent, Factory, OpenCode, Amp. Powered by Tantivy for sub-60ms keyword queries, with optional MiniLM semantic search. When an agent needs to know "have we solved this before?" or "what did I do last session?", CASS is the answer.

**Key commands:**
```
cass search "query"                       # Keyword search across all sessions
cass search "query" --mode semantic       # Semantic search (MiniLM embeddings)
cass index ~/.claude/projects/            # Re-index session files
```

**How it fits:** CASS is the read-only prior-art layer. Before starting a new task, Jeff (or an agent with CASS as a skill) searches for prior work: "Have we handled this error pattern before?" "What approach did the Gemini agent use for the buffer pool?" CM (CASS Memory) builds on top of CASS to create persistent procedural memory.

> *"I do have a cass skill which the agents can use to find past conversation excerpts easily that are relevant."*
> -- Jeffrey Emanuel

**Forensic signal:** No direct git signal (it is a personal context recall tool). Its value shows up indirectly: agents making decisions consistent with prior sessions, patterns reappearing across projects.

---

### 3.1.2 Tier 2 -- Safety, Quality, and Coordination

These tools extend the core with safety nets, quality gates, and cross-session memory.

#### MCP Agent Mail

| | |
|---|---|
| **Stars** | 1,742 |
| **Language** | Python (Rust variant: 22 stars) |
| **GitHub** | [mcp_agent_mail](https://github.com/Dicklesworthstone/mcp_agent_mail) |

**Role:** Inter-agent coordination. The messaging backbone.

Agents register identities, send/receive messages, and declare file reservations to prevent edit conflicts. HTTP FastMCP server with Web UI and static export. Agents use it through MCP tools in Claude Code, Codex, or Gemini CLI.

**Key operations (via MCP tools, not CLI):**
- Register agent identity on spawn (P18)
- Send messages to specific agents by name
- Declare file reservations (prevents two agents editing the same file)
- Check inbox for blockers, discoveries, and coordination messages
- Agent Mail SQLite is a monitoring surface for the human operator

**The anti-deadlock pattern:** P09 exists specifically because agents can fall into "communication purgatory" -- messaging each other without shipping code. P09 forces them back to work: "Don't get stuck in communication purgatory where nothing is getting done; be proactive about starting tasks."

**Forensic signal:** "check agent mail" in commits. Cross-agent coordination messages. AGENTS.md with "Agent Mail" coordination section. `mcp_agent_mail_rust` sync commits confirm parallel agents.

---

#### UBS -- Ultimate Bug Scanner

| | |
|---|---|
| **Stars** | 132 |
| **Language** | Bash |
| **GitHub** | [ultimate_bug_scanner](https://github.com/Dicklesworthstone/ultimate_bug_scanner) |

**Role:** AST-grep patterns, 1000+ detection rules, 8 languages, 18 categories. The quality gate.

UBS is the standard scan that runs as part of P21 (UBS Scan prompt). Sub-5s scans. Auto-wires into Claude Code, Codex, Gemini, and Cursor.

**Key commands:**
```
ubs .                                     # Scan entire project
ubs file.ts file.py                       # Scan specific files (<1s)
ubs $(git diff --name-only --cached)      # Scan staged files only
```

**Forensic signal:** "ubs scan" in commit notes. Defensive patterns added post-scan. Maps to P21 (UBS Scan prompt).

---

#### DCG -- Destructive Command Guard

| | |
|---|---|
| **Stars** | 89 |
| **Language** | Rust |
| **GitHub** | [destructive_command_guard](https://github.com/Dicklesworthstone/destructive_command_guard) |

**Role:** Claude Code PreToolUse hook. Blocks dangerous commands BEFORE execution.

DCG is what makes "vibe mode" (passwordless sudo, dangerous flags enabled) safe. It intercepts every tool use attempt and checks against 50+ rule packs across 17 categories: git, filesystem, DB, k8s, cloud, CI/CD. Fail-open design means it never blocks legitimate work.

**What it catches:**
- `rm -rf` (outside /tmp)
- `git reset --hard`, `git push --force`, `git branch -D`
- `git clean -f`, `git stash drop/clear`
- Database drops, k8s deletes, cloud resource destruction

**Configuration:** Lives in `~/.claude/settings.json` as a PreToolUse hook.

> *"Agent safety should be baked into the runtime, not bolted on after. Validation gates that catch destructive ops before they execute is such an obvious pattern yet almost nobody ships with it by default."*
> -- @doodlestein community member, Feb 2026

> *"[DCG] gets called automatically by the agent on every attempted tool use. [...] The agent will learn from dcg and stop trying to do dumb stuff. Definitely that happens within the individual session level via in-context learning, and might even persist now that Claude Code has built in native memory by default."*
> -- Jeffrey Emanuel

**Forensic signal:** N/A for Jeff's repos (personal safety tool, no git trace).

---

#### Brenner Bot

| | |
|---|---|
| **Stars** | 28 |
| **Language** | TypeScript |
| **GitHub** | [brenner_bot](https://github.com/Dicklesworthstone/brenner_bot) |

**Role:** Research orchestration inspired by Nobel laureate Sydney Brenner.

Not a code review bot. Brenner Bot is an "iterated knowledge distillation" system that converts unstructured research corpora into refined "algebras of ideas" with operators. Hypothesis lifecycle tracking, discriminative test design, anomaly management, evidence pack integration.

Jeff describes the broader value beyond molecular biology:

> *"Although the system itself is very cool and powerful, and Brenner's approach and worldview are breathtakingly brilliant and fruitful, perhaps the more relevant thing is the process of iterated knowledge distillation that was used to convert a massive amount of unstructured, meandering stories about his life and work into a refined 'algebra of ideas' complete with operators. [...] What else could it be used for? Perhaps something as far afield as philosophy or the classics?"*
> -- Jeffrey Emanuel

**Forensic signal:** Deep research docs, hypothesis files in repos. Very rare. Jeff's most complex research projects only.

---

#### CM -- CASS Memory System

| | |
|---|---|
| **Stars** | 253 |
| **Language** | TypeScript |
| **GitHub** | [cass_memory_system](https://github.com/Dicklesworthstone/cass_memory_system) |

**Role:** Procedural memory for agents. Three-layer cognitive architecture: Episodic -> Working -> Procedural.

CM transforms scattered sessions into persistent cross-agent memory. Patterns discovered in one Cursor session automatically help Claude Code in the next session. It builds on top of CASS (session search) to create durable, actionable memory.

**Key commands:**
```
cm context "<task>" --json     # Inject relevant procedural rules into agent prompts
cm mark <id> --helpful         # Positive feedback loop
cm mark <id> --harmful         # Negative feedback loop
cm reflect                     # Trigger memory consolidation
cm stats                       # Memory health metrics
cm stale                       # Find outdated memories
```

**How it fits:** Pre-session, Jeff runs `cm context "<task>"` to load relevant procedural rules for the upcoming work. This context gets injected into agent prompts, giving fresh agents the benefit of prior sessions' discoveries.

**Forensic signal:** `cm context` blocks in project AGENTS.md. Consistent patterns across Jeff's many projects.

---

#### RCH -- Remote Compilation Helper

| | |
|---|---|
| **Stars** | 35 |
| **Language** | Rust |
| **GitHub** | [remote_compilation_helper](https://github.com/Dicklesworthstone/remote_compilation_helper) |

**Role:** Claude Code PreToolUse hook. Offloads cargo builds to remote workers.

Intercepts build commands, syncs source via rsync + zstd compression, builds on the remote machine, streams artifacts back. Jeff has a 64-core machine dedicated to compilation; RCH makes it transparent to agents running on VPS instances.

**Forensic signal:** Fast build times on VPS-class hardware. No direct git trace.

---

#### SLB -- Simultaneous Launch Button

| | |
|---|---|
| **Stars** | 59 |
| **Language** | Go |
| **GitHub** | [simultaneous_launch_button](https://github.com/Dicklesworthstone/simultaneous_launch_button) |

**Role:** Two-person rule for dangerous commands. Nuclear-launch-style safety.

Four risk tiers: CRITICAL (2+ approvals required), DANGEROUS, CAUTION, SAFE. Cryptographic signing, rollback support. For security-critical projects where a single misconfigured agent could cause real damage.

**Forensic signal:** Security-critical projects only.

---

### 3.1.3 Tier 3 -- Acceleration and Infrastructure

These tools enable Jeff's specific scale (50+ agents across 3 machines) but are optional for most practitioners.

#### CAAM -- Coding Agent Account Manager

| | |
|---|---|
| **Stars** | 59 |
| **Language** | TypeScript |
| **GitHub** | [coding_agent_account_manager](https://github.com/Dicklesworthstone/coding_agent_account_manager) |

**Role:** Sub-100ms auth switching across multiple AI provider accounts.

Jeff runs 2 Claude Max accounts ($400/month), ChatGPT Pro ($200/month), and Gemini. With 10+ agents across 3 machines, CAAM handles smart rotation, cooldown tracking, health scoring, and vault isolation. When one account hits rate limits, CAAM transparently rotates to the next.

**Key commands:**
```
caam switch      # Rotate to next healthy account
caam status      # Health and cooldown state for all accounts
caam rotate      # Force rotation
```

> *"Nah, I'd rather burn the tokens. The scripts never work right. I do have a lot of pro/max accounts, though."*
> -- Jeffrey Emanuel

**When you need it:** Only relevant when running more agents than a single subscription supports (roughly >5 concurrent Claude Code instances on Max).

---

#### S2P -- Source to Prompt TUI

**Role:** Interactive code-to-prompt generator.

A TUI tool for assembling code context into prompts. Useful when you need to extract specific code sections for review or multi-model comparison, particularly during P04/P15 planning rounds where you paste code into competing model conversations.

---

#### Meta Skill (ms)

| | |
|---|---|
| **Stars** | 126 |
| **Language** | Rust |
| **GitHub** | [meta_skill](https://github.com/Dicklesworthstone/meta_skill) |

**Role:** Skill management with MCP integration.

Dual persistence (SQLite + Git), hybrid search (BM25 + semantic + RRF), UCB bandit optimization for skill selection. Manages the growing library of agent skills and prompts.

**Key commands:**
```
ms search "query"    # Find relevant skills
ms install <id>      # Install a skill
ms list              # List available skills
```

---

#### RANO

| | |
|---|---|
| **Stars** | 23 |
| **Language** | Rust |
| **GitHub** | [rano](https://github.com/Dicklesworthstone/rano) |

**Role:** Network observer. Tracks outbound connections from AI CLI processes.

Monitors what Claude/Codex/Gemini is connecting to. Useful for auditing agent behavior and understanding which external services agents contact during execution.

---

#### Supporting Tools (Quick Reference)

| Tool | Role |
|------|------|
| **APR** (Automated Plan Reviser Pro) | Automates multi-round P04 spec review using extended reasoning. `apr refine plan.md --rounds 10` |
| **JFP** (JeffreysPrompts CLI) | Browse and install battle-tested prompts. `jfp list / jfp search / jfp install`. This IS the MASTER_PROMPTS.md -- the CLI version. |
| **RU** (Repo Updater) | Sync 100+ GitHub repos in one command. `ru sync` |
| **SRPS** (System Resource Protection) | ananicy-cpp + curated rules. Auto-deprioritize background processes. Essential at 10+ agent scale. |
| **MDWB** (Markdown Web Browser) | Convert websites to markdown for LLM consumption |
| **TRU** (TOON Rust) | Token-optimized notation, 30-50% context reduction |
| **PT** (Process Triage) | Bayesian zombie process detection |
| **CAUT** (Usage Tracker) | Track LLM provider usage and costs |
| **WA** (WezTerm Automata) | Terminal hypervisor for multi-agent automation (superseded by FrankenTerm) |

---

### 3.1.4 Decision Matrix: Which Tools Do You Need?

Not everyone needs the full toolchain. Here is a decision matrix based on project scale:

| Scale | Agents | Essential Tools | Recommended Additions |
|-------|--------|----------------|----------------------|
| **Solo** | 1 CC | Beads (br), CLAUDE.md | UBS, DCG |
| **Small team** | 2-3 agents | + NTM, Agent Mail, BV | + CASS, SLB |
| **Standard** | 4-6 agents | + CM, UBS, DCG | + RCH (if Rust), SRPS |
| **Heavy** | 7-10 agents | + CAAM (if multi-account), SRPS | + APR, Meta Skill |
| **Jeff-scale** | 11-50+ agents | All of the above | + S2P, RANO, SLB, RU |

**The minimum viable flywheel** is: Beads + BV + AGENTS.md + one Claude Code instance. You can run the entire methodology with just these. Everything else is acceleration.

---

!!! tip "Related pages on this site"

    - [Tool Quick Reference](../reference/toolchain.md)

