# Tweet Integration Design — "Jeff Teaches"

**Date:** 2026-03-02
**Status:** Approved

## Goal

Integrate Jeffrey Emanuel's (@doodlestein) public tweets into the Flywheel Planning Playbook. Two deliverables:

1. **Inline quotes** in existing playbook pages (blockquotes with attribution)
2. **"Jeff Teaches" page** — standalone pedagogical page where Jeff teaches each concept via his tweets

## Approach: Mine & Curate

### Step 1 — Mining

Script mines `Twitter/tweets.jsonl` for doodlestein tweets matching playbook concepts.

**Keyword → Page mapping:**

| Page | Keywords | Expected density |
|---|---|---|
| principles.md | planning, flywheel | 16+13 hits |
| phase-0-prerequisites.md | AGENTS | 80 hits |
| phase-2-refine.md | praise, planning, critique | Medium |
| phase-3-alien-artifacts.md | alien artifact | 16 hits |
| phase-4-beads.md | beads | 42 hits |
| phase-6-swarm.md | agents, swarm | High |
| phase-7-review.md | fresh eyes, review | 8 hits |
| phase-9-metacognition.md | spec evolution | Medium |
| doctrine.md | AGENTS, mocks | High |
| anti-patterns.md | skeleton, mocks | Low |
| agent/index.md | flywheel, agents | High |

**Sort by:** likes + retweets (engagement = quotability signal)

**Output:** `curated_tweets.json` — top 3-5 tweets per concept with id, text, engagement, URL

### Step 2 — Curation

Review mined tweets. Select:
- 1-3 verbatim quotes per existing page (for inline insertion)
- 1-2 verbatim quotes per Jeff Teaches section
- Write [Adapted] paraphrases for each section

### Step 3 — Inline Quotes

Insert blockquotes into existing pages. Format:

```markdown
> "Tweet text here"
>
> — @doodlestein ([source](https://x.com/doodlestein/status/ID))
```

Placement: after section intro, before technical content.
Density: 1-3 quotes per page max.

### Step 4 — Jeff Teaches Page

New top-level nav tab. Structure per section:

```
## On [Concept]

> "Verbatim tweet"
>
> — @doodlestein (N likes)

[Adapted] Paraphrase that bridges tweet to playbook concept.

→ Read more: [Page Name](link)
```

Covers ~10 concepts where tweet density is highest.

## Nav Change

```
Home | Playbook | Prompts | Reference | Send Your Agent Here | Kernel | Jeff Teaches
```

## What Does NOT Change

- Prompts stay as-is (code blocks, no citations)
- Kernel page and llms-full.txt are robot pages, not modified
- Dispatch table stays intact
- No rewriting of existing pages, only blockquote insertions

## Tone Rules

- **Verbatim quotes:** exact tweet text, in blockquotes, with source link
- **[Adapted] paraphrases:** clearly marked, bridge tweet to playbook concept
- **Never fabricate a quote** — if no tweet covers a concept, skip it
- Terms absent from Twitter ("praise push", "premortem"): note this, don't force a tweet
