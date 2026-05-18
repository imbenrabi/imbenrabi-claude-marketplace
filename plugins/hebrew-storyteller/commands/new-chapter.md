---
description: Generate a new chapter using story bible context, narrative planning, and multi-pass Hebrew quality gates.
allowed-tools: Read, Write, Edit, Bash
---

# New Chapter

Generate a new chapter with full story bible context, narrative planning, and quality validation.

## Step 1: Verify story bible

```bash
ls .story-bible/ 2>/dev/null
```

If `.story-bible/` does not exist: stop and ask the user to run `/hebrew-storyteller:new-story` first.

## Step 2: Gather chapter info

Ask the user:
1. Chapter number
2. Chapter brief: 2–3 sentences describing what happens in this chapter (not the prose — the events)
3. Active characters: which characters appear in this chapter (by name)

## Step 3: Load story context

Read:
- `.story-bible/tone-profile.json`
- `.story-bible/timeline.json`
- `.story-bible/relationships.json`
- `.story-bible/characters/{name}.json` for each active character

If chapter N > 1, also read:
- `.story-bible/chapters/{N-1}.json` for open threads, emotional residue, foreshadowing

## Step 4: Plan with narrative-planner-agent

Delegate to the `narrative-planner-agent`:
- Pass: chapter brief, active characters, previous chapter memory (if exists), story bible summary
- Receive: structured scene list with beats, pacing, tension curve, setup/payoff

Display the scene list to the user. Ask: "Does this structure work? Any changes before writing?" Allow edits before proceeding.

## Step 5: Generate prose per scene

For each scene in the plan, delegate to `story-orchestrator-agent`:
- Pass: scene description, active character files, tone-profile, scene position in chapter (first/middle/last)
- Receive (three structured blocks, always in this order):
  1. Hebrew prose for that scene
  2. Iteration report (status, attempts, score_log, delta_log, verdict_returned, warning, optional recommendation)
  3. Chapter memory delta (per-scene, with `scope: "scene"`)

The orchestrator runs its own internal write-judge-revise loop (capped at 3 attempts) and emits the best draft with the iteration report. Do NOT call the `hebrew-humanizer` skill yourself at this layer — the orchestrator already applied selective humanizer heuristics during its revision attempts.

Inspect each scene's iteration report:

- `verdict_returned == PASS` (regardless of which attempt achieved it): continue silently to the next scene.
- `verdict_returned == FLAG` with `status == PASS_ON_ATTEMPT_*`: shouldn't happen by construction; if it does, treat as PASS and continue.
- `verdict_returned == FLAG` with `status == MAX_ATTEMPTS_REACHED` or `NO_IMPROVEMENT_ABORT`: show the user the full `score_log` and ask:
  > Scene [N] reached the iteration cap with a FLAG verdict (X.X/10). Accept as-is, regenerate (orchestrator gets a fresh 3-attempt budget), or skip and edit manually?
- `verdict_returned == FAIL` with `status == MAX_ATTEMPTS_REACHED` or `NO_IMPROVEMENT_ABORT`: same prompt as FLAG-abort, but **pre-select "regenerate" as the default**. If the user accepts the FAIL prose anyway, save with the warning visible in the report.

If the user chooses "regenerate" for any scene, re-delegate that scene to `story-orchestrator-agent` (fresh invocation, fresh 3-attempt budget). Track regenerations in the final report (Step 7).

Collect each scene's prose and chapter memory delta. Assemble all scene prose into the chapter in scene order.

## Step 6: Save

Ask user: "Save chapter [N] to `chapters/chapter-[NN].md`?"

If yes:
- Write `chapters/chapter-{NN}.md` with the full assembled Hebrew prose.
- Aggregate the per-scene chapter memory deltas (collected in Step 5) into a single chapter-level memory object using the merge rules below. Write `.story-bible/chapters/{N}.json` with that aggregated result.

**Merge rules across per-scene deltas:**

- `open_threads`: union of all scene deltas, deduplicated by `id`. A thread that appears in scene K with `intensity > 0` and is then **resolved** in scene K+M (i.e., the narrative plan or the resolving scene's prose closes it) moves from `open_threads` to `callbacks` instead — do not leave resolved threads as "open."
- `unresolved_dialogue`: union, deduplicated by `(speakers, topic)` pair. If the same conversation thread appears across multiple scenes, keep only the latest `last_state`.
- `emotional_residue`: take the **last scene's** delta entry for each character (most recent emotional state wins). Do not average or merge — emotional state is a current-snapshot value.
- `callbacks`: union across all scenes (deduplicated by `(to_chapter, to_scene, what)` tuple).
- `foreshadowing`: union across all scenes (deduplicated by `setup` field).
- `character_states_at_end`: for each character, take their entry from the **last scene in which they appeared**. Characters who appeared earlier in the chapter but not in the final scene still get an entry, populated from their last appearing scene.
- `last_known_locations`: for each character, take the last non-null location from any scene they appeared in. If every scene's entry for that character was `null`, keep it `null` — never invent a location.

Write the merged result to `.story-bible/chapters/{N}.json`. Do NOT prompt the user to fill in any field. Do NOT call `story-orchestrator-agent` to "fill in unclear fields" — that cop-out is removed; if a field is `null` after merge, it stays `null`.

Schema of the saved file:

```json
{
  "chapter": N,
  "open_threads": [],
  "unresolved_dialogue": [],
  "emotional_residue": [],
  "callbacks": [],
  "foreshadowing": [],
  "character_states_at_end": {},
  "last_known_locations": {}
}
```

Update `.story-bible/timeline.json` with any events that occurred in this chapter (extracted from the narrative plan + per-scene deltas).

## Step 7: Report

Confirm:
- Chapter [N] saved to `chapters/chapter-[NN].md`
- Chapter memory saved to `.story-bible/chapters/[N].json`
- Timeline updated
- Overall QA average across all scenes: X.X/10
- Iteration summary: X PASS on attempt 1, Y PASS on attempt 2, Z PASS on attempt 3, W aborted with warning, R regenerated by user request

Do NOT proceed to write the next chapter automatically. Wait for user direction.
