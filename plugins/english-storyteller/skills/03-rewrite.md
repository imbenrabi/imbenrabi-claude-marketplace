---
name: 03-rewrite
description: >
  Apply the benchmark style to rewrite the source material. Uses reference works as
  anchor; enforces interiority, dialogue, pacing, and scene construction rules.
  Step 3 of the english-storyteller pipeline. Requires 01-ingest output and
  02-reference-finder output as inputs.
  Triggers on: "rewrite", "rewrite this", "apply the style", "rewrite using references",
  "apply benchmark", "rewrite chapter", "humanize this", "literary rewrite",
  "apply reference style", "write this in the style of".
allowed-tools: Read
---

# 03 — Rewrite

Apply the benchmark style to the source material, using the reference works as the style
anchor and the 8 QA dimensions as the quality target.

## Input

Requires:
- Source material (from `01-ingest.md` or directly provided)
- Reference anchor work and composite style note (from `02-reference-finder.md` output or
  `.story-bible/references.json`)
- Tone preset (from `.story-bible/tone-profile.json` or default `military-realism`)

## Process

**Step 1: Load context**

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/english-voice-engine/SKILL.md`
2. Read `${CLAUDE_PLUGIN_ROOT}/skills/english-voice-engine/references/prose-patterns.md`
3. Read `.story-bible/references.json` if it exists — load anchor work techniques
4. Read `.story-bible/tone-profile.json` if it exists — load preset parameters

Internalize the `composite_style_note` and the technique annotations from the anchor work.
These are not general instructions but specific constraints: the rewrite must demonstrably
apply those techniques.

**Step 2: Pre-rewrite audit**

Before generating, run `english-humanizer` detection pass on the source:
- Count AI flags (HIGH/MEDIUM/LOW)
- Identify which of the 8 QA dimensions are already strong vs. need work
- Note which specific passages are the worst offenders

**Step 3: Rewrite**

Apply the benchmark style while enforcing all `english-voice-engine` rules:

**Interiority rules:**
- Interiority occurs through physical action and free indirect discourse — not through
  "he thought" or "she felt"
- Match `interiority_depth` from tone preset (0 = camera-eye; 10 = full interior)
- At `interiority_depth` ≤ 3: body before mind, always
- At `interiority_depth` > 6: long sentences carry interior; present tense possible

**Dialogue rules:**
- Match `dialogue_pacing` from tone preset
- At `staccato`: no turn exceeds 8 words; fragments encouraged; non-answers preferred
- At `slow`: dialogue has weight; pauses are beats; silence is narrated
- Remove: all dialogue adverbs ("she said softly"), all emotional dialogue tags
- Add: physical beats in place of emotional tags; silence as a turn

**Pacing rules:**
- Scene peaks: sentences ≤ 7 words; fragments allowed
- Scene valleys / interiority: sentences 18–40 words; accumulation structure
- Transition and blocking: medium sentences (8–14 words)
- Never: 3 consecutive sentences of the same approximate length

**Scene construction rules (from scene-composer):**
- Entry: in media res; action or sensation first; never time/weather/establishing shot
- Sensory anchor: one physical detail before any emotion
- Close: on image or action; never on resolution or summary

**Reference technique application:**
- Identify which 2–3 techniques from the anchor work's annotations are most applicable
- Apply each technique at least once in the rewrite
- Do not imitate — translate: find the equivalent moment in this story where the technique works

**Step 4: Self-review**

After rewriting, run the self-audit:
- Filter words: are any still present? Remove.
- Named emotions: are any still present? Replace with enacted behavior.
- Dialogue completeness: does any turn answer the question too directly? Fragment it.
- Rhythm: are there consecutive sentences of similar length? Break.
- Reference techniques: are at least 2 of the 3 annotated techniques visible? If not, apply.

## Output

Show:
1. The rewritten prose (full text)
2. A brief change log (3–5 bullet points on the major transformations)
3. Reference technique usage (which techniques were applied and where)

This output feeds directly into `04-score.md`.
