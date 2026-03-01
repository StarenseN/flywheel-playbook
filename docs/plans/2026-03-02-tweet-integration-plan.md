# Tweet Integration ("Jeff Teaches") Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Mine @doodlestein tweets from `Twitter/tweets.jsonl`, create a "Jeff Teaches" standalone page, and insert inline blockquotes into existing playbook pages.

**Architecture:** Python script mines tweets by keyword per playbook concept, sorts by engagement, outputs curated JSON. Then we manually create the Jeff Teaches page and insert blockquotes into ~10 existing pages.

**Tech Stack:** Python 3 (stdlib only), Markdown, Zensical (MkDocs Material)

---

### Task 1: Write the tweet mining script

**Files:**
- Create: `/data/projects/Doodlestein/Twitter/scripts/mine_for_playbook.py`

**Step 1: Write the mining script**

```python
#!/usr/bin/env python3
"""Mine @doodlestein tweets for flywheel playbook integration.

Reads tweets.jsonl, filters by author, searches by keyword per playbook
concept, sorts by engagement, outputs curated_tweets.json.
"""
import json
import re
import sys
from pathlib import Path

TWEETS_FILE = Path(__file__).parent.parent / "tweets.jsonl"
OUTPUT_FILE = Path(__file__).parent.parent / "curated_tweets.json"

# Playbook concept -> keywords to search (case-insensitive)
CONCEPT_KEYWORDS = {
    "planning": ["planning", "plan ", "85%", "plan is the product"],
    "flywheel": ["flywheel", "compounding", "compound"],
    "agents_md": ["AGENTS.md", "AGENTS dot md", "agents.md", "constitution"],
    "beads": ["beads", "bead", "task graph", "execution graph"],
    "alien_artifacts": ["alien artifact", "alien", "BOCPD", "conformal", "mathemati"],
    "fresh_eyes": ["fresh eyes", "fresh-eyes", "randomly explore", "fresh eye"],
    "praise_push": ["praise", "much better", "push harder", "brilliant", "OPTIMAL"],
    "swarm": ["swarm", "5 codex", "5 claude", "instances", "parallel agent"],
    "review": ["review", "Part 1", "Part 2", "deep review", "bugs are there"],
    "premortem": ["premortem", "pre-mortem", "failed", "failure mode"],
    "mocks": ["mock", "fake data", "fake api", "end to end", "e2e"],
    "spec_evolution": ["spec evolution", "evolution", "metacognition", "spec's evolution"],
    "performance": ["profil", "p50", "p95", "p99", "latency", "throughput"],
    "skeleton": ["skeleton", "scaffold", "stub"],
    "commit_agent": ["commit agent", "commit", "git commit", "hairstylist", "salon"],
    "general_methodology": ["methodology", "approach", "workflow", "agentic coding"],
}

TOP_N = 5  # tweets per concept


def load_doodlestein_tweets():
    tweets = []
    with open(TWEETS_FILE) as f:
        for line in f:
            t = json.loads(line)
            if t.get("author", {}).get("username", "").lower() == "doodlestein":
                if not t.get("_isRetweet", False):
                    tweets.append(t)
    return tweets


def engagement(t):
    return t.get("likeCount", 0) + t.get("retweetCount", 0) * 3


def matches_concept(text, keywords):
    text_lower = text.lower()
    return any(kw.lower() in text_lower for kw in keywords)


def tweet_url(t):
    return f"https://x.com/doodlestein/status/{t['id']}"


def clean_text(text):
    # Remove leading @mentions (replies)
    text = re.sub(r'^(@\w+\s+)+', '', text)
    return text.strip()


def mine():
    tweets = load_doodlestein_tweets()
    print(f"Loaded {len(tweets)} doodlestein tweets (non-RT)")

    curated = {}
    for concept, keywords in CONCEPT_KEYWORDS.items():
        hits = [t for t in tweets if matches_concept(t["text"], keywords)]
        hits.sort(key=engagement, reverse=True)
        top = hits[:TOP_N]

        curated[concept] = []
        for t in top:
            curated[concept].append({
                "id": t["id"],
                "text": clean_text(t["text"]),
                "raw_text": t["text"],
                "likes": t.get("likeCount", 0),
                "retweets": t.get("retweetCount", 0),
                "engagement": engagement(t),
                "url": tweet_url(t),
                "created_at": t.get("createdAt", ""),
                "char_count": len(t["text"]),
            })

        print(f"  {concept}: {len(hits)} hits, top {len(top)} selected "
              f"(best: {top[0]['likeCount']}L)" if top else
              f"  {concept}: 0 hits")

    with open(OUTPUT_FILE, "w") as f:
        json.dump(curated, f, indent=2, ensure_ascii=False)

    print(f"\nWrote {OUTPUT_FILE}")
    total = sum(len(v) for v in curated.values())
    print(f"Total curated tweets: {total}")


if __name__ == "__main__":
    mine()
```

**Step 2: Run the mining script**

Run: `python3 /data/projects/Doodlestein/Twitter/scripts/mine_for_playbook.py`
Expected: prints hit counts per concept, writes `Twitter/curated_tweets.json`

**Step 3: Review output and verify quality**

