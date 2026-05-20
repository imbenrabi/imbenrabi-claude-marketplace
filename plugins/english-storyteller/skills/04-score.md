---
name: english-storyteller-score
description: >
  Stage 4 of the English Storyteller pipeline. Score prose on 8 dimensions (each 0–10).
  Use rubric anchors for each dimension. Produce structured verdict. Block output below
  threshold. Pass handoff block to 05-iterate.
  Triggers as part of the english-storyteller pipeline or standalone as a QA check on
  English literary prose. Triggers on: "score this", "QA check", "evaluate prose",
  "rate this writing", "how good is this", "quality check English", "does this pass".
allowed-tools: Read
---

# Stage 04 — Score

Score the prose from 03-rewrite on 8 dimensions. Apply the rubric. Do not soften scores to be encouraging. Do not inflate scores to reward effort.

**Step 1: Load rubric**

Read `${CLAUDE_PLUGIN_ROOT}/rubric/scoring.md` before scoring. It contains anchor passages at levels 2, 5, 8, and 9+ for each dimension. Match the prose against these anchors.

**Step 2: Load benchmark context**

Read the 02-reference-finder handoff block. Dimension 8 (Reference Fidelity) scores against the primary benchmark — you must know the benchmark to score against it.

---

## Scoring Dimensions

Score each dimension 0–10 (integers only). Use rubric anchors. Interpolate between anchor bands.

---

### Dimension 1: Rhythm (0–10)

Does sentence length vary deliberately? Do short sentences land at peaks? Do long sentences carry accumulation, memory, or interiority? Is there audible cadence variation within paragraphs?

- **10**: Rhythm serves meaning. Length changes are motivated by content. The prose has a pulse readers feel before they analyze.
- **8**: Mostly varied with one or two passages where rhythm plateaus.
- **5**: Some variation exists but is not consistent. Medium-length sentences dominate.
- **2**: Uniform sentence length throughout. Metronome prose.

---

### Dimension 2: Character Voice (0–10)

Is the narrator's voice distinct and consistent? Would you recognize it in a blind test? Do characters in dialogue sound different from each other and from the narrator?

- **10**: The narrator's voice is irreducible. Idiosyncratic vocabulary, sentence patterns, and perceptual focus that could belong only to this narrator.
- **8**: Consistent and distinctive. Slight genericness in 1–2 passages.
- **5**: Voice is present but several passages could belong to any literary narrator.
- **2**: Generic literary voice. Interchangeable with any competent workshop prose.

---

### Dimension 3: Continuity (0–10)

Does the prose maintain temporal, spatial, and emotional coherence across scene breaks? Does the reader always know where they are in time and space? Do emotional states carry across correctly?

- **10**: The reader is never disoriented. Continuity is so seamless it's invisible.
- **8**: One moment where spatial or temporal grounding slips briefly.
- **5**: Multiple moments of disorientation. The reader must re-orient.
- **2**: Chronology is unclear, locations unspecified, emotional states contradict across sections.

---

### Dimension 4: Prose Density (0–10)

Is every word earning its place? Is there filler, qualifier, or redundancy? Are concrete specifics used in place of abstractions?

- **10**: Compression so precise that nothing can be cut without losing information. Every noun is the right noun.
- **8**: Mostly compressed. One or two passages where an adjective or clause could be cut.
- **5**: Some filler exists. Qualifiers present. Abstract nouns replace concrete ones in places.
- **2**: Significant filler. Many sentences that could be deleted without loss. Adjective stacking. Abstract clusters.

---

### Dimension 5: POV Consistency (0–10)

Does the perspective hold? No information smuggled in from outside the narrator's access. No sudden omniscience where it was not established. No POV slippage.

- **10**: Perfect POV lock. Everything seen, heard, and known is within the narrator's possible access.
- **8**: One moment where the narrator appears to know something they couldn't.
- **5**: Two or three POV slips. The narrator occasionally reports what other characters feel or think without access.
- **2**: Consistent POV violations. The narrator is omniscient in some paragraphs and first-person limited in others.

---

### Dimension 6: Emotional Register (0–10)

Does the tone stay locked within a scene? No unearned emotional shifts. No sudden sentimentality in prose that has been cold. No sudden coldness in prose that has been intimate.

