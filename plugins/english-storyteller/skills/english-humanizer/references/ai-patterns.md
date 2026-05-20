# English AI Pattern Inventory

Red flags for AI-generated English literary prose. Weighted score ≥ 3 = mandatory rewrite.

Each pattern: what it looks like, severity, why it signals AI, which dimension it threatens.

---

## HIGH Severity (weight: 3 each)

---

### H1: Filter Words

**Pattern:** Narrator interposes a character's perception between the reader and the
scene. "She saw", "he noticed", "she heard", "he felt [abstract state]", "she realized".

**Example (flagged):**
> "She noticed that the room had gone quiet. He heard a sound from the hallway."

**Why AI:** AI defaults to reporting perception rather than rendering it. Literary prose
puts the reader in the scene directly. Every filter word creates a layer of narrative
distance where there should be immediacy.

**Dimensions threatened:** POV Consistency, Show Don't Tell, Prose Density

**Detection:** any instance of [she/he/they] + [saw/noticed/heard/felt/realized/understood/knew/sensed/thought] acting as an
epistemic filter on scene content. Count each instance.

---

### H2: Named Emotions

**Pattern:** Direct statement of a character's emotional state as narrated fact.

**Example (flagged):**
> "She felt profound sadness. He was overcome with guilt. She experienced a surge of hope."

**Why AI:** AI labels emotions because it knows what a character should feel. Literary
prose shows emotion through the body and behavior and lets the reader name it.

**Dimensions threatened:** Show Don't Tell, Emotional Register, Reference Fidelity

**Detection:** any of: [she/he/they] + [felt/was/experienced/had] + [abstract emotional noun or adjective].
Each instance = 1 flag.

---

### H3: Uniform Rhythm

**Pattern:** 4+ consecutive sentences of approximately equal length (within 30% of each
other in word count), or 4+ consecutive sentences beginning with the same grammatical subject.

**Example (flagged):**
> "He walked to the window. He looked outside. He saw the street. He went back."

**Why AI:** AI generates paragraph-length chunks of semantically uniform output. Literary
prose has cadence — variation in sentence length is structural, not decorative.

**Dimensions threatened:** Rhythm

**Detection:** count sentence lengths in words for each 200-word window. Flag if 4+ sentences
are within 30% of each other. Also count subject-position starts. ≥ 4 same subject = flag.

---

### H4: Adverb Stacking

**Pattern:** 2+ adverbs modifying the same verb, or adverb + intensifier + adjective chain.

**Example (flagged):**
> "She spoke very softly and quietly, her voice barely audible."

**Why AI:** AI defaults to adverbs when it wants precision but uses weak verbs. Literary
prose finds the single precise verb that doesn't need adverbial support.

**Dimensions threatened:** Prose Density, Rhythm

**Detection:** flag any [verb] + [2+ adverbs] or [very/really/extremely/incredibly] + adjective.

---

### H5: Emotional Exposition (Over-Explanation)

**Pattern:** Narrator explains the significance of an event, action, or line of dialogue
that the reader can interpret without help.

**Example (flagged):**
> "She slammed the door. This showed how angry she was. He understood that she was
> finished with the conversation."

**Why AI:** AI does not trust the reader to make inferences. Literary prose trusts action
to carry meaning. The explanation after the action is always redundant.

**Dimensions threatened:** Show Don't Tell, Prose Density

**Detection:** any sentence beginning with "This showed/revealed/meant/demonstrated" or
any explanatory sentence following a physical action.

---

## MEDIUM Severity (weight: 1.5 each)

---

### M1: Hedged Qualifiers

**Pattern:** Softening language that signals a lack of commitment: "kind of", "somewhat",
"rather", "a bit", "fairly", "pretty much", "almost as if", "seemed to", "appeared to".

**Example (flagged):**
> "The room seemed rather empty. She felt somewhat sad. He was kind of angry."

**Why AI:** AI hedges to avoid being wrong. Literary prose commits. If a room is empty,
say so. If a character is sad, show it without hedging.

**Dimensions threatened:** Prose Density, Emotional Register

---

### M2: Over-Complete Dialogue

**Pattern:** Every dialogue turn is a complete grammatical sentence that directly answers
the previous turn.

**Example (flagged):**
> "Where were you?" she asked.
> "I was at the office working late because of the Henderson account," he explained.

**Why AI:** AI generates dialogue as information exchange. Real dialogue is deflection,
interruption, implication, and non-answer.

**Dimensions threatened:** Character Voice, Emotional Register

---

### M3: Context-Setting Opening

**Pattern:** Scene begins with time-of-day, weather, or establishing description before
any action.

**Example (flagged):**
> "It was a gray Tuesday morning in November. Sarah was sitting at the kitchen table,
> thinking about the meeting earlier that week."

**Why AI:** AI defaults to orientation before event. Literary prose enters in the middle
of something.

**Dimensions threatened:** Rhythm, Prose Density

---

### M4: Symmetrical Prose

**Pattern:** Two consecutive sentences with nearly identical grammatical structure
(often with paired opposites).

**Example (flagged):**
> "He loved her but he feared her. She needed him but she resented him."

**Why AI:** AI reaches for parallel structure as a literary gesture. Literary prose breaks
symmetry — the asymmetry is where the truth is.

**Dimensions threatened:** Rhythm, Character Voice

---

### M5: Abstract Noun Clusters

**Pattern:** 3+ abstract nouns (truth/beauty/grief/memory/love/time/silence/darkness)
within 40 words without a concrete anchor image.

**Example (flagged):**
> "It was a moment of truth and grief, when time seemed to hold its breath and silence
> filled the space between love and loss."

**Why AI:** AI confuses abstraction with literary depth. Literary depth is earned through
the concrete detail that makes abstraction necessary.

**Dimensions threatened:** Prose Density, Reference Fidelity

---

## LOW Severity (weight: 0.5 each)

---

### L1: Passive Distancing

**Pattern:** Passive constructions used where active would be stronger.

**Example:** "The door was closed by him" instead of "He closed the door."

Use passive deliberately (when the actor is unknown or unimportant). Flag if passive
appears more than 2 times per 200 words without clear craft reason.

---

### L2: Perfect Grammar Under Stress

**Pattern:** Dialogue in high-emotion scenes is grammatically complete and precise.

Real speech under stress drops words, contradicts itself, trails off. Flawless grammar
in confrontation scenes signals AI production.

---

### L3: Repetitive Sentence Openers

**Pattern:** 3+ consecutive sentences beginning with the same word (especially "The"
or a character's name).

**Example:** "The house was quiet. The light was failing. The day was almost over."

---

### L4: Dialogue Tag Overuse

**Pattern:** Every line of dialogue has a dialogue tag; no beats substituted.

**Example:** "Come in," he said. "Thank you," she replied. "Sit down," he told her.

Replace half with physical beats. Dialogue tag: "she said". Beat: she put down her glass.

---

## Counting Instructions

- Each instance of a pattern = 1 flag
- Count across the full submitted passage
- Total: HIGH × 3 + MEDIUM × 1.5 + LOW × 0.5
- Weighted total ≥ 3 = mandatory rewrite pass
- Weighted total ≥ 9 = full humanizer pass required before output
