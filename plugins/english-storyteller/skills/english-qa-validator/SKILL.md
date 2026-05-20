---
name: english-qa-validator
description: >
  Score English prose on eight quality dimensions and flag passages that require rewriting.
  Apply after generation to gate output quality. Pass threshold: all dimensions ≥ 9/10.
  Auto-triggers on quality check, review, validate, score tasks, and after prose generation.
  Triggers on: "quality check", "score", "validate", "review prose", "QA", "is this good",
  "sounds AI", "check output", "rate this", "score this", "evaluate", "how is this",
  "is this literary", "does this sound right", "prose quality", "all dimensions", "pass threshold".
allowed-tools: Read
---

# English QA Validator

> **Canonical judge process.** When scoring is required as part of a generation loop
> (not ad-hoc human inspection), delegate to `english-judge-agent` instead of applying
> this skill in-context. This skill is the source of truth for the rubric; the agent
> is the isolated process that applies it.

Score English literary prose across eight dimensions. Pass threshold: **all dimensions ≥ 9/10**.

**Step 1: Load reference file**

Read `${CLAUDE_SKILL_DIR}/references/quality-rubric.md` before scoring. It contains anchor
passages at score levels 2, 5, 8, and 10 for each dimension.

**Step 2: Load reference anchor (if available)**

If `.story-bible/references.json` exists, read it. Use the `anchor_work` and
`technique_annotations` to calibrate the `reference_fidelity` dimension score.

---

## Scoring Dimensions

Score each dimension 0–10 based on the rubric anchors in `quality-rubric.md`.

**1. Rhythm (0–10)**
Does sentence length vary with structural purpose? Do short sentences land at peaks?
10 = audible cadence; reader feels rhythm as meaning. 0 = complete metronome.

**2. Character Voice (0–10)**
Could you remove the dialogue tags and still know who is speaking and thinking?
10 = voices completely distinct and consistent. 0 = everyone sounds like the narrator.

**3. Continuity (0–10)**
Are timeline, objects, and emotional states consistent within and across sections?
10 = no contradictions; time jumps handled explicitly. 0 = contradictions that break logic.

**4. Prose Density (0–10)**
Can every sentence survive the deletion test?
10 = nothing can be cut without loss. 0 = multiple redundant or filler sentences.

**5. POV Consistency (0–10)**
Does the narrative perspective hold without unintentional head-hopping or register breaks?
10 = locked perspective; any shift is clearly intentional. 0 = perspective shifts freely.

**6. Emotional Register (0–10)**
Does the emotional temperature match the scene's function? Is modulation intentional?
10 = register serves the scene; transitions are purposeful. 0 = tonally incoherent.

**7. Show Don't Tell (0–10)**
Are emotions enacted rather than named? Is the ratio of shown to told affect high?
10 = no named emotions; all affect implied. 0 = predominantly told.

**8. Reference Fidelity (0–10)**
Does the prose apply the specific techniques from the identified benchmark works?
10 = 2–3 annotated techniques clearly present. 0 = no correspondence to benchmarks.
N/A = no `references.json` exists for this project.

---

## Verdict

All 8 applicable dimensions must reach 9/10 for PASS.

| Condition | Verdict | Action |
|-----------|---------|--------|
| All dimensions ≥ 9 | **ALL_PASS** | Output to user |
| All dimensions ≥ 7, some < 9 | **PARTIAL_PASS** | Show scores, offer targeted pass |
| Any dimension 5–8 | **FLAG** | Run targeted heuristic for that dimension |
| Any dimension < 5 | **FAIL** | Run full humanizer pass automatically |

---

## Flagged Passages

After scoring, list specific passages that pulled scores down:
- Quote the passage (up to 30 words)
- Name the issue using vocabulary from `ai-patterns.md`
  (e.g., `filter_words`, `named_emotions`, `uniform_rhythm`, `hedged_qualifiers`,
  `over_complete_dialogue`, `context_setting_opening`, `abstract_noun_cluster`,
  `adverb_stacking`, `emotional_exposition`, `pov_drift`, `register_break`)
- Name which dimension the passage hurts
- Suggest the transformation in one sentence

Limit to the 3 most impactful flagged passages. Do not list every minor issue.

---

## Output Format

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

FLAGGED PASSAGES
1. "..." — [issue_tag] — [dimension] → [suggestion]
2. "..." — [issue_tag] — [dimension] → [suggestion]
3. "..." — [issue_tag] — [dimension] → [suggestion]
```

If verdict is ALL_PASS with no flagged passages, output only the score block and the words
ALL_PASS. Keep it short.
