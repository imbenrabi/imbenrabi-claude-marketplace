---
description: Humanize a passage of Hebrew prose — remove AI patterns, improve naturalness, fix register, improve rhythm.
allowed-tools: Read
---

# Rewrite

Run the hebrew-humanizer skill on a passage of Hebrew prose.

## Input

`$ARGUMENTS` contains the Hebrew text to rewrite.

If `$ARGUMENTS` is empty: ask the user to paste the Hebrew passage they want rewritten.

## Process

**Step 1: Detect tone context**

If `.story-bible/tone-profile.json` exists in the current directory, read it. Apply the preset constraints during rewriting (slang_density, emotional_explicitness, narration_distance).

If no story bible exists: apply `contemporary-israeli-realism` defaults.

**Step 2: Run AI pattern scan**

Count red flags using the hebrew-humanizer skill's ai-patterns inventory:
- H1: Subject repetition loop
- H2: Emotional exposition
- H3: Connective overuse
- H4: Register oscillation
- H5: LinkedIn opening
- M1–M4: Medium severity patterns

Compute weighted flag count (H×3 + M×1.5 + L×0.5).

**Step 3: Rewrite**

Apply hebrew-humanizer rewrite rules to the passage:
1. Delete emotional exposition sentences
2. Break subject repetition loops through omission and inversion
3. Fragment over-complete dialogue
4. Normalize rhythm — break uniform meter
5. Fix register oscillation
6. Compress translated-English structures to Hebrew-native forms

**Step 4: QA score**

Score the rewritten passage using hebrew-qa-validator dimensions. Report the score.

## Output Format

```
FLAGS DETECTED: X total (Y HIGH, Z MEDIUM, W LOW)

─── REWRITTEN ───
[Hebrew prose, rewritten]

─── CHANGES ───
• [What changed and why, one line per change]
• ...

QA SCORE: X.X/10 — PASS / FLAG / FAIL
```

Do NOT show the original unless the user asks for a side-by-side comparison.

Do NOT save the rewritten text — return output only. The user decides what to do with it.

If the weighted flag count is 0: report "No significant AI patterns detected. Minor style suggestions only:" and list any low-severity observations.
