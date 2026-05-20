---
name: 05-iterate
description: >
  For each dimension below 9: identify the exact passage, explain the failure mode,
  produce a targeted fix, re-score that dimension. Repeat until all dimensions ≥ 9 or
  5 attempts exhausted. Step 5 (final) of the english-storyteller pipeline.
  Triggers on: "iterate", "improve scores", "fix failing dimensions", "fix below 9",
  "reach 9/10", "improve", "keep going", "another pass", "targeted fix", "fix this score".
allowed-tools: Read
---

# 05 — Iterate

Fix each dimension below 9 through targeted surgical rewrites. Repeat until all dimensions
reach 9 or the iteration budget is exhausted.

## Input

- Current prose draft
- `04-score.md` output (or `MACHINE BLOCK` from prior `english-judge-agent` run)
- `FAILING_DIMENSIONS` list and `TOP_ISSUE_TAGS` list

## Loop Design

```
MAX_ATTEMPTS = 5
attempt = current_attempt_number
failing_dimensions = [from 04-score MACHINE BLOCK]

while len(failing_dimensions) > 0 and attempt <= MAX_ATTEMPTS:

  for each failing_dimension in failing_dimensions:

    Step 1: Identify the specific passage
    Step 2: Diagnose the failure mode
    Step 3: Apply targeted fix
    Step 4: Verify fix did not break other dimensions

  Re-score all 8 dimensions (run 04-score)

  if new_failing = [] : stop with status ALL_PASS
  if attempt == MAX_ATTEMPTS : stop with MAX_ATTEMPTS_REACHED

  check for regression:
    if new_failing ⊃ old_failing (more dimensions now failing):
      roll back the regression-causing change
      report the tension

  if consecutive attempts with same failing_dimensions and no score change:
    stop with NO_IMPROVEMENT_ABORT

  attempt += 1
```

## Per-Dimension Fix Protocol

For each failing dimension, run this exact procedure:

**Step 1: Identify the specific passage**

Quote the exact words (up to 30) that are most responsible for the low score.
Name the issue tag from `ai-patterns.md` vocabulary.

**Step 2: Diagnose the failure mode**

State the failure mode precisely:
- What rule is being violated?
- Why is this passage scoring below 9?
- Is this a singular fixable issue or a pervasive pattern?

**Step 3: Apply targeted fix**

Apply ONLY the heuristic section mapped to this dimension in `04-score.md`.
Do NOT run the full humanizer pass — that is too blunt and introduces new failures.

Surgical rewrite: change the minimum number of words needed to fix the specific issue.
Annotate what changed and why.

**Step 4: Verify no regression**

Read through all 8 dimensions for the changed passage. Confirm the fix did not:
- Change the POV unexpectedly
- Alter another character's voice
- Create a continuity gap
- Break the rhythm of adjacent sentences

If a regression is detected: note it, roll back the change, and report the tension to
the user. A tension means the fix for dimension A breaks dimension B — this requires
a structural solution, not a local patch.

## Iteration Report

After each attempt, emit:

```
─── ITERATION REPORT (attempt N/5) ───
status: IN_PROGRESS | ALL_PASS | MAX_ATTEMPTS_REACHED | NO_IMPROVEMENT_ABORT
attempt: N
dimensions_fixed_this_pass: [list]
dimensions_still_failing: [list]
regressions_detected: [list or "none"]
score_delta: avg N-1 → avg N (+/-X.X)
score_log:
  attempt N — avg X.X (Rh:X CV:X Co:X PD:X PO:X ER:X ST:X RF:X)
  attempt N-1 — avg X.X (...)
```

## Final Report (when loop exits)

```
─── FINAL ITERATION REPORT ───
status: ALL_PASS | MAX_ATTEMPTS_REACHED | NO_IMPROVEMENT_ABORT
total_attempts: N
best_attempt: N

FINAL SCORES
  Rhythm:               X/10
  Character Voice:      X/10
  Continuity:           X/10
  Prose Density:        X/10
  POV Consistency:      X/10
  Emotional Register:   X/10
  Show Don't Tell:      X/10
  Reference Fidelity:   X/10  (or N/A)
  Average: X.X/10

[if ALL_PASS:]
All dimensions ≥ 9. Output is ready.

[if MAX_ATTEMPTS_REACHED or NO_IMPROVEMENT_ABORT:]
Dimensions still below 9: [list each with current score]
For each: [exact passage] — [why it resists] — [what would unlock it]
Recommendation: [specific structural change the user could make to break the blockage]
```

## Tension Reporting

When a fix for one dimension breaks another, report the tension explicitly:

```
TENSION DETECTED
  Fixing [dimension A] by [approach] breaks [dimension B] because [reason].
  To resolve: [structural option 1] or [structural option 2].
  Current best: returned at attempt N with [dimension A]: X/10, [dimension B]: X/10.
  Recommendation: [which approach to try first and why].
```

Tensions are not failures — they are structural constraints that require a human creative
decision. The plugin reports them clearly and does not try to resolve them automatically.
