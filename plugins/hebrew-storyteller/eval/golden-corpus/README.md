# Golden Hebrew QA Corpus

Calibration corpus for `hebrew-judge-agent`. Each passage is hand-scored by a human; `/hebrew-storyteller:qa-calibrate` runs the judge against the corpus and reports drift.

## How it works

1. `index.json` lists 18 passages with per-dimension ground-truth scores and tolerances.
2. Each passage `.md` file has frontmatter (passage_id, target_dimension, ground_truth_score on the target dimension, applicable_dimensions, tone_preset) plus a Hebrew body.
3. `/qa-calibrate` iterates `index.json.passages`, sends each passage body to `hebrew-judge-agent`, parses the judge's `SCORES_JSON` machine block, and compares to `ground_truth_scores`.
4. The judge PASSES calibration if: every per-dimension MAE ≤ 1.0, no single passage error > 2.5 on any dimension, verdict-band agreement ≥ 0.85, and zero PASS↔FAIL flips.

## Ground-truth methodology

- **Scores are set by a human author, never by Claude.** If Claude generated a passage, the human must still validate every score before the passage joins the corpus.
- **Provisional vs. validated.** The bundled 18 passages are provisional: their Hebrew bodies were AI-drafted to anchor to specific rubric bands, but scores require native-speaker validation before being trusted as calibration ground truth. Treat the initial corpus as a starting point; re-score and edit before relying on calibration results.
- **Anchor to rubric bands, not vibes.** When setting a ground-truth score, locate the closest rubric anchor (2, 5, 8, or 10) in `skills/hebrew-qa-validator/references/quality-rubric.md` and justify the score in the passage's `notes` field.
- **The target dimension is what the passage primarily exemplifies.** Other dimensions still get scores (they affect the average and verdict), but calibration emphasis is on the target.

## Adding a passage

1. Pick the dimension subdirectory (`naturalness/`, `dialogue-realism/`, etc.) or `multi-dimension/` for cross-cutting cases.
2. Create `P{NN}-{band}.md` (e.g. `P19-flag.md`) with the frontmatter schema below.
3. Write the Hebrew body.
4. Score it against the rubric, by hand, citing which anchor band you matched per dimension.
5. Add an entry to `index.json.passages` with the same scores.
6. Run `/hebrew-storyteller:qa-calibrate` to confirm the corpus still loads and the new passage participates.

### Per-passage frontmatter schema

```yaml
---
passage_id: P19
target_dimension: naturalness                   # one of: naturalness, ai_detectability, register_consistency, dialogue_realism, rhythm_variety
ground_truth_score: 7                            # on target_dimension only
ground_truth_verdict: FLAG                       # band of the average; PASS / FLAG / FAIL
applicable_dimensions:                           # always 4; include dialogue_realism iff body has dialogue
  - naturalness
  - ai_detectability
  - register_consistency
  - rhythm_variety
dialogue_present: false
tone_preset: contemporary-israeli-realism        # one of the 10 named presets
tone_overrides: {}
notes: "One-sentence justification anchored to a rubric band."
expected_top_issue_tags:                         # what the judge's TOP_ISSUE_TAGS should plausibly contain; soft check, not pass/fail
  - emotional_exposition
---

[Hebrew body here]
```

## Re-calibration discipline

The judge must be re-calibrated whenever **any** of these change:

- `agents/hebrew-judge-agent.md` (judge instructions)
- `skills/hebrew-qa-validator/references/quality-rubric.md` (rubric anchors)
- The underlying model version (e.g. Sonnet 4.6 → Sonnet 4.7)

If calibration FAILS after a change, the judge cannot be used for production gating until the change is reverted or the rubric/judge is re-tuned and calibration PASSES.

## Tolerances (in `index.json`)

- `per_dimension_mae_threshold: 1.0` — mean absolute error per dimension across all applicable samples must be ≤ 1.0.
- `per_dimension_max_error_threshold: 2.5` — no single passage may have predicted vs. truth gap > 2.5 on any applicable dimension.
- `verdict_band_agreement_threshold: 0.85` — ≥85% of passages (≥16 of 18) must land in the correct PASS/FLAG/FAIL band.
- `zero_pass_fail_flips_required: true` — even one PASS↔FAIL flip fails calibration (PASS↔FLAG and FLAG↔FAIL flips are tolerated within agreement threshold).

Tighten or loosen these in `index.json` as the corpus grows and judge confidence is established. Changes to tolerances should be documented in a commit message.
