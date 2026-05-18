---
name: hebrew-voice-engine
description: >
  Enforce Hebrew-native prose generation for literary, cinematic, and conversational
  storytelling. Apply register control, syntax correction, rhythm shaping, and gender
  agreement. Auto-triggers when writing Hebrew fiction, prose, dialogue, narration, or
  any story content in Hebrew.
  Triggers on: "כתוב בעברית", "prose", "fiction", "dialogue", "Hebrew story", "narration",
  "screenplay", "scene", "chapter", "Hebrew writing", "narrative", "literary Hebrew",
  "story", "character voice", "register", "rhythm", "prose style", "Hebrew fiction",
  "write in Hebrew", "Israeli", "עברית", "סיפור", "דיאלוג", "פרוזה", "קול".
allowed-tools: Read
---

# Hebrew Voice Engine

Rules for Hebrew-native prose generation. Load reference files before writing.

**Step 1: Load reference files**

Read the following before generating any Hebrew prose:
- `${CLAUDE_SKILL_DIR}/references/syntax-rules.md` — Hebrew syntax transforms
- `${CLAUDE_SKILL_DIR}/references/register-profiles.md` — register archetypes
- `${CLAUDE_SKILL_DIR}/references/tone-presets.md` — tone style configs

If a `.story-bible/tone-profile.json` exists in the project, load it and apply its preset values. If not, default to `contemporary-israeli-realism`.

---

## 1. Syntax Rules

**Hebrew is not English with translated words. Restructure, don't translate.**

- **Narrative**: prefer VSO order (verb first). "פתחה דנה את הדלת" reads more native than "דנה פתחה את הדלת."
- **Dialogue**: SVO is natural — characters speak in subject-first patterns. "דנה פותחת את הדלת" is fine in speech.
- **של chains**: cap at 2 consecutive. "בית אחות אמו" not "ביתה של אחותה של אמא שלו."
- **Subject omission**: drop הוא/היא when recoverable from verb conjugation. "קם בבוקר. שתה קפה. יצא." reads better than three sentences starting with הוא.
- **Sentence length**: in any 100-word window, the shortest sentence must be under 40% of the longest. Never three consecutive sentences of similar length.

## 2. Register Lock

**Identify and lock register before writing. Never oscillate within a paragraph.**

Before writing any scene:
1. Name the narrator's register (e.g., `everyday-conversational`, `melancholic-literary`)
2. Name each character's individual register (e.g., `slang-dense`, `professional`)
3. Keep these fixed within a scene. Register transitions happen between scenes, not within sentences.

Narrator register ≠ character register. A character can use slang in dialogue while the narrator maintains measured prose — but never mix registers within a single voice's paragraph.

When uncertain: prefer natural Israeli Hebrew over formal correctness. A real Israeli in that scene would not say "בטרם יצאה, הכינה לעצמה קפה" — unless the character is specifically archaic or ironic.

## 3. Connective Suppression

**Suppress אבל/ואז/כי/כאשר at paragraph starts or in excess.**

Flag any passage with:
- 3+ אבל in 200 words
- 3+ ואז in a single scene
- 3+ כי in consecutive sentences
- Any paragraph starting with ולכן/ואז/אבל three times in a row

Alternatives to אבל: new paragraph, em-dash mid-sentence, clause inversion, silence.
Alternatives to ואז: present-tense cut, new paragraph, scene jump, simple period.
Alternatives to כי: restructure into implication — let the reader infer cause.

## 4. Rhythm Shaping

**Prose must have audible cadence variation. Uniform rhythm is the signature of AI text.**

- Short sentences (under 8 words): tension, revelation, impact, shock.
- Long sentences (20+ words): interiority, memory, description, accumulation.
- Scene peaks use shortest sentences. Scene valleys use longest.
- Never write a lullaby — prose that rocks back and forth in predictable equal beats.

Read the paragraph aloud (silently). If it has a metronome, break the meter.

## 5. Gender Agreement

**Track every entity's grammatical gender. Apply consistently.**

- Before writing, identify the gender of every named entity and object appearing in the scene.
- Verbs conjugate to match grammatical subject.
- Adjectives agree with their noun.
- Possessives agree with the possessor.

If uncertain about gendered form: prefer the most common form for that entity type, then validate.

## Application

Apply all five rules on every Hebrew prose generation pass. After writing, check:
- Is the register stable throughout?
- Are there consecutive same-length sentences?
- Are connective words overused?
- Does subject agreement hold?
- Does the rhythm have variation?

Violations → rewrite before outputting. Surface any ambiguities about the story's tone profile before generating.
