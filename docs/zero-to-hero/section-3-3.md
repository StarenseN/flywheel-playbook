---
title: "3.3 Configuration"
icon: lucide/settings
---

Three configuration surfaces define an ACFS project: AGENTS.md (the project constitution), CLAUDE.md (the model-specific configuration), and the MCP/hooks config in `settings.json`. Together they form the complete operating environment for agents.

---

### 3.3.1 AGENTS.md -- The Full Specification

AGENTS.md is the single most important file in an ACFS project. It is not documentation. It is an operating manual for minds that will wake up without memory of any prior conversation. Jeff writes this himself or heavily directs its creation.

> *"You need to have a whole section in AGENTS dot md telling it to never do stuff like that."*
> -- Jeffrey Emanuel

> *"Try replacing your whole AGENTS dot md with one based on [the reference] but adapted to fit your project/stack (Claude can do this for you). And you have to tell it to read it carefully as the very first message."*
> -- Jeffrey Emanuel

> *"ALWAYS read the project's AGENTS.md file before starting a task. This is always essential, but especially when testing, since it will often outline testing guidance."*
> -- Jeffrey Emanuel (added to his own ~/.codex/AGENTS after an agent ignored testing instructions)

#### The Top-Level Rules

Every AGENTS.md begins with non-negotiable rules that override everything else:

**Rule 0 -- The Fundamental Override:** The human operator is always in charge. If instructed to stop, the agent stops. No exceptions, no negotiation.

**Rule 1 -- No File Deletion:** Agents are NEVER allowed to delete files without express permission. This is a top-level rule with no exceptions.

**Additional Critical Rules:**

- **No file proliferation** -- never create variations like `mainV2.rs` or `install_v2.sh`
- **No script-based code changes** -- never run scripts that process/modify code files (no codemods, no sed on source)
- **Multi-agent environment** -- when you see changes from other agents, DO NOT PANIC, DO NOT STASH, DO NOT REVERT. Treat those changes identically to changes you made yourself.
- **Git branch: `main` only** -- never create or switch to `master` or feature branches unless explicitly instructed

These rules exist because agents have a tendency to do catastrophic things when unsupervised: deleting files to "clean up," creating file variants instead of editing in place, running bulk sed operations that corrupt source, and panicking when they see unfamiliar changes from other agents. Rule 0 and Rule 1 are the first lines of defense.

#### Mandatory Sections

**1. Project Identity**
```markdown
## Project: <NAME>
<1-2 paragraph description: what this is, who it's for, why it exists>
Tech stack: <languages, frameworks, key libraries>
Repo structure: <brief map of src/, tests/, docs/>
```

**2. Tool Rules**
```markdown
## Tools
- Beads: `br` (or `bd`). Use `br create`, `br close`, `br list`.
- Beads Viewer: `bv --robot-triage`, `bv --robot-insights`.
  NEVER use `bv` without `--robot-*` flags.
- Agent Mail: Register on spawn. Check inbox before each bead. Declare file reservations.
- UBS: Run `ubs .` after major changes.
- CM: Run `cm context "<task>"` before starting new task areas.
```

**3. Bead Execution Protocol (MANDATORY)**
```markdown
## Bead Execution Protocol
For every bead you implement:
1. Pick the highest-priority ready bead via `bv --robot-triage`
2. Mark it `in_progress`: `br start <id>`
3. Implement it completely
4. Before closing: read over ALL code you just wrote with fresh eyes.
   Look for bugs, errors, edge cases. Fix everything you find.
5. Only then: `br close <id>`
6. Communicate to fellow agents via Agent Mail
7. Pick next bead. Repeat.
Do NOT commit (the commit agent handles this).
Do NOT stop between beads. Keep the loop going.
```

**4. Coordination Rules**
```markdown
## Coordination
- Claim beads before starting (mark `in_progress`)
- Communicate blockers and discoveries, not just status
- Group commits by bead/feature slice, not by time
- Bead IDs in commit messages are the coordination surface
- Agent failure is normal operations -- log it, handle it, move on
- Do NOT get stuck in "communication purgatory"
```

**5. Constraints and Best Practices**
```markdown
## Constraints
- Never use mocks in tests. Real data, real API calls, real e2e.
- Never commit ephemeral files or secrets.
- Never reopen PLAN.md during execution. The bead-map is the execution surface.
- Follow <LANGUAGE> best practices: <link to style guide or inline rules>
- All error handling must be explicit. No silent failures.
```

**6. Model Selection (if multi-model)**
```markdown
## Models
- Claude Code (Opus): primary implementation
- Codex: parallel implementation, deterministic work
- Gemini CLI: review duties, shell-restricted tasks
- Commit agent: 1x CC with EX-06 only. Never edits code.
```

#### Optional Sections

