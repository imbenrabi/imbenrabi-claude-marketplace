---
name: hebrew-qa-validator
description: >
  Score Hebrew prose on five quality dimensions and flag passages that require rewriting.
  Apply after generation to gate output quality. Auto-triggers on quality check, review,
  validate, score tasks, and after story generation passes.
  Triggers on: "quality check", "score", "validate Hebrew", "review prose", "QA",
  "is this natural", "sounds good", "check output", "בדיקת איכות", "לבדוק", "לאמת",
  "rate this", "score this", "evaluate", "how natural", "does this sound right",
  "Hebrew quality", "naturalness", "is this Israeli", "grammar check Hebrew".
allowed-tools: Read
---

# Hebrew QA Validator

> **Canonical judge process.** When scoring is required as part of a generation loop (not ad-hoc human inspection), delegate to `hebrew-judge-agent` instead of applying this skill in-context. This skill remains the source of truth for the rubric; the agent is the isolated process that applies it.

Score Hebrew prose across five dimensions. Produce a structured verdict before any output reaches the user.

**Step 1: Load reference file**

Read `${CLAUDE_SKILL_DIR}/references/quality-rubric.md` before scoring. It contains anchor examples at each score level.

---

## Scoring Dimensions

Score each dimension 0–10 based on the rubric anchors in `quality-rubric.md`.

**1. Naturalness (0–10)**
Does this read like something a real Israeli would write or say? 10 = invisible. 0 = obviously machine-generated.

**2. AI Detectability Inverse (0–10)**
How hard is it to tell this was written by an AI? 10 = indistinguishable from human. 0 = immediately flagged.

**3. Register Consistency (0–10)**
Does the tone stay stable throughout? 10 = single coherent voice. 0 = oscillating between registers in every paragraph.

**4. Dialogue Realism (0–10)**
If dialogue exists: do characters sound like people? 10 = you could imagine hearing this conversation. 0 = characters speak in complete, grammatically perfect explanations.
If no dialogue: score this dimension as N/A and exclude from average.

**5. Rhythm Variety (0–10)**
Does sentence length and structure vary? 10 = paragraph has audible cadence. 0 = uniform beat throughout.

---

## Verdict

Calculate the average of applicable dimensions.

| Average | Verdict | Action |
|---------|---------|--------|
| ≥ 7.0 | **PASS** | Output to user |
| 5.0–6.9 | **FLAG** | Show score, ask user whether to run humanizer pass |
| < 5.0 | **FAIL** | Run humanizer pass automatically before output |

---

## Flagged Passages

After scoring, list specific passages that pulled scores down:
- Quote the passage (up to 30 words)
- Name the issue (e.g., "emotional over-explanation", "register oscillation", "uniform sentence length")
- Suggest the transformation (e.g., "delete the הרגיש clause; the preceding action implies it")

Limit to the 3 most impactful flagged passages. Do not list every minor issue.

---

## Output Format

```
HEBREW QA SCORE
───────────────
Naturalness:           X/10
AI Detectability:      X/10
Register Consistency:  X/10
Dialogue Realism:      X/10  (or N/A)
Rhythm Variety:        X/10
───────────────
Average: X.X/10
Verdict: PASS / FLAG / FAIL

FLAGGED PASSAGES
1. "..." — [issue] → [suggestion]
2. "..." — [issue] → [suggestion]
3. "..." — [issue] → [suggestion]
```

If verdict is PASS with no flagged passages, output only the score block and the word PASS. Keep it short.
