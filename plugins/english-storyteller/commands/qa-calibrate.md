---
description: Run english-judge-agent against the golden English corpus and report per-dimension accuracy, verdict-band agreement, and tolerance breaches.
allowed-tools: Read, Bash
---

# QA Calibrate

Verify that `english-judge-agent` agrees with human ground truth on the golden English
corpus. The judge passes calibration if its scores match the corpus within tolerance.

This command is **read-only**: it reports, it does not modify the rubric, corpus, or judge.

## Step 1: Locate the corpus

```bash
ls "${CLAUDE_PLUGIN_ROOT}/eval/golden-corpus/index.json" 2>/dev/null
```

If not found: stop. Direct user to `eval/golden-corpus/README.md` for setup.

If found: read `index.json`. Extract `tolerances` and `passages`.

## Step 2: Iterate passages

For each passage in `index.json.passages`:
1. Read the passage file
2. Extract the prose body (everything after frontmatter `---`)
3. Delegate to `english-judge-agent` with:
   ```
   passage: <prose body>
   tone_preset_name: <passage.tone_preset>
   tone_overrides: <passage.tone_overrides or {}>
   references_json: <passage.references_json_summary or null>
   applicable_dimensions: <passage.applicable_dimensions>
   attempt_number: 1
   previous_attempt_scores: []
   ```
4. Parse `SCORES_JSON`, `FAILING_DIMENSIONS`, `TOP_ISSUE_TAGS` from MACHINE BLOCK
5. Record calibration sample

Run passages **sequentially** — do not parallelize.

## Step 3: Aggregate

Compute per-dimension MAE and Max Error across all applicable samples.
Compute verdict-band agreement rate.
Identify PASS↔FAIL flips (most severe).
Report top-issue-tag overlap (informational only).

## Step 4: Apply tolerances

Judge PASSES calibration if ALL of:
- Every per-dimension MAE ≤ `tolerances.per_dimension_mae_threshold`
- Every per-dimension Max Error ≤ `tolerances.per_dimension_max_error_threshold`
- Verdict-band agreement ≥ `tolerances.verdict_band_agreement_threshold`
- If `zero_pass_fail_flips_required`: zero PASS↔FAIL flips

## Step 5: Output

```
JUDGE CALIBRATION REPORT
─────────────────────────
Corpus: N passages
Run on: <ISO date>

PER-DIMENSION ACCURACY
                       MAE    Max Err   Samples
Rhythm                 X.XX   X.X       N
Character Voice        X.XX   X.X       N
Continuity             X.XX   X.X       N
Prose Density          X.XX   X.X       N
POV Consistency        X.XX   X.X       N
Emotional Register     X.XX   X.X       N
Show Don't Tell        X.XX   X.X       N
Reference Fidelity     X.XX   X.X       N

VERDICT-BAND AGREEMENT
Total agreements: X/N (XX%)
PASS↔FAIL flips:  X  ← most severe
PASS↔FLAG flips:  X
FLAG↔FAIL flips:  X

DISAGREEMENTS ABOVE TOLERANCE
P{NN} ({dim}): predicted X, truth Y, gap Z
(or: "None.")

TOP-ISSUE-TAG OVERLAP (informational)
Overlap rate: XX%

TOLERANCES
per_dimension_mae_threshold:        X.XX
per_dimension_max_error_threshold:  X.XX
verdict_band_agreement_threshold:   X.XX
zero_pass_fail_flips_required:      true/false

VERDICT: JUDGE CALIBRATION PASS  |  JUDGE CALIBRATION FAIL
```

If FAIL, append:
```
Reasons:
- [explicit breach]

Recommendation:
- Review quality-rubric.md anchors for dimensions with worst MAE.
- Do NOT rely on english-judge-agent for production gating until calibration PASSES.
```

## Constraints

- Do NOT modify any files
- Do NOT delegate to orchestrator or humanizer — calibration uses only english-judge-agent
- Do NOT skip passages; if a passage fails to parse, abort with clear error naming the passage
