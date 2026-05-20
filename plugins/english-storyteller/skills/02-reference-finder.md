---
name: 02-reference-finder
description: >
  Use web search to identify 3–5 comparable published English literary works. Extract a
  style excerpt from each, annotate the technique it demonstrates, and rank by relevance
  to the source material's signals. Step 2 of the english-storyteller pipeline.
  Triggers on: "find references", "reference finder", "comparable works", "style search",
  "benchmark works", "what to read", "style model", "literary models", "find authors".
allowed-tools: Read, WebSearch
---

# 02 — Reference Finder

Identify published literary works that match the source material's signals and serve as
the style anchor for the rewrite pass.

## Input

Use the `REFERENCE SIGNALS` section from `01-ingest.md` output, or accept direct user
input specifying genre, tone, and themes.

## Process

**Step 1: Load style taxonomy**

Read `${CLAUDE_PLUGIN_ROOT}/skills/reference-finder/references/style-taxonomy.md`.
Use the `Signal-to-Author Matrix` to identify 5–7 candidate authors.

**Step 2: Run targeted searches**

Run 2–3 web searches using queries constructed from the ingestion signals. Example queries:
- `"[genre] literary fiction [tone] [theme] best novels"
- `"[tone] style fiction novels critically acclaimed"`
- `"novels like [description of source's feeling and approach]"`

For each candidate work found:
1. Verify it is a published literary novel with established prose craft reputation
2. Find or reconstruct a representative excerpt (50–100 words of actual prose)
3. Confirm the excerpt demonstrates technique(s) relevant to the source material

**Step 3: Score and rank**

For each candidate, score relevance 1–10 based on:
- Genre match (0–3 points)
- Tone match (0–3 points)
- Technique overlap with source's needs (0–4 points)

Select the top 3–5 by score. The #1 work is the `anchor_work`.

**Step 4: Annotate techniques**

For each selected work's excerpt, identify 2–3 specific techniques from the style taxonomy.
For each technique: name it, quote the line(s) in the excerpt that use it, explain
how the author applies it (one sentence).

## Output

Display to user:

```
REFERENCE WORKS IDENTIFIED
───────────────────────────
Signals used: [genre, tone, themes from ingestion]

RANK 1 — [Title] by [Author] ([Year])
Relevance: X/10
Excerpt:
  "[50–100 words of prose]"
Techniques:
  1. [technique_name]: "[quoted phrase]" — [one-sentence explanation]
  2. [technique_name]: "[quoted phrase]" — [one-sentence explanation]
  3. [technique_name]: "[quoted phrase]" — [one-sentence explanation]
Relevance reasoning: [one sentence]

RANK 2 — [Title] by [Author] ([Year])
[same format]

RANK 3 — [Title] by [Author] ([Year])
[same format]

[RANK 4, 5 if identified]

ANCHOR WORK: [Title] by [Author]
COMPOSITE STYLE NOTE:
  [2–3 sentences synthesizing what the top works share and what the rewrite should do]
```

Also write to `.story-bible/references.json` using the schema from
`${CLAUDE_PLUGIN_ROOT}/skills/reference-finder/SKILL.md`.

## Constraints

- Do NOT fabricate excerpts. If a genuine excerpt cannot be found via web search,
  note this and use a technique description without a quoted excerpt. Mark the excerpt
  field as `"[excerpt unavailable — technique described above]"`.
- Do NOT cite a work for theme alone when its prose style is irrelevant to the source.
- If fewer than 3 works can be confidently identified, report what was found and
  recommend the user review `corpus/` for pre-curated examples.
- The anchor work must have at least 2 concrete technique annotations; if only 1 can
  be identified, demote it to rank 2 and promote the next work.