- **Best Practice Guides:** Inline or referenced. Language-specific coding standards.
- **Recovery Runbook:** What to do when the system is in a bad state.
- **Risk Register:** Top risks with severity and mitigation status.
- **Architecture Decision Records (ADRs):** Key decisions locked as reference. frankentui uses beads for ADRs (bd-10i.10.1 through bd-10i.10.6).
- **CM Context Blocks:** Pre-loaded procedural memory for agents.

#### The Five Layers of AGENTS.md

1. **Core Rules** (~70 lines) -- absolute rules, irreversible action safeguards, Rule 0 and Rule 1
2. **Toolchain Block** -- bash discipline, language-specific rules, linting commands
3. **Project-Specific** -- architecture, file structure, patterns, API conventions
4. **Shared Tooling** -- beads, Agent Mail, UBS, CASS, BV usage instructions
5. **Standard Tail** -- code editing discipline, console output conventions, quality gates

When starting a new project, copy AGENTS.md from your best previous project and ask Claude to adapt it to the new stack. The document evolves with each project.

#### Anti-Patterns

- AGENTS.md generated by an AI and never reviewed. The constitution must be human-authored or human-directed.
- AGENTS.md that is too long. Agents read it at the start of every session and after every context compaction. If it exceeds ~500 lines, agents lose attention on the critical rules.
- AGENTS.md without the Bead Execution Protocol. This section is what makes the loop work.

---

### 3.3.2 CLAUDE.md Inheritance

Claude Code supports a three-level configuration hierarchy:

| Level | File | Scope |
|-------|------|-------|
| **User** | `~/.claude/CLAUDE.md` | All projects on this machine |
| **Project** | `<repo>/CLAUDE.md` | This repository |
| **Directory** | `<repo>/subdir/CLAUDE.md` | This subdirectory (additive) |

All three are loaded and merged. More-specific files override or extend less-specific ones.

**What goes where:**

| File | Content |
|------|---------|
| `~/.claude/CLAUDE.md` | Global defaults: preferred language style, safety rules, personal conventions. Applies to all repos. |
| `<repo>/CLAUDE.md` | Project-specific instructions: tech stack, testing framework, linting rules, custom commands. Checked into git so all developers share it. |
| `<subdir>/CLAUDE.md` | Directory-specific rules: "files in this directory are auto-generated, do not edit" or framework-specific conventions. |

**Relationship to AGENTS.md:** CLAUDE.md is a Claude Code-specific feature. AGENTS.md is tool-agnostic -- it works with Codex, Gemini, and any future agent. In practice, CLAUDE.md contains Claude Code-specific configuration (MCP tool hints, hook behavior), while AGENTS.md contains the universal project constitution that all agents read.

Jeff's pattern: AGENTS.md is the primary document. CLAUDE.md supplements it with Claude Code-specific configuration and occasionally duplicates critical rules from AGENTS.md for emphasis.

> *"It's such a bummer trying to maintain separate CLAUDE dot md files. [...] I should probably modify it for codex, though."*
> -- Jeffrey Emanuel

---

### 3.3.3 MCP Configuration

MCP (Model Context Protocol) tools are configured in `~/.claude/settings.json`. This is where Agent Mail, CASS skills, and other MCP servers are wired into Claude Code.

**Example `settings.json` structure:**

```json
{
  "mcpServers": {
    "agent-mail": {
      "command": "python3.13",
      "args": ["-m", "mcp_agent_mail.cli", "serve-http", "--host", "0.0.0.0", "--port", "8765"]
    }
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash|Computer",
        "command": "dcg check \"$INPUT\""
      }
    ]
  }
}
```

**Key MCP tools in a typical ACFS setup:**

| MCP Server | Port | Purpose |
|------------|------|---------|
| Agent Mail | 8765 | Inter-agent messaging and file reservations |
| CASS (as skill) | -- | Session search as an agent-accessible tool |
| CM (as skill) | -- | Procedural memory injection |
| Meta Skill | -- | Skill search and management |

**Hooks:**

| Hook Type | Tool | Purpose |
|-----------|------|---------|
| PreToolUse | DCG | Blocks destructive commands before execution |
| PreToolUse | RCH | Intercepts cargo builds, offloads to remote |

> *"It helps if you structure the MCP tools a certain way and document it well. There's a blurb I also provide to add to your agents dot md file that helps a lot."*
> -- Jeffrey Emanuel

**Common issue:** Agents ignoring MCP tools despite configuration. Jeff's solution is explicit instruction in AGENTS.md, not just settings.json configuration:

> *"Getting claude code to use each of the tools consistently. The blurbs are often ignored: they use bd ready rather than bv, or ignore cm altogether. Should I add to the system prompt? Use hooks?"*
> -- ACFS community member

Document tool usage in AGENTS.md with explicit examples, and reference the tools in the Bead Execution Protocol.

---

### 3.3.4 NTM Layouts

NTM session layouts define the agent formation for a project. The key command is `ntm --robot-spawn` with agent count flags.

**Typical formations:**