- **10**: Single, locked register throughout. Every scene has a consistent emotional key.
- **8**: One tonal departure that could be intentional or could be drift.
- **5**: Two or three register breaks. The prose shifts between emotional temperatures without transition.
- **2**: Constant register oscillation. Sentimental in one paragraph, flat in the next, lyrical in the third.

---

### Dimension 7: Show-Don't-Tell (0–10)

Is emotion implied through behavior, image, and subtext rather than stated? Are insight statements present? Are emotional labels used where scene work would serve?

- **10**: Every emotion is recoverable from action and image. Not one emotion is named before it is earned.
- **8**: Mostly shown. One or two sentences where the narrator labels an emotion that the preceding passage already implied.
- **5**: Several tell sentences. Emotion is often explained after being shown, doubling the effect.
- **2**: The prose systematically tells. Insight statements follow every scene. Emotions are labeled throughout.

---

### Dimension 8: Reference Fidelity (0–10)

How closely does the prose match the primary benchmark identified in 02-reference-finder? Does it apply the craft moves listed in the handoff block? Would a reader familiar with the benchmark recognize the influence?

- **10**: The craft moves are fully applied. A reader who knows the benchmark author sees clear formal kinship — not imitation, but genuine influence.
- **8**: 3–4 of 5 craft moves are evident. The primary benchmark's fingerprint is present in sentence-level decisions.
- **5**: 2 of 5 craft moves visible. The benchmark was consulted but its techniques are not systematically applied.
- **2**: No trace of the benchmark in the prose. Could have been written without reference to any specific author.

---

## Verdict Calculation

```
Score dimensions 1–8.
Mark Dimension 5 (POV Consistency) as N/A only if source is omniscient third-person
  with clearly established omniscient narrator.

Average = mean of all applicable scores.

| Average | Verdict |
|---------|---------|
| ≥ 8.5   | STRONG PASS — eligible for pipeline exit |
| 7.0–8.4 | PASS — but flag dimensions below 9 for stage 05 |
| 5.0–6.9 | FLAG — require 05-iterate before re-output |
| < 5.0   | FAIL — require 05-iterate; recommend full redraft |
```

The pipeline does not exit until **all 8 dimensions score ≥ 9.**

---

## Flagged Passages

List the 3–5 passages that most pulled scores down. For each:
- Quote verbatim (up to 40 words)
- Name the dimension affected
- Identify the specific pattern (use vocabulary from 03-rewrite's rule names)
- Suggest the transformation in one sentence

---

## Output Format

```
ENGLISH STORYTELLER — QUALITY SCORE
══════════════════════════════════════

Dimension 1 — Rhythm:              X/10
Dimension 2 — Character Voice:     X/10
Dimension 3 — Continuity:          X/10
Dimension 4 — Prose Density:       X/10
Dimension 5 — POV Consistency:     X/10  (or N/A)
Dimension 6 — Emotional Register:  X/10
Dimension 7 — Show-Don't-Tell:     X/10
Dimension 8 — Reference Fidelity:  X/10
══════════════════════════════════════
Average: X.X/10
Verdict: STRONG PASS | PASS | FLAG | FAIL

Dimensions below 9: [D1, D3, D7...]
Pipeline status: EXIT ELIGIBLE | CONTINUE TO 05-ITERATE

FLAGGED PASSAGES
1. "[quote]" — [dimension] — [pattern] → [one-sentence fix]
2. "[quote]" — [dimension] — [pattern] → [one-sentence fix]
3. "[quote]" — [dimension] — [pattern] → [one-sentence fix]
```

```
─── HANDOFF → 05-iterate ───
scores: {
  "rhythm": X,
  "character_voice": X,
  "continuity": X,
  "prose_density": X,
  "pov_consistency": X,
  "emotional_register": X,
  "show_dont_tell": X,
  "reference_fidelity": X,
  "average": X.X
}
verdict: "PASS | FLAG | FAIL"
dimensions_below_9: ["rhythm", "show_dont_tell"]
flagged_passages: [
  {"quote": "...", "dimension": "...", "pattern": "...", "fix": "..."},
  ...
]
primary_benchmark: "[Author, Title]"
craft_moves: ["..."]
```
