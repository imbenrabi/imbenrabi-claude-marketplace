---
name: 01-ingest
description: >
  Read source material and extract genre, tone, theme, and structural signals.
  Step 1 of the english-storyteller pipeline. Run before reference finding or rewriting.
  Triggers on: "ingest", "analyze source", "read source", "extract signals", "what genre",
  "analyze this text", "what tone is this", "read this chapter", "study this text".
allowed-tools: Read
---

# 01 — Ingest

Read source material and produce a structured signal extraction that feeds into the
reference-finder, rewrite, and scoring steps.

## Input

`$ARGUMENTS` contains the source text, file path, or request to read a specific file.

If `$ARGUMENTS` is a file path: read it. If it is inline text: use it directly.

## Process

**Step 1: Read the source material**

Read the full source. If it is longer than 2,000 words, read the first and last 500 words
plus a 500-word sample from the middle.

**Step 2: Extract signals**

From the source, identify and document:

**Genre signals:**
- Literary genre (war fiction / psychological / domestic / thriller / etc.)
- Subgenre markers (military realism, dark psychology, coming-of-age, etc.)
- Whether genre is clearly established or ambiguous

**Tone signals:**
- Narrative distance (close / medium / far)
- Emotional temperature (cold / warm / lyrical / brutal / ironic)
- Pacing register (fast / slow / varied)
- Register: formal / conversational / vernacular / elevated
- Emotional explicitness (0 = pure subtext, 10 = stated directly)

**Theme signals:**
- Primary themes (2–3 maximum)
- Motifs present in the excerpt
- What the work seems to be "about" beneath the surface story

**Structural signals:**
- POV (first-person / close third / omniscient / second-person)
- Tense (past / present / mixed)
- Timeline (linear / non-linear / fragmented)
- Scene entry style (in media res vs. context-setting)

**Voice signals:**
- Narrator's register (name the archetype from `register-profiles.md`)
- Character registers (list each character and their archetype)
- Dialogue style (complete sentences / fragmented / dialect-inflected)

**Strength assessment:**
- What the source does well (specific techniques)
- What the source does that signals AI generation (specific flags)
- Which of the 8 QA dimensions are strongest / weakest in the current source

## Output

Produce a structured ingestion report:

```
INGESTION REPORT
─────────────────
Source: [file path or "inline text"]
Word count: ~N

GENRE
  Primary: [genre tag]
  Subgenre: [if identifiable]
  Confidence: high / medium / low

TONE
  Distance: [close / medium / far]
  Temperature: [cold / warm / lyrical / brutal / ironic]
  Pacing: [fast / slow / varied]
  Register: [formal / conversational / vernacular / elevated]
  Emotional explicitness: [0–10]

THEMES
  Primary: [theme 1], [theme 2]
  Motifs: [if present]

STRUCTURE
  POV: [type]
  Tense: [past / present / mixed]
  Timeline: [linear / non-linear]
  Scene entry: [in media res / context-setting / mixed]

VOICE
  Narrator: [register archetype]
  Characters:
    [Name]: [register archetype]
    [Name]: [register archetype]

QUALITY ASSESSMENT
  Strengths: [2–3 specific techniques working well]
  AI Flags: [specific patterns detected, if any]
  Dimensions to improve: [list any of the 8 dimensions that appear weak]

REFERENCE SIGNALS
  Suggested genre tags for reference search: [...]
  Suggested tone descriptors: [...]
  Suggested themes for search: [...]
```

This report is the input to `02-reference-finder.md`.