| Formation | Command | Use Case |
|-----------|---------|----------|
| Solo | `ntm --robot-spawn=proj --spawn-cc=1` | Single-agent work, small projects |
| Small | `ntm --robot-spawn=proj --spawn-cc=2 --spawn-cod=1` | 2-3 agent projects |
| Standard | `ntm --robot-spawn=proj --spawn-cc=4 --spawn-cod=2 --spawn-gmi=1` | Jeff's typical (4-6 agents) |
| Heavy | `ntm --robot-spawn=proj --spawn-cc=6 --spawn-cod=3 --spawn-gmi=1` | 7-10 agents, heavy projects |
| Jeff-scale | 3 machines, 8 CC + 8 Codex per project | 50+ agents total |

**Formation composition:**

| Agent Type | Count | Role |
|------------|-------|------|
| Claude Code (Opus) | 5-6 | Primary implementation |
| Codex | 2-3 | Parallel implementation |
| Gemini | 1-2 | Review duties |
| Commit agent | 1 | Git operations only (EX-06) |

**Important:** All implementation agents are fungible generalists. Do not specialize. The single exception is the commit agent, which runs EX-06 only and never modifies code.

!!! note "ACFS changes the tmux prefix"
    ACFS changes the tmux prefix from the default `Ctrl-b` to `Ctrl-a`. If you are used to the default prefix, be aware of this change.

**Scaling rule:** Scale based on bead graph size and budget. A project with 200 beads does not need 15 agents. A project with 2,000 beads might.

---

### 3.3.5 Git Conventions

Git is a first-class coordination surface in ACFS. Commit messages, branch structure, and commit grouping all follow specific conventions.

**Commit messages must include bead IDs:**
```
feat(buffer-pool): implement write-back cache [bead-142]

Adds the write-back cache with dirty-page tracking and LRU eviction.
Configurable flush interval (default 5s). Includes unit tests for
eviction under memory pressure.

Co-authored-by: RedSnow <agent@ntm>
```

Bead IDs in commit messages are the ground-truth coordination surface (Doctrine #6). Agents read git messages to know what is done. Agent Mail is for higher-level coordination.

**The commit agent pattern:**
- One dedicated agent runs EX-06 continuously
- It reads the diff, groups changes into logical commits by bead/feature slice
- It writes detailed messages with bead IDs
- It never modifies code
- It pushes after each commit round

**Commit grouping:** By bead or feature slice, not by time. If three agents worked on three different beads in the same hour, that produces three separate commits (or commit groups), not one time-based merge.

**Branch structure:** ACFS projects typically work on a single branch (main) with the commit agent handling all pushes. Multi-branch workflows are avoided because they add coordination overhead that the bead system already handles.

**What never gets committed:**
- Ephemeral files (build artifacts, cache files)
- Secrets (API keys, credentials, .env files)
- Agent session data (JSONL thoughts, Agent Mail SQLite -- these are operational, not code)

---

### 3.3.6 The Stream Deck Configuration

Jeff maps all 32+ prompts to physical Stream Deck buttons for instant dispatch. Each button sends a specific prompt verbatim to the active NTM session via `ntm --robot-send`.

> *"It's already a button on my Stream Deck. What I need is for it to be fully automated."*
> -- Jeffrey Emanuel

> *"And yeah, basically manually, but it's quick because each prompt is a button on my stream deck."*
> -- Jeffrey Emanuel

The Stream Deck is not required. The prompts work identically when typed or pasted. The Stream Deck eliminates the friction of finding the right prompt and ensures it is sent verbatim without modification.

**For practitioners without a Stream Deck**, here are equivalent setups:

**Shell aliases** (add to `~/.zshrc`):
```bash
alias ex01='ntm --robot-send=$NTM_SESSION --msg="Reread AGENTS.md so it'\''s still fresh in your mind. Now check Agent Mail for any messages from other agents. Execute the highest-priority ready bead. Follow the bead execution protocol in AGENTS.md exactly. Use ultrathink."'
alias ex03='ntm --robot-send=$NTM_SESSION --msg="First read ALL of the AGENTS.md file and README.md file super carefully and understand ALL of both! Then use your code investigation agent mode to fully understand the code, and technical architecture and purpose of the project. Then register with MCP Agent Mail and introduce yourself to the other agents."'
alias ex04='ntm --robot-send=$NTM_SESSION --msg="Reread AGENTS.md so it'\''s still fresh in your mind. Use ultrathink."'
alias ex06='ntm --robot-send=$NTM_SESSION --msg="Based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with detailed commit messages for each, and then push."'
```

**Text expander** (e.g., Espanso, TextExpander): Map triggers like `;ex01`, `;rv02`, `;mt04` to the full prompt text from the [Prompt Pack](../prompts/prompt-pack.md). The key is sub-second dispatch -- any method that eliminates prompt lookup time works.

---

!!! tip "Related pages on this site"

    - [Anatomy of an ACFS Project](section-2-2.md)

