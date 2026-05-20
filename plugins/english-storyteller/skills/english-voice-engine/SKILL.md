---
name: english-voice-engine
description: >
  Enforce literary-native English prose generation. Apply register control, rhythm shaping,
  filter-word suppression, POV locking, and prose density enforcement. Auto-triggers when
  writing English fiction, prose, dialogue, narration, or any story content.
  Triggers on: "write prose", "fiction", "dialogue", "story", "narration", "scene",
  "chapter", "narrative", "literary", "literary fiction", "character voice", "register",
  "rhythm", "prose style", "write in English", "write this scene", "generate passage",
  "prose generation", "show don't tell", "point of view", "POV", "voice", "filter words",
  "prose density", "sentence rhythm".
allowed-tools: Read
---

# English Voice Engine

Rules for literary-native English prose generation. Load reference files before writing.

**Step 1: Load reference files**

Read the following before generating any prose:
- `${CLAUDE_SKILL_DIR}/references/prose-patterns.md` — rhythm and structure transforms
- `${CLAUDE_SKILL_DIR}/references/register-profiles.md` — register archetypes
- `${CLAUDE_SKILL_DIR}/references/tone-presets.md` — tone style configs

If a `.story-bible/tone-profile.json` exists in the project, load it. Apply its preset
values, including any `references.json` anchor work. If not, default to `military-realism`.

If a `.story-bible/references.json` exists, load the `composite_style_note` and the
technique annotations from the `anchor_work`. These constrain the prose style.

---

## 1. Filter Word Suppression

**Filter words interpose the narrator between the reader and the scene.**

Delete or rewrite any instance of:
- `she saw` / `he saw` / `they saw`
- `she heard` / `he heard`
- `she noticed` / `he noticed`
- `she felt` / `he felt` (as a filter — keep "she felt the wall" not "she felt sadness")
- `she watched` / `he watched` (when watching without acting)
- `she realized` / `he realized`
- `she thought` / `he thought` (free indirect discourse preferred)
- `she knew` / `he knew` (as an emotional label)

Replace with: direct sensory description, free indirect discourse, or action.

**Before:** "She heard the door close behind him."
**After:** "The door closed. She didn't turn."

**Before:** "He noticed the light was different."
**After:** "The light was different. Wrong angle."

---

## 2. Register Lock

**Identify and lock register before writing. Never oscillate within a paragraph.**

Before writing any scene:
1. Name the narrator's register (e.g., `detached-observer`, `intimate-close`, `ironic-dry`)
2. Name each character's individual register (e.g., `terse-military`, `educated-formal`, `street-vernacular`)
3. Keep these fixed within a scene. Register transitions happen between scenes, not within sentences.

Narrator register ≠ character register. A character can use profanity in dialogue while the
narrator maintains measured prose — but never mix registers within a single voice's paragraph.

When uncertain: prefer the register established in the first paragraph of the chapter.

---

## 3. Rhythm Shaping

**Prose must have audible cadence variation. Metronome rhythm is the signature of AI text.**

- Short sentences (under 7 words): tension, revelation, impact, aftermath.
- Long sentences (18+ words): interiority, accumulation, memory, description.
- Fragments (1–4 words, no main verb): aftermath, shock, emphasis.
- Scene peaks use shortest sentences. Scene valleys use longest.
- Never write three consecutive sentences of the same approximate length.
- Never start three consecutive sentences with the same word or word class.

Read the paragraph silently. If it has a metronome — if you can tap your foot to it — break the meter.

---

## 4. Prose Density Enforcement

**Every sentence must earn its place. If a sentence can be cut without information loss, cut it.**

Flag any:
- Transitional sentences that do no work ("He turned and walked to the window")
- Redundant modifiers ("cold and icy", "small little", "dark and shadowy")
- Sentences that restate what the previous sentence already shows
- Throat-clearing ("It was at this point that he realized...")
- Over-connected tissue (three consecutive sentences beginning with "And" or "But")

The deletion test: remove the sentence. Does the reader lose information they cannot
recover from context? If yes, keep it. If no, cut it.

---

## 5. POV Consistency

**Lock the point of view before writing. Know exactly what the POV character can and cannot know.**

Before writing a scene, state explicitly:
- POV: first-person / third-person close / third-person limited / omniscient
- Distance: intimate (nearly free indirect) / close (behind the eyes) / medium / far (camera)

Then enforce:
- The narrator can only report what the POV character senses, thinks, or is told
- No "meanwhile, across town, X was doing Y" in a close third without explicit POV shift
- No knowing what another character is thinking unless POV character is told, sees, or infers
- Free indirect discourse (the narrator adopting the character's idiom) is allowed; omniscient
  narrator commenting is not, unless the preset specifically calls for omniscient narration

When POV shifts (multiple POV chapters): always note the shift explicitly at scene or chapter
boundaries. Never shift mid-scene without craft justification.

---

## 6. Show Don't Tell Application

**Enacted emotion is always stronger than named emotion.**

Before writing any emotional moment:
1. Identify the emotion the scene intends to convey
2. Find the physical action, object, or behavioral tic that enacts it
3. Write the action. Delete the label.

| Named (banned) | Enacted (required) |
|---|---|
| "She felt grief" | "She folded his shirt again. Already folded." |
| "He was afraid" | "His hands checked the safety a second time." |
| "She was angry" | "She set the glass down carefully. Too carefully." |
| "He felt relieved" | "He breathed out. Sat down on the floor." |
| "She was lonely" | "She turned on the TV for the noise." |

Exception: a character can name their own emotion in dialogue when that naming is itself
revealing (usually of denial or suppression).

---

## Application

Apply all six rules on every prose generation pass. After writing, check:
- Are there filter words? ("she saw", "he noticed", "she felt [emotion]")
- Is the register stable throughout?
- Are there consecutive same-length sentences?
- Does POV drift?
- Does any sentence survive the deletion test only by explaining what the reader can infer?
- Are emotions enacted rather than named?

Violations → rewrite before outputting. Surface any ambiguities about the story's tone
profile and reference anchor before generating.
