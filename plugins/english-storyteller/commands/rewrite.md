---
description: Humanize a passage of English prose — remove AI patterns, improve naturalness, fix filter words, remove named emotions, improve rhythm, enforce reference style.
allowed-tools: Read
---

# Rewrite

Run the `english-humanizer` skill on a passage of English prose.

## Input

`$ARGUMENTS` contains the English text to rewrite.

If `$ARGUMENTS` is empty: ask the user to paste the passage they want rewritten.

## Process

**Step 1: Detect style context**

If `.story-bible/tone-profile.json` exists, read it. Apply preset constraints.
If `.story-bible/references.json` exists, read the anchor work techniques.
If no story bible: apply `military-realism` defaults; note reference fidelity is N/A.

**Step 2: Run AI pattern scan**

Count red flags using `english-humanizer` ai-patterns inventory:
- H1: Filter words
- H2: Named emotions
- H3: Uniform rhythm
- H4: Adverb stacking
- H5: Emotional exposition
- M1–M4, L1–L4: medium and low severity patterns

Compute weighted flag count (H×3 + M×1.5 + L×0.5).

**Step 3: Rewrite**

Apply `english-humanizer` rewrite rules:
1. Delete all filter words (H1) — put reader in scene directly
2. Replace named emotions with enacted behavior (H2)
3. Break uniform rhythm (H3) — vary sentence length purposefully
4. Replace adverb stacks with precise verbs or physical actions (H4)
5. Delete emotional exposition after dialogue (H5)
6. Fragment over-complete dialogue (M2)
7. Replace context-setting opening with in-media-res entry (M3)
8. Break symmetrical clause pairs (M4)
9. Replace abstract noun clusters with concrete anchor (M5)

**Step 4: Reference technique check (if references.json exists)**

Check whether 1–2 of the anchor work's annotated techniques are now present in the
rewritten prose. If not, apply the most applicable technique.

**Step 5: QA score**

Score the rewritten passage using `english-qa-validator` dimensions. Report scores.

## Output Format

```
FLAGS DETECTED: X total (Y HIGH, Z MEDIUM, W LOW) — weighted score: N

─── REWRITTEN ───
[English prose, rewritten]

─── CHANGES ───
• [What changed and why, one line per change, noting which dimension each change improves]
• ...

QA SCORE:
  Rhythm: X/10
  Show Don't Tell: X/10
  Prose Density: X/10
  [other dimensions as applicable]
  Average: X.X/10
  Verdict: ALL_PASS / PARTIAL_PASS / FLAG / FAIL
  Dimensions below 9: [list] / none
```

Do NOT show the original unless the user asks for side-by-side comparison.
Do NOT save the rewritten text — return output only.

If weighted flag count is 0: report "No significant AI patterns detected. Minor style
suggestions only:" and list any low-severity observations.
