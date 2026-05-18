---
description: Run hebrew-judge-agent against the golden Hebrew QA corpus and report per-dimension accuracy, verdict-band agreement, and tolerance breaches. Use to verify the judge is calibrated before relying on it for production quality gating, and after any change to the rubric, judge instructions, or underlying model.
allowed-tools: Read, Bash
---

# QA Calibrate

Verify that `hebrew-judge-agent` agrees with human ground truth on the golden Hebrew corpus. The judge passes calibration if its scores match the corpus within tolerance; otherwise the judge cannot be trusted for production quality gating until re-tuned.

This command is **read-only**: it reports, it does not modify the rubric, the corpus, or the judge.

## Step 1: Locate the corpus

```bash
ls "${CLAUDE_PLUGIN_ROOT}/eval/golden-corpus/index.json" 2>/dev/null
```

If the file does not exist: stop and explain that calibration requires the golden corpus at `eval/golden-corpus/index.json`. Direct the user to `eval/golden-corpus/README.md` for setup.

If found: read `index.json`. Extract `tolerances` and `passages`.

## Step 2: Iterate passages

For each passage in `index.json.passages`:

1. Read the passage file at `eval/golden-corpus/{passage.file}`.
2. Extract the Hebrew body (everything after the frontmatter `---` block).
3. Delegate to `hebrew-judge-agent` with this input block:
   ```
   passage: <Hebrew body>
   tone_preset_name: <passage.tone_preset>
   tone_overrides: <passage.tone_overrides if present, else {}>
   applicable_dimensions: <passage.applicable_dimensions or inferred from ground_truth_scores non-null fields>
   attempt_number: 1
   previous_attempt_scores: []
   ```
4. Receive the judge's response. Parse two machine-block lines:
   - `SCORES_JSON: {...}` — predicted per-dimension scores, average, verdict.
   - `TOP_ISSUE_TAGS: [...]` — predicted top issues (used for soft check only).
5. Record one calibration sample: `{passage_id, predicted_scores, predicted_verdict, ground_truth_scores, ground_truth_verdict, expected_top_issue_tags, predicted_top_issue_tags}`.

Run passages sequentially — do not parallelize. The judge should treat every passage as a fresh, isolated invocation, and serial execution prevents any accidental context bleed.

## Step 3: Aggregate

Compute across all 18 samples:

**Per-dimension accuracy** (for each of the 5 dimensions):
- Mean Absolute Error: `mean(|predicted[dim] - truth[dim]|)` across samples where `truth[dim]` is not null.
- Max Absolute Error: `max(|predicted[dim] - truth[dim]|)` across same.
- Sample count.

**Verdict-band agreement:**
- Agreement rate: `count(predicted_verdict == truth_verdict) / 18`.
- Flip breakdown:
  - PASS↔FAIL flips (most severe)
  - PASS↔FLAG flips
  - FLAG↔FAIL flips

**Disagreements above tolerance:**
- List every sample where `|predicted[dim] - truth[dim]| > tolerances.per_dimension_max_error_threshold` (default 2.5). Format: `P{id} ({dim}): predicted X, truth Y, gap Z`.

**Top-issue-tag soft check** (informational, not a pass/fail criterion):
- For each sample with non-empty `expected_top_issue_tags`, compute overlap with `predicted_top_issue_tags`. Report total overlap rate.

## Step 4: Apply tolerances

The judge **PASSES calibration** if and only if ALL of:

- Every per-dimension MAE ≤ `tolerances.per_dimension_mae_threshold` (default 1.0)
- Every per-dimension Max Error ≤ `tolerances.per_dimension_max_error_threshold` (default 2.5)
- Verdict-band agreement rate ≥ `tolerances.verdict_band_agreement_threshold` (default 0.85)
- If `tolerances.zero_pass_fail_flips_required` is true (default true): zero PASS↔FAIL flips

Otherwise: FAIL, with explicit list of which tolerance(s) breached.

## Step 5: Output

Format exactly as below. Use ASCII separators for terminal-friendliness.

```
JUDGE CALIBRATION REPORT
─────────────────────────
Corpus: 18 passages
Run on: <ISO date>

PER-DIMENSION ACCURACY
                       MAE    Max Err   Samples
Naturalness            X.XX   X.X       N
AI Detectability       X.XX   X.X       N
Register Consistency   X.XX   X.X       N
Dialogue Realism       X.XX   X.X       N
Rhythm Variety         X.XX   X.X       N

VERDICT-BAND AGREEMENT
Total agreements: X/18 (XX%)
PASS↔FAIL flips:  X  ← most severe
PASS↔FLAG flips:  X
FLAG↔FAIL flips:  X

DISAGREEMENTS ABOVE TOLERANCE
P{NN} ({dim}): predicted X, truth Y, gap Z
P{NN} ({dim}): predicted X, truth Y, gap Z
(or: "None.")

TOP-ISSUE-TAG OVERLAP (informational)
Overlap rate: XX% of samples shared at least one top-issue tag with ground truth.

TOLERANCES (from eval/golden-corpus/index.json)
per_dimension_mae_threshold:        1.00
per_dimension_max_error_threshold:  2.50
verdict_band_agreement_threshold:   0.85
zero_pass_fail_flips_required:      true

VERDICT: JUDGE CALIBRATION PASS  |  JUDGE CALIBRATION FAIL
```

If **PASS**: end with the verdict line. No further commentary.

If **FAIL**: append:

```
Reasons:
- [explicit breach, e.g. "Naturalness MAE 1.42 exceeds threshold 1.00"]
- [explicit breach, e.g. "1 PASS↔FAIL flip detected (zero required)"]

Recommendation:
- Review quality-rubric.md anchors for dimensions with worst MAE.
- If judge instructions changed recently, revert or refine.
- Do NOT rely on hebrew-judge-agent for production gating until calibration PASSES.
```

## Constraints

- Do NOT modify `quality-rubric.md`, `hebrew-judge-agent.md`, `index.json`, or any passage file based on calibration results. The user decides what to revise.
- Do NOT delegate to `story-orchestrator-agent`, `hebrew-humanizer`, or any other component. Calibration uses only `hebrew-judge-agent` to keep the judge isolated.
- Do NOT run more than one calibration per invocation. The user re-invokes if they want to re-test after rubric edits.
- Do NOT skip passages or fail silently. If a passage's frontmatter is malformed or the judge returns unparseable output, abort with a clear error naming the failing passage.
- Do NOT pass `previous_attempt_scores` populated — every calibration call is a fresh attempt 1.
