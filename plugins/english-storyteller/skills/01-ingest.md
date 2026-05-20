---
name: english-storyteller-ingest
description: >
  Stage 1 of the English Storyteller pipeline. Read source material in any language
  or form; extract genre, tone, theme, POV, structural signals, and key passages.
  Produces a handoff block consumed by 02-reference-finder.
  Triggers as part of the english-storyteller pipeline or standalone when asked to
  analyze source material before rewriting.
allowed-tools: Read
---

# Stage 01 — Ingest

Read the source. Extract everything 02-reference-finder needs to find accurate literary benchmarks.

**Do not rewrite yet. Do not score yet. Analyze only.**

---

## Step 1: Read the Source

If a file path was given: `Read` the file.
If text was pasted directly: analyze as-is.
If the source is in a foreign language: translate only the analytical labels and quotes into English for the handoff block. Do not produce an English draft yet.

---

## Step 2: Extract Signals

Work through each category below. Be specific — vague labels ("emotional") are useless to the next stage.

### 2.1 Genre & Subgenre

Name the genre precisely:
- Primary: literary fiction / war narrative / psychological thriller / autofiction / memoir / etc.
- Subgenre: military realism / dark psychology / trauma narrative / dissociation study / etc.
- Mode: action-driven / interiority-driven / witness-mode / ensemble / etc.

### 2.2 POV & Narrator Distance

- POV type: first-person / second-person / third close / third distant / plural ("we")
- Narrator distance: intimate (free indirect discourse) / camera-external / split (dissociated narrator referencing self in third person)
- Reliability: reliable / unreliable / deliberately fragmented

### 2.3 Tone & Register

Name the dominant emotional register and any secondary registers:
- Primary tone: flat affect / compressed grief / dark irony / controlled rage / stunned witness / etc.
- Secondary tones (if register shifts): list each and where it appears
- Vocabulary register: military jargon / colloquial / formal / mixed
- Profanity density: none / low / high (note if functional or expressive)

### 2.4 Structural Signals

- Chapter/scene structure: continuous / fragmented / vignette-based / enumerated sections
- Time handling: linear / fragmented / retrospective / simultaneous
- White space usage: heavy / minimal / used as beat
- Italics / quotation marks / em-dashes: note if unconventional
- Interior monologue: labeled (quotation marks / italics) or unlabeled
- Paragraph length: short / variable / long / mixed

### 2.5 Thematic Core

List 3–5 themes as precise labels (not adjectives):
- e.g., "dissociation as survival mechanism" / "institutional failure vs. individual action" / "civilian witness of military reality" / "guilt deferred through action"

### 2.6 Key Passages

Extract 3–5 short passages (in original language) that best represent the source's style. These anchor the benchmark search in stage 02. For each, note what makes it distinctive.

### 2.7 Character & Voice Inventory

For each named character:
- Name / role
- Voice register (if dialogue exists)
- Relationship to narrator

For the narrator:
- What do they omit that the reader reconstructs?
- What is their dominant coping mechanism?

### 2.8 Pacing Signature

- Fastest moments: what happens? how long are sentences there?
- Slowest moments: what happens? how long are sentences there?
- Does the source use fragment sentences? inventory lists? repetition?

---

## Step 3: Identify Translation/Adaptation Challenges

If source is in a foreign language or non-literary form, flag:
- Idioms with no direct English equivalent (preserve the concept, not the words)
- Cultural references requiring localization decisions (preserve / explain / replace)
- Structural elements that are language-specific (e.g., Hebrew VSO narration → English alternatives)
- Emotional content that loses force in direct translation (flag for amplification in stage 03)

---

## Output Format

Produce a clean analysis block followed by the handoff block.

```
INGEST ANALYSIS — [source title or "Untitled"]
═══════════════════════════════════════════════

GENRE: [primary] / [subgenre] / [mode]
POV: [type] — [narrator distance] — [reliability]
DOMINANT TONE: [label]
SECONDARY TONES: [label — where it appears], [label — where]
STRUCTURE: [pattern] / time: [handling] / white space: [usage]
THEMES: [1], [2], [3], [4], [5]
PACING: fastest → [description]; slowest → [description]

KEY PASSAGES (original language):
1. "[quote]" — [what makes it distinctive]
2. "[quote]" — [what makes it distinctive]
3. "[quote]" — [what makes it distinctive]

TRANSLATION CHALLENGES:
- [challenge and recommendation]
- [challenge and recommendation]

NARRATOR COPING MECHANISM: [description]
DOMINANT LITERARY DEVICE: [name and example]
```

```
─── HANDOFF → 02-reference-finder ───
genre_label: [precise genre string for web search]
subgenre_label: [precise subgenre string]
tone_labels: ["[tone1]", "[tone2]"]
themes: ["[theme1]", "[theme2]", "[theme3]"]
structural_features: ["[feature1]", "[feature2]"]
pacing_signature: "[description of sentence-length pattern]"
key_passage_for_matching: "[the single best passage to use as style exemplar in searches]"
source_language: [ISO 639-1 code or "en"]
adaptation_notes: "[critical notes for stage 03]"
```
