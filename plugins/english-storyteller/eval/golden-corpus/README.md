# Golden English QA Corpus

Calibration corpus for `english-judge-agent`. Each passage is hand-scored; `qa-calibrate`
runs the judge against the corpus and reports drift.

## How it works

1. `index.json` lists 18 passages with per-dimension ground-truth scores and tolerances.
2. Each passage `.md` file has frontmatter (passage_id, target_dimension, ground_truth
   scores, applicable_dimensions, tone_preset) plus an English prose body.
3. `/qa-calibrate` iterates `index.json.passages`, sends each passage to `english-judge-agent`,
   parses the judge's `SCORES_JSON` machine block, and compares to ground truth.
4. The judge PASSES calibration if: every per-dimension MAE ≤ 1.0, no single passage
   error > 2.5 on any dimension, verdict-band agreement ≥ 0.85, zero PASS↔FAIL flips.

## Ground-truth methodology

- **Scores are set by a human author, never by Claude.** If Claude generated a passage,
  the human must validate every score before the passage joins the corpus.
- **Anchor to rubric bands.** When setting a ground-truth score, cite which rubric anchor
  band (2, 5, 8, or 10) the score corresponds to. Justify the score in the `notes` field.
- **The target dimension is what the passage primarily exemplifies.** Other dimensions
  still get scores (they affect the average and verdict), but calibration emphasis is on
  the target.
- **Provisional status.** The 18 bundled passages are provisional: their prose bodies are
  AI-drafted to anchor to specific rubric bands. Scores require native-reader validation
  before calibration results can be fully trusted.

## Per-passage frontmatter schema

```yaml
---
passage_id: P01
target_dimension: rhythm
ground_truth_score: 2                    # on target_dimension only
ground_truth_verdict: FAIL               # ALL_PASS / PARTIAL_PASS / FLAG / FAIL
applicable_dimensions:
  - rhythm
  - prose_density
  - show_dont_tell
  - pov_consistency
  - emotional_register
  # include character_voice if multiple characters appear with distinct voices
  # include reference_fidelity if references_json_summary is provided
references_json_summary: null            # or brief summary of anchor work techniques
tone_preset: military-realism
tone_overrides: {}
notes: "Justification anchored to rubric band."
expected_top_issue_tags:
  - uniform_rhythm
---

[prose body here]
```

## Re-calibration discipline

Re-calibrate whenever:
- `agents/english-judge-agent.md` changes
- `skills/english-qa-validator/references/quality-rubric.md` changes
- The underlying model version changes

If calibration FAILS, the judge cannot be used for production gating until re-tuned.

## Tolerances

- `per_dimension_mae_threshold: 1.0`
- `per_dimension_max_error_threshold: 2.5`
- `verdict_band_agreement_threshold: 0.85`
- `zero_pass_fail_flips_required: true`

## Corpus coverage targets

| Dimension | FAIL passage | FLAG passage | PASS passage |
|---|---|---|---|
| rhythm | P01 | P02 | P03 |
| character_voice | P04 | P05 | P06 |
| prose_density | P07 | P08 | P09 |
| show_dont_tell | P10 | P11 | P12 |
| pov_consistency | P13 | P14 | P15 |
| reference_fidelity | P16 | P17 | P18 |
| multi-dimension | see `multi-dimension/` |  |  |
