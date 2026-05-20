---
description: Generate a new chapter using story bible context, reference anchor, narrative planning, and multi-pass quality gates (all dimensions ≥ 9/10).
allowed-tools: Read, Write, Edit, Bash
---

# New Chapter

Generate a new chapter with full story bible context, reference anchor, narrative planning,
and 8-dimension quality validation.

## Step 1: Verify story bible

```bash
ls .story-bible/ 2>/dev/null
```

If `.story-bible/` does not exist: stop and ask user to run `/english-storyteller:new-story` first.

Check if `.story-bible/references.json` exists. If not: recommend running
`/english-storyteller:find-references` first. Proceed without it if user declines.

## Step 2: Gather chapter info

Ask the user:
1. Chapter number
2. Chapter brief: 2–3 sentences describing what happens (events, not prose)
3. Active characters: which characters appear in this chapter

## Step 3: Load story context

Read:
- `.story-bible/tone-profile.json`
- `.story-bible/references.json` (if exists)
- `.story-bible/timeline.json`
- `.story-bible/relationships.json`
- `.story-bible/characters/{name}.json` for each active character
- `.story-bible/chapters/{N-1}.json` for open threads (if N > 1)

## Step 4: Plan with narrative-planner-agent

Delegate to `narrative-planner-agent`:
- Pass: chapter brief, active characters, previous chapter memory, story bible summary,
  reference composite_style_note
- Receive: structured scene list with beats, pacing, tension curve, setup/payoff

Display the scene list to the user. Ask: "Does this structure work? Any changes before writing?"
Allow edits before proceeding.

## Step 5: Generate prose per scene

For each scene in the plan, delegate to `story-orchestrator-agent`:
- Pass: scene description, active character files, tone-profile, references.json,
  scene position in chapter (first/middle/last)
- Receive (three structured blocks, always in this order):
  1. English prose for that scene
  2. Iteration report (status, attempts, score_log, failing_dimensions at each attempt)
  3. Chapter memory delta

Inspect each scene's iteration report:

- `verdict_returned == ALL_PASS` (regardless of which attempt): continue silently.
- `verdict_returned == PARTIAL_PASS` with `status == ALL_PASS_ON_ATTEMPT_*`: shouldn't
  happen by construction; treat as ALL_PASS and continue.
- `verdict_returned == FLAG` with `status == MAX_ATTEMPTS_REACHED` or `NO_IMPROVEMENT_ABORT`:
  show user the full score_log and ask:
  > Scene [N] reached the iteration cap (FLAG). Accept as-is, regenerate (fresh 5-attempt budget), or skip?
- `verdict_returned == FAIL` with `status == MAX_ATTEMPTS_REACHED`: same prompt but
  **pre-select "regenerate"** as default. If user accepts FAIL prose anyway, save with
  warning visible.

Collect all scene prose and chapter memory deltas.

## Step 6: Save

Ask: "Save chapter [N] to `chapters/chapter-{NN}.md`?"

If yes:
- Write `chapters/chapter-{NN}.md` with full assembled prose
- Aggregate per-scene memory deltas using the merge rules (same as Hebrew plugin):
  - `open_threads`: union, deduplicated by `id`; resolved threads move to `callbacks`
  - `unresolved_dialogue`: union, deduplicated by `(speakers, topic)`
  - `emotional_residue`: last scene's delta per character wins
  - `callbacks`: union across all scenes
  - `foreshadowing`: union across all scenes
  - `character_states_at_end`: last scene each character appeared in
  - `last_known_locations`: last non-null location per character
- Write `.story-bible/chapters/{N}.json` with the aggregated result
- Update `.story-bible/timeline.json` with events from this chapter

## Step 7: Report

Confirm:
- Chapter [N] saved
- Chapter memory saved
- Timeline updated
- Overall QA: dimensions that reached 9 on first attempt / after iteration
- Iteration summary: X ALL_PASS on attempt 1, Y on attempt 2+, Z aborted with warning

Do NOT proceed to write the next chapter automatically.
