---
name: 04-score
description: >
  Run the 8-dimension scoring rubric on the rewritten prose. Output scores per dimension
  and flag anything below 9. Step 4 of the english-storyteller pipeline.
  Triggers on: "score", "evaluate", "rate this", "run the rubric", "QA score", "score this",
  "how does this score", "quality score", "run scoring", "all dimensions", "check quality".
allowed-tools: Read
---

# 04 — Score

Run the 8-dimension scoring rubric. Produce a structured verdict before any output advances.

## Input

The rewritten prose from `03-rewrite.md`, or any prose directly submitted for scoring.

## Process

**Step 1: Load rubric**

Read `${CLAUDE_PLUGIN_ROOT}/skills/english-qa-validator/references/quality-rubric.md`.
Read `.story-bible/references.json` if it exists (for reference_fidelity calibration).

**Step 2: Score each dimension**

For each dimension, locate the closest rubric anchor (2, 5, 8, or 10). Interpolate if
between bands. Use integers only; the average will be computed from integer scores.

Score 8 dimensions:
1. Rhythm
2. Character Voice
3. Continuity
4. Prose Density
5. POV Consistency
6. Emotional Register
7. Show Don't Tell
8. Reference Fidelity (N/A if no `references.json` exists)

**Step 3: Identify failing passages**

For each dimension scoring below 9: find the 1–2 specific passages (quote up to 30 words)
most responsible for the low score. Name the issue tag and the dimension it hurts.

**Step 4: Determine action**

For each dimension below 9, determine which heuristic section in
`${CLAUDE_PLUGIN_ROOT}/skills/english-humanizer/references/rewrite-heuristics.md`
addresses the failure. Map:

| Failing dimension | Primary heuristic |
|---|---|
| `rhythm` | H3 (uniform rhythm), plus rhythm patterns in `prose-patterns.md` |
| `character_voice` | M2 (over-complete dialogue), plus register profiles |
| `continuity` | Timeline/object consistency check (re-read for contradictions) |
| `prose_density` | H5 (emotional exposition), deletion audit |
| `pov_consistency` | H1 (filter words), POV lock rules |
| `emotional_register` | General register calibration; tone preset review |
| `show_dont_tell` | H2 (named emotions), enactment replacements |
| `reference_fidelity` | Re-read reference annotations; apply missing techniques |

## Output

```
ENGLISH QA SCORE
───────────────
Rhythm:               X/10
Character Voice:      X/10
Continuity:           X/10
Prose Density:        X/10
POV Consistency:      X/10
Emotional Register:   X/10
Show Don't Tell:      X/10
Reference Fidelity:   X/10  (or N/A)
───────────────
Average: X.X/10
Verdict: ALL_PASS / PARTIAL_PASS / FLAG / FAIL
Dimensions below 9: [list] / none

FAILING PASSAGES
For each dimension below 9:
  [Dimension] (score X/10):
    "quoted passage" — [issue_tag] → [one-sentence fix direction]

HEURISTIC MAP
For each failing dimension:
  [Dimension] → apply [heuristic section] in rewrite-heuristics.md

──── MACHINE BLOCK ────
SCORES_JSON: {"rhythm": X, "character_voice": X, "continuity": X, "prose_density": X, "pov_consistency": X, "emotional_register": X, "show_dont_tell": X, "reference_fidelity": X, "average": X.X}
FAILING_DIMENSIONS: ["dimension1", "dimension2"]
TOP_ISSUE_TAGS: ["tag1", "tag2"]
```

This output feeds directly into `05-iterate.md`.
