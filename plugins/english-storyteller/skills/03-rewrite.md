---
name: english-storyteller-rewrite
description: >
  Stage 3 of the English Storyteller pipeline. Apply benchmark style and craft moves
  from stage 02 to produce an English literary draft. Rules for interiority, dialogue,
  pacing, and scene construction. Produces prose for stage 04 scoring.
  Triggers as part of the english-storyteller pipeline or standalone when asked to
  rewrite, translate-and-rewrite, or apply a literary style to existing prose.
allowed-tools: Read
---

# Stage 03 — Rewrite

Produce the English literary draft. Apply benchmark style. Do not explain what you're doing — write.

**Prerequisites:**
- 01-ingest handoff block consumed
- 02-reference-finder handoff block consumed
- Corpus loaded: Read `${CLAUDE_PLUGIN_ROOT}/corpus/` (all files) before writing
- Rubric loaded: Read `${CLAUDE_PLUGIN_ROOT}/rubric/scoring.md` before writing

---

## Pre-Write Checklist

Before writing the first sentence:

1. **Primary benchmark style internalized** — you know what [Author, Title] sounds like at the sentence level
2. **Craft moves memorized** — all 5 moves from the handoff block are active constraints
3. **Tone locked** — dominant tone from 01-ingest is the register ceiling; nothing softer, nothing more expressive
4. **POV fixed** — narrator distance from 01-ingest does not change within a scene
5. **Structure mapped** — source's section breaks become English section breaks (preserve architecture)

---

## Rule 1: Interiority

**Interior thought is the most commonly mishandled element in military realism and trauma narrative.**

Rules:
- Never label interior thought as "he thought" or "she realized." If it's in the character's voice, it is the character's voice.
- Do not use italics for interior thought unless the source already uses them as convention — and then use sparingly.
- Interior monologue must be shorter than the surrounding narration. Interiority that runs long is exposition wearing a costume.
- The narrator may know more than they're saying. The opposite of explanation is implication — let absence of explanation carry the weight.
- **Dissociated narrators**: when the source narrator refers to themselves in the third person (e.g., "the one watching"), preserve this device in English. Do not normalize it into first-person — the split is the content.

**Test for interiority:** remove the interior sentence. Does the reader lose information they cannot reconstruct from context and behavior? If no — delete it.

---

## Rule 2: Dialogue

**Authentic dialogue is incomplete, deflecting, and rhythmically broken.**

Rules:
- No complete sentences under stress. Stress truncates.
- No direct answer to a direct question — real people answer the question they wish they'd been asked, or go silent.
- Silence is a beat. Mark it with a physical action rather than "he said nothing."
- Military dialogue: no explanation. Commands without softeners. Questions are statements. "Let's go" not "I think we should leave."
- Em-dash interruption (—) for cut-off speech. Trailing ellipsis (...) for trailing off. Choose one per character voice and stick to it.
- Attribution: use "said" or omit entirely. Never "exclaimed," "whispered breathlessly," or similar. Physical beats replace attribution where possible.
- Dialect: do not write accent phonetically. Let vocabulary and syntax carry register.

**Dialogue density:** match the source. If the source is sparse on dialogue, do not add it. Dialogue that wasn't earned in the source is not earned in the rewrite.

---

## Rule 3: Pacing

**Sentence length is pacing. Pacing is meaning.**

| Situation | Sentence length |
|-----------|----------------|
| Combat, immediate threat | 1–5 words |
| Physical action sequence | 5–10 words |
| Transition between events | 8–15 words |
| Sensory observation | 10–20 words |
| Interiority / memory | 15–40 words |
| Character dislocation / list accumulation | variable, building |

**Staircase technique** (from benchmark corpus): peak scenes descend in sentence length. The climax is the shortest sentence. Never write a long sentence at a high-tension moment.

**Long-short puncture** (from benchmark corpus): let one very long sentence (accumulation of detail, military inventory, memory cascade) break into a single short sentence that changes everything. The length gap is the shock.

**List as catalog**: when a character is overwhelmed, let the prose list without completing the implied action. The incomplete list is the overwhelm. (Tim O'Brien technique.)

**White space**: every scene section break earns silence. Do not add explanatory connectives across section breaks ("Meanwhile..." / "Later that night..."). Cut directly.

---

## Rule 4: Scene Construction

**Every scene is already happening when we enter it.**

Scene entry rules:
- Begin with action, sensation, or mid-dialogue. Never with time/weather setup.
- The first sentence contains a verb. Ideally it is the verb.
- If the source opens a section with context-setting, cut to the first active moment.

Scene body rules:
- Sensory anchor precedes emotional state. The reader must be physically located before they can feel with the character.
- Movement carries meaning. A character who walks to the window instead of answering has answered.
- Objects remember what people want to forget. Use objects as emotional proxies.

Scene close rules:
- End before resolution. Cut one beat earlier than the obvious ending.
- Close on image, interrupted action, or a statement that opens a question.
- Never: summary sentence, thematic resolution, authorial commentary.

---

## Rule 5: Show-Don't-Tell

**The prohibition is on labeling emotion, not on having it.**

Delete any sentence of the form:
- "He felt [emotion]."
- "She realized [insight]."
- "They knew [fact that reader already inferred]."
- "The [noun] was overwhelming / devastating / terrifying."

Replace with: physical behavior, object detail, truncated speech, or silence.

**The rule is not "never describe feeling." The rule is "never name it before the reader has felt it through the prose."** Once the reader has arrived at the emotion through image and action, a single restrained word can land it — but that word must come after, not instead of, the scene that earns it.

---

## Rule 6: Prose Density

**Every word must earn its place. The standard is Phil Klay, not a literary workshop.**

Compress:
- "He walked slowly across the room" → "He crossed the room."
- "She looked at him for a long moment before speaking" → "She looked at him. Then:"
- "There was a kind of silence that fell over them" → "Silence."

Delete:
- Adverbs modifying strong verbs
- Qualifiers: "somewhat," "rather," "quite," "a little"
- Throat-clearing: "It was clear that," "It seemed as though," "One could see that"
- Double-nouns: "the sound of the explosion" → "the explosion"

---

## Rule 7: Translation Adaptation (when source is non-English)

When rewriting from a foreign-language source:

**Preserve:**
- Fragmented structure if the source uses it
- Military jargon — translate precisely, do not soften
- Proper nouns (place names, unit designations) — keep or transliterate
- Cultural specifics that have no English equivalent — keep with minimal clarification via context, not footnote

**Amplify (not invent):**
- Emotional subtext that loses force in direct translation — recover it through English craft moves
- Sentence rhythm — find English equivalents of the source's pacing signature

**Do not:**
- Domesticate the prose into comfortable American/British idiom
- Add explanation the source did not have
- Soften military or atrocity language to protect the reader
- Invent plot or dialogue not implied by the source

---

## Output Format

Produce the rewritten prose in full. No meta-commentary. No "Here is the rewrite:" preamble.

After the prose:

```
─── HANDOFF → 04-score ───
word_count: [N]
section_count: [N]
has_dialogue: [true/false]
primary_benchmark_applied: "[Author, Title]"
craft_moves_applied: ["[move]", "[move]", "[move]"]
known_weaknesses: ["[anything you suspect is below standard]"]
```
