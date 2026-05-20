---
name: english-storyteller-iterate
description: >
  Stage 5 of the English Storyteller pipeline. For each dimension below 9: identify
  the exact failing passage, diagnose the failure against the rubric, produce a surgical
  fix, and re-score that dimension. Repeat until all 8 dimensions score ≥ 9 or 5
  iterations are exhausted. This is the refinement loop — not a full redraft.
  Triggers as part of the english-storyteller pipeline after 04-score, or standalone
  when asked to improve, refine, iterate, polish, or raise scores on English prose.
allowed-tools: Read
---

# Stage 05 — Iterate

Fix every dimension below 9. Do this surgically — identify the exact passage, diagnose the exact failure, produce the minimal fix that raises the score without disturbing neighboring prose.

**This is not a full redraft.** A redraft is a failure mode. Surgical edits preserve what already works.

---

## Step 1: Consume Handoff from Stage 04

Read the `scores` and `dimensions_below_9` from the 04-score handoff block.
Read the `flagged_passages` list — these are your starting targets.
Read the rubric anchors for each failing dimension: `${CLAUDE_PLUGIN_ROOT}/rubric/scoring.md`.

---

## Step 2: Prioritize Dimensions

Order the failing dimensions by impact:

1. Dimensions that affect multiple other scores (Show-Don't-Tell failures often drag Character Voice and Emotional Register)
2. Dimensions with the lowest absolute score (a 5 needs more work than an 8)
3. Dimensions with the most specific flagged passages (easier to fix when the problem is precisely located)

Tackle in this order. Do not jump ahead.

---

## Step 3: For Each Failing Dimension

Run this loop once per failing dimension:

### 3a. Locate the Passage

The flagged passage from stage 04 is the starting point. But also scan the full prose for additional instances of the same pattern. A single fix that addresses the flagged passage but misses three other instances is not a fix — the dimension will re-score identically.

### 3b. Diagnose the Failure

Name the specific pattern using the vocabulary from 03-rewrite's rules:

| Dimension | Common patterns |
|-----------|----------------|
| Rhythm | uniform_sentence_length / staircase_absent / peak_overlong / valley_underlong |
| Character Voice | generic_narrator / voice_drift_across_sections / character_dialogue_homogeneous |
| Continuity | temporal_gap / spatial_anchor_missing / emotional_state_reset / section_break_unexplained |
| Prose Density | adverb_on_strong_verb / qualifier_cluster / abstract_noun_replacing_concrete / double_noun |
| POV Consistency | omniscient_intrusion / character_thought_access_violation / knowledge_smuggled |
| Emotional Register | register_oscillation / unearned_sentiment / flat_affect_broken_early / sentimental_close |
| Show-Don't-Tell | insight_statement / emotion_labeled / explanation_after_implication / tell_follows_show |
| Reference Fidelity | craft_move_absent / benchmark_fingerprint_absent / style_generic / move_misapplied |

### 3c. Apply the Fix

**Rules for surgical editing:**
- Change the minimum number of words to resolve the pattern
- Do not change sentences adjacent to the fix unless they interact with it
- Preserve all proper nouns, names, and cultural references
- Preserve the source's section structure
- If fixing Show-Don't-Tell: delete the tell sentence; never rewrite it as a different form of tell
- If fixing Rhythm: change sentence length, not sentence content
- If fixing Prose Density: cut words; do not add words
- If fixing Reference Fidelity: add or strengthen the craft move; do not remove other techniques

**For each fix, produce:**
1. The original passage (verbatim)
2. The diagnosis (one sentence)
3. The fix (the new passage)
4. The change log (bullet, one line per change)

### 3d. Re-score the Dimension

Score only the fixed dimension against the rubric. Use the rubric anchors.
If the dimension is now ≥ 9: mark it RESOLVED.
If the dimension improved but is still below 9: apply a second fix in the same pass.
If the dimension did not improve: flag for manual review (do not loop more than twice on the same dimension in a single iteration).

---

## Step 4: Verify No Regressions

After all fixes are applied, check each previously-passing dimension:

- Read through all changed passages
- Verify that fixes to one dimension did not introduce failures in another
- If a regression is detected: revert the specific change that caused it and find an alternative fix

---

## Step 5: Re-score Full Prose

After all dimension fixes and regression checks, produce a full re-score using the 04-score format.

If all 8 dimensions ≥ 9: the pipeline exits. Output the final prose + final score card.
If any dimension is still below 9: run another iteration (back to Step 2), now using the updated scores and fresh flagged passages.

**Maximum 5 iterations.** If after 5 iterations any dimension is below 9:
- Output the best version produced across all iterations
- Mark the failing dimensions explicitly
- Recommend manual review for those specific dimensions

---

## Output Format Per Iteration

```
ITERATION [N] — [N] dimensions below 9
══════════════════════════════════════

FIXING: [Dimension Name] (current score: X/10)

Original passage:
  "[verbatim quote]"

Diagnosis: [pattern name] — [one sentence]

Fixed passage:
  "[new passage]"

Changes:
  • [what changed and why]
  • [what changed and why]

Re-score: Dimension X → [new score]/10 — [RESOLVED | STILL BELOW 9]

---

[repeat for each failing dimension]

---

FULL RE-SCORE AFTER ITERATION [N]:
══════════════════════════════════════
[04-score format — all 8 dimensions]

Average: X.X/10
Verdict: [STRONG PASS | PASS | FLAG | FAIL]
Remaining below 9: [dimensions] | NONE — PIPELINE EXIT
```

---

## Pipeline Exit

When all 8 dimensions ≥ 9:

```
══════════════════════════════════════
PIPELINE COMPLETE
All 8 dimensions ≥ 9. Prose is publication-ready at benchmark level.
Iterations used: [N] of 5
Final scores: R:[X] V:[X] C:[X] D:[X] P:[X] E:[X] S:[X] F:[X]
Average: [X.X]/10
══════════════════════════════════════
```

Output the final prose in full. No meta-commentary. No "Here is the final version." Start with the prose.
