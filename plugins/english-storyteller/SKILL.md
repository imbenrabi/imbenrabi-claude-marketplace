---
name: english-storyteller
description: >
  Transform raw source material (notes, drafts, foreign-language text, transcripts) into
  publication-ready English literary fiction. Runs a five-stage pipeline:
  ingest → reference-find → rewrite → score → iterate. Specializes in military realism
  and dark psychology. Run stages in order: 01 → 02 → 03 → 04 → 05.
  Triggers on: "rewrite this", "make this literary", "English fiction", "improve prose",
  "write this chapter", "translate and rewrite", "benchmark this", "apply literary style",
  "polish English", "military fiction", "war prose", "literary rewrite", "dark psychology",
  "make this publishable", "run the pipeline", "english storyteller", "rewrite to English".
allowed-tools: Read, WebSearch
---

# English Storyteller — Master Pipeline

Transform any source into publication-ready English literary fiction through five sequential stages.

## Pipeline Overview

```
01-ingest       → extract genre, tone, theme, structure signals from source
02-reference-finder → web search 3-5 published English works; extract style excerpts
03-rewrite      → produce English literary draft using benchmark style
04-score        → score 8 dimensions (each 0–10); block output below threshold
05-iterate      → fix each dimension below 9; re-score; repeat until all ≥ 9
```

## When to Run

Run the full pipeline when:
- Source material exists (any language, any form — notes, draft, transcript, foreign text)
- Goal is publishable English literary prose
- Genre is military realism, dark psychology, or adjacent territory

Run individual skills when:
- 01-ingest: source analysis only
- 02-reference-finder: benchmark search without rewrite
- 04-score: QA check on already-written English prose
- 05-iterate: improvement pass on prose that has already been scored

## Handoff Protocol

Each skill produces a **handoff block** at the end of its output. The next skill reads this block before starting. Format:

```
─── HANDOFF → [NEXT SKILL] ───
[structured fields for next stage]
```

Do not skip stages. Do not proceed to 03-rewrite without a 02-reference-finder handoff — the rewrite depends on the benchmark excerpts.

## Quality Gate

No prose leaves stage 04 unless average score ≥ 7.0.
No prose exits the pipeline until all 8 dimensions score ≥ 9.
Stage 05 runs until this condition is met or 5 iterations are exhausted.

## Loading Corpus

Before any rewrite pass, read the corpus files in `${CLAUDE_PLUGIN_ROOT}/corpus/`. These are golden excerpts from published English literary fiction with technique annotations. Use them as style anchors when 02-reference-finder returns fewer than 3 results or when offline.

## Loading Rubric

Before scoring, read `${CLAUDE_PLUGIN_ROOT}/rubric/scoring.md`. It contains per-dimension descriptors for scores 2, 5, 8, and 9+.
