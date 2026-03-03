---
icon: lucide/monitor
---

## 2.1 Setting Up Your Environment

### 2.1.1 Hardware

**Minimum:** Any Linux or Mac machine, 16GB RAM, stable internet. This is enough to run a single agent and learn the methodology.

**Recommended:** 32-core CPU, 48-64GB RAM, NVMe SSD. Each active agent consumes roughly 2GB of RAM. Running 10 agents in parallel requires headroom for the agents, the build tools, and the test suites.

**Dedicated server option:** A bare-metal VPS from Contabo, OVH, or Hetzner runs $50-340/month depending on specs. Jeff's own setup uses two 32-core/256GB bare-metal servers (~$340/month each from Contabo, bare metal, not VPS) plus a home workstation. The home workstation was an $849 eBay find with a $2,802 upgrade to 512GB ECC RAM. He also runs a dedicated terminal just for health monitoring. You do not need this to start; a $50/month VPS with 32GB RAM is enough for 4-6 agents.

> *"Probably better off just getting a beefy Linux machine from OVH or Hetzner for $50/month."*
> — Jeffrey Emanuel (advice to beginners)

---

### 2.1.2 Subscriptions

Priority order for a new practitioner:

1. **Claude Max ($200/month):** the primary execution engine. Non-negotiable.
2. **ChatGPT Pro ($200/month):** for multi-model competition during planning. Strongly recommended.
3. **Gemini CLI:** often available on free tier. Useful as a third voice in plan reviews.
4. **VPS** ($20-50/month): use if you don't have a local machine with enough resources.

Total comfortable starter cost: **$440-456/month.**

API-based usage (without subscriptions) runs roughly $10 per 30 minutes of active Claude Code use. A Max subscription subsidizes heavy usage heavily; the breakeven is roughly an hour of daily use.

> *"Before Claude Code Max I was spending ~$100 a day on Sonnet. Now it's like $30/day."*
> — ACFS community member

As you scale to more parallel agents, you may need additional Max accounts. The CAAM tool (part of the stack) handles automatic rotation between accounts to maximize throughput without hitting per-account rate limits.

---

### 2.1.3 Installing ACFS

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/agentic_coding_flywheel_setup/main/install.sh | bash -s -- --yes --mode vibe
```

The installer runs 10 phases in sequence: user setup, filesystem, shell configuration, CLI tools, language runtimes, agent CLIs, cloud/database services, the Dicklesworthstone stack, and finalization. Total time is roughly ~30 minutes on a standard VPS.

What it installs (highlights):
- **Agent CLIs:** Claude Code (`cc`), Codex (`cod`), Gemini CLI (`gmi`)
- **Flywheel tools:** beads_rust (`br`), beads_viewer (`bv`), NTM (tmux manager), Agent Mail (MCP server), CASS (memory), CM (compatibility checker)
- **Supporting tools:** xf (search), brenner (code review), ruff (Python linter), plus ~500 utilities in `~/.local/bin/`
- **Shell enhancements:** Zsh with autosuggestions, syntax highlighting, powerlevel10k prompt

To update an existing installation:

```bash
acfs update
```

---

### 2.1.4 Post-Install Validation

```bash
acfs doctor
```

Every line should be green. Common issues after a fresh install:

| Symptom | Fix |
|---------|-----|
| PATH not updated | Restart your shell (`exec zsh`) |
| API key missing | Set in `~/.claude/settings.json` |
| MCP server not running | `systemctl status mcp-agent-mail` |
| DCG warning | Run `dcg install` (optional, for git safety) |

If `acfs doctor` reports all green, run a smoke test:

```bash
cc                    # start a Claude Code session
# type: "What tools do you have access to?"
# verify it can see Agent Mail, beads, etc.
```

---

### 2.1.5 Your Shell After ACFS

After installation, your PATH includes these aliases:

| Alias | Tool | Purpose |
|-------|------|---------|
| `cc` | Claude Code | Primary coding agent |
| `cod` | Codex CLI | OpenAI coding agent |
| `gmi` | Gemini CLI | Google coding agent |
| `br` | beads_rust | Bead lifecycle management |
| `bv` | beads_viewer | Visual bead management |
| `ntm` | NTM | Tmux session manager |
| `xf` | xf | Full-text + semantic search |

Jeff also maps his 32 master prompts to a physical Stream Deck. Each prompt fires with a single button press, enabling sub-second prompt dispatch to any agent session. This is optional but becomes valuable once you're managing 4+ agents simultaneously.

> *"Each of these blurbs takes under a second to do with a single button press using my little command palette gizmo."*
> — Jeffrey Emanuel

---

### 2.1.6 The Learning Hub and Onboard Tutorial

After installation, run `onboard` for an interactive 33-lesson tutorial (254 minutes total) covering everything from Linux basics through full agentic workflows. The learning hub is also available at [agent-flywheel.com/learn](https://agent-flywheel.com/learn).

Lessons are grouped into three tiers: **Foundations** (Lessons 1-9: Linux, shell, git, and toolchain basics for practitioners new to CLI-based development), **Core Methodology** (Lessons 10-20: the flywheel loop, prompting, planning, beads, and case studies), and **Advanced** (Lessons 21-33: multi-machine scaling, custom tool building, and production operations).

Key lessons for practitioners:
- **Lesson 10** — The Flywheel Loop: how all 29 tools connect into a single workflow
- **Lesson 18** — The Art of Agent Direction: Jeff's core prompting methodology (complements [section 1.5](section-1-5.md))
- **Lesson 19** — Case Study: cass-memory: build a complex project in one day (complements [section 2.5.8](section-2-5.md))
- **Lesson 20** — Case Study: SLB: from tweet to working tool in one evening (complements [section 2.5.8](section-2-5.md))

---

!!! tip "Related pages on this site"

    - [Tool Reference](section-3-1.md)
    - [Configuration Reference](section-3-3.md)