Run: `python3 -c "import json; d=json.load(open('/data/projects/Doodlestein/Twitter/curated_tweets.json')); [print(f'{k}: {len(v)} tweets, best={v[0][\"likes\"]}L' if v else f'{k}: empty') for k,v in d.items()]"`
Expected: most concepts have 3-5 tweets, some may be empty (praise_push, premortem)

**Step 4: Commit**

```bash
cd /data/projects/Doodlestein
git add Twitter/scripts/mine_for_playbook.py Twitter/curated_tweets.json
git commit -m "feat: add tweet mining script for playbook integration"
```

---

### Task 2: Create the "Jeff Teaches" page

**Files:**
- Create: `/tmp/flywheel-playbook/docs/jeff-teaches.md`
- Modify: `/tmp/flywheel-playbook/zensical.toml` (add nav entry)

**Step 1: Build the Jeff Teaches page from curated tweets**

Read `Twitter/curated_tweets.json`. For each concept with good tweets (engagement > 50), create a section following the pattern:

```markdown
---
icon: lucide/quote
---

# Jeff Teaches

Jeffrey Emanuel ([@doodlestein](https://x.com/doodlestein)) invented the agentic coding flywheel. These are his words, from his public Twitter, teaching each concept of the methodology.

---

## On [Concept Name]

> "Verbatim tweet text, trimmed for clarity if very long"
>
> — @doodlestein ([source](url)) · N likes

[Adapted from @doodlestein] One-sentence paraphrase bridging the tweet
to the playbook concept. Links idea to specific principle/doctrine/phase.

→ Read more: [Relevant Page](link)
```

Concepts to cover (based on mining density):
1. Planning (85% of the work) → links to principles.md, phase-1-draft.md
2. The AGENTS.md Constitution → links to phase-0-prerequisites.md
3. Agent Swarms → links to phase-6-swarm.md
4. Beads (Execution Graph) → links to phase-4-beads.md
5. Alien Artifacts → links to phase-3-alien-artifacts.md
6. Fresh-Eyes Review → links to phase-7-review.md
7. The Flywheel Effect → links to principles.md
8. Testing (No Mocks) → links to doctrine.md
9. The Commit Agent → links to doctrine.md
10. General Methodology → links to playbook/index.md

Skip concepts with 0 tweets (praise_push, premortem) — note their absence.

**Step 2: Add nav entry to zensical.toml**

Add after the Kernel entry:
```toml
  { "Jeff Teaches" = "jeff-teaches.md" },
```

Nav becomes: `Home | Playbook | Prompts | Reference | Send Your Agent Here | Kernel | Jeff Teaches`

**Step 3: Commit**

```bash
cd /tmp/flywheel-playbook
git add docs/jeff-teaches.md zensical.toml
git commit -m "feat: add Jeff Teaches page — methodology via @doodlestein tweets"
```

---

### Task 3: Insert inline quotes into existing playbook pages

**Files to modify (insert 1-3 blockquotes each):**
- `/tmp/flywheel-playbook/docs/playbook/principles.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-0-prerequisites.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-2-refine.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-3-alien-artifacts.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-4-beads.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-6-swarm.md`
- `/tmp/flywheel-playbook/docs/playbook/phase-7-review.md`
- `/tmp/flywheel-playbook/docs/reference/doctrine.md`
- `/tmp/flywheel-playbook/docs/reference/anti-patterns.md`

**Step 1: Read each page and identify insertion points**

For each page:
1. Read the file
2. Find the first `---` after the intro paragraph (or after the H1)
3. Select the best 1-2 tweets from `curated_tweets.json` for that concept
4. Insert blockquote after the intro, before the first H2

Format:
```markdown
> "Tweet text trimmed to ~280 chars if needed..."
>
> — @doodlestein ([source](url))
```

**Step 2: Insert quotes into each page**

Use the Edit tool. One edit per page. Place the blockquote after the intro paragraph and before the first content section (H2).

Rules:
- Max 2 quotes per page
- Trim tweets longer than ~400 chars (use `...` and link to full tweet)
- Skip pages where no tweet is a good fit (don't force it)
- Keep the blockquote between the intro `---` and the first H2

**Step 3: Commit**

```bash
cd /tmp/flywheel-playbook
git add docs/playbook/*.md docs/reference/doctrine.md docs/reference/anti-patterns.md
git commit -m "feat: add inline @doodlestein quotes to playbook pages"
```

---

### Task 4: Push and verify

**Step 1: Push all changes**

```bash
cd /tmp/flywheel-playbook
git push
```

**Step 2: Verify deployment**

Wait for GitHub Pages build, then check:
- `https://starensen.github.io/flywheel-playbook/jeff-teaches/` (new page renders)
- `https://starensen.github.io/flywheel-playbook/playbook/principles/` (inline quote visible)
- Nav shows "Jeff Teaches" tab

---

## Notes

- **Terms absent from Twitter:** "praise push" and "premortem" are not in Jeff's Twitter vocabulary. He describes these concepts differently. Note this in Jeff Teaches page.
- **Tweet trimming:** Some tweets are 1000+ chars. Trim to the most quotable segment, link to full tweet.
- **xf limitation:** xf search cannot filter by author and has no semantic model. Direct `tweets.jsonl` mining is required.
- **No modification** to kernel page, llms-full.txt, or agent/index.md (robot pages).
