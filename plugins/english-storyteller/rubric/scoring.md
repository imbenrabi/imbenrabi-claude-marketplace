# English QA Scoring Rubric

Full rubric with score-level descriptors for all 8 dimensions. Pass threshold: all
applicable dimensions ≥ 9/10.

---

## How Scoring Works

Each dimension is scored 0–10 using integer values. Intermediate scores are permitted.
The rubric provides anchors at 2, 5, 8, and 10; interpolate for values between anchors.

**Pass threshold:** ALL applicable dimensions ≥ 9/10. The average alone is not
sufficient — a passage with seven 10s and one 4 does not pass.

**Applicable dimensions:**
- All 8 are applicable by default
- `reference_fidelity` is N/A when `.story-bible/references.json` does not exist
- Exclude N/A dimensions from the average calculation

**Verdict mapping:**
| Condition | Verdict |
|---|---|
| All applicable dimensions ≥ 9 | ALL_PASS |
| All applicable dimensions ≥ 7, some < 9 | PARTIAL_PASS |
| Any applicable dimension 5–8 | FLAG |
| Any applicable dimension < 5 | FAIL |

---

## Dimension 1: Rhythm

**What it measures:** sentence length variation, stress patterns, where breath lands.
Tests whether cadence is audible and whether variation is purposeful (not random).

| Score | What it looks like |
|---|---|
| 0–2 | Complete metronome: all sentences the same length. OR all sentences very long with no variation. No cadence, no breath point. |
| 3–4 | Some variation but mechanical: one long sentence among many short, or vice versa. Pattern is noticeable and feels like a formula. |
| 5–6 | Real variation in sentence length, but not clearly purposeful. The long sentences don't land at interiority; the short sentences don't land at peaks. Rhythm varies without meaning. |
| 7–8 | Variation is mostly purposeful. Short sentences land at tension peaks. Long sentences carry memory or accumulation. One or two instances where rhythm could be better calibrated. |
| 9 | Rhythm is consistently purposeful. Short sentences mark peaks. Long sentences mark interiority or accumulation. Fragments appear where emotional compression is needed. The reader can hear the cadence. |
| 10 | Rhythm is itself a meaning-making device. The pattern and the breaks in pattern are both structural. The shortest sentence in the passage is the emotionally densest. Silence is built into the rhythm. |

**Exception:** `literary-minimalism` preset allows intentional rhythm monotony.
Score 9–10 for intentionally uniform short sentences if the preset is `literary-minimalism`.

---

## Dimension 2: Character Voice

**What it measures:** whether each character's speech and thought is distinct from other
characters and from the narrator. Tests idiolect consistency across the passage.

| Score | What it looks like |
|---|---|
| 0–2 | All characters sound identical. Dialogue could be reassigned to any character without loss. Narrator's register bleeds into character speech. |
| 3–4 | Characters are distinguishable by content but not register. One character talks about different things but in the same syntactic structure as another. |
| 5–6 | Characters are distinguishable but drift. One character has a clear voice but another is generic. Voice holds in some moments and slips in others. |
| 7–8 | Voices are distinct and mostly consistent. One character has a clear idiolect; another is slightly generic. Dialogue tags could almost be removed but not quite. |
| 9 | Voices are distinct and consistent. Given any line of dialogue, you could identify the speaker without the dialogue tag. Internal thought also varies by character. |
| 10 | Voices are unmistakable. Each character's speech and thought could not belong to another character. The voice is structural, not decorative — it reveals the character's psychology in its form, not just its content. |

---

## Dimension 3: Continuity

**What it measures:** timeline, object, and emotional-state consistency within and across
sections. Tests whether the physical and psychological world is internally consistent.

| Score | What it looks like |
|---|---|
| 0–2 | Explicit contradictions that break story logic: character in two places at once, object appears that was left elsewhere, character knows something they haven't been told. |
| 3–4 | Multiple minor inconsistencies that cumulatively undermine trust in the narrator or the world. Objects appear and disappear. Emotional arc is unclear. |
| 5–6 | Mostly consistent with one notable inconsistency the reader has to work to explain. Time jumps are sometimes handled, sometimes ignored. |
| 7–8 | Consistent with minor ambiguities. All objects and locations track. Emotional arc is clear and causal. One moment where the passage of time is ambiguous. |
| 9 | No inconsistencies. Every named object has a consistent position. Every character knows only what they've been told or observed. Emotional arc follows causality. Time jumps handled explicitly. |
| 10 | Airtight continuity that creates trust. The reader can rely on every detail. Physical and emotional continuity reinforce each other — consistency is not just absence of error but presence of craft. |

---

## Dimension 4: Prose Density

**What it measures:** the ratio of load-bearing sentences to filler, redundancy, and
throat-clearing. Tests whether every sentence survives the deletion test.

| Score | What it looks like |
|---|---|
| 0–2 | Heavily padded. Multiple sentences that restate the same fact. Transitional sentences that carry no information. Context-setting that was unnecessary. The passage could be cut by 40% without content loss. |
| 3–4 | Some padding. A few sentences that explain what was already shown. Transitions that connect things the reader would have connected anyway. Could be cut by 20% without loss. |
| 5–6 | Mostly lean with some redundancy. One or two sentences that could be cut. The narrative moves but some steps are explained when they could be implied. |
| 7–8 | Lean prose. Most sentences carry their weight. One sentence that could be cut; one transition that could be implied. |
| 9 | Every sentence survives the deletion test. Nothing can be cut without information loss. The passage is as short as it needs to be and as long as it must be. |
| 10 | Surgical prose. Every word earns its place, not just every sentence. The compression creates pressure — the reader feels the weight of what is not said as much as what is. |

---

## Dimension 5: POV Consistency

**What it measures:** whether the narrative perspective holds without unintentional
head-hopping, register breaks, or knowing things the POV character cannot know.

| Score | What it looks like |
|---|---|
| 0–2 | Frequent unintentional perspective shifts. Omniscient commentary in a close-third passage. Access to another character's thoughts without attribution. The narrator knows things the POV character cannot know. |
| 3–4 | Perspective mostly held but with multiple drifts. The reader sometimes loses track of whose consciousness they are in. One clear instance of knowing what cannot be known. |
| 5–6 | Perspective holds for most of the passage with one significant drift. The narrator temporarily accesses another character's mind or knows something the POV character hasn't been told. |
| 7–8 | Perspective mostly locked. One ambiguous moment that could be read as intentional (free indirect discourse) or as drift. The POV character's limits are respected. |
| 9 | Perspective locked throughout. The narrator knows only what the POV character senses, thinks, or is told. Any ambiguity is clearly free indirect discourse, not unintentional drift. |
| 10 | Perspective is a craft tool. The limits of what the POV character knows are themselves meaningful — what the character doesn't know creates tension, irony, or surprise. The constraint is structural. |

---

## Dimension 6: Emotional Register

**What it measures:** whether the emotional temperature matches the scene's function and
whether modulation is intentional. Tests tonal coherence.

| Score | What it looks like |
|---|---|
| 0–2 | Tonally incoherent. Comic and tragic registers collide without irony. A grief scene has a joke that deflates rather than complicates. Warm narration in a cold scene. The emotional temperature has no relationship to the scene's purpose. |
| 3–4 | Mostly one register with several accidental breaks. A war scene with pastoral beauty that hasn't been earned. A domestic scene with sudden violence that hasn't been set up. Register breaks feel like inconsistency, not irony. |
| 5–6 | Mostly consistent register with one accidental break. One sentence that is warmer or colder than the surrounding prose. The break feels like a word choice error, not a structural decision. |
| 7–8 | Consistent register with intentional modulation at one or two moments. The tonal shift at the modulation point is earned. The reader feels the shift as meaningful. |
| 9 | Emotional register calibrated to every beat. The temperature rises and falls with the scene's structure. Every shift is intentional; the reader understands the shift as purposeful even when they can't explain it. |
| 10 | Register is a structural tool. The gap between what is described and how it is described creates meaning (irony, dread, dark comedy). The emotional temperature is itself an argument. |

---

## Dimension 7: Show Don't Tell

**What it measures:** the ratio of named emotions to enacted emotions. Tests whether
affect is implied through behavior and detail or stated directly.

| Score | What it looks like |
|---|---|
| 0–2 | Predominantly told. Emotional states named directly: "she was devastated", "he felt guilty", "they were both sad". No enacted equivalents. The reader is told rather than shown. |
| 3–4 | Mixture of told and shown, with told predominating. Some enacted behavior but each enacted moment followed by a named emotion that renders it redundant. |
| 5–6 | Mostly shown, with some telling. 2–3 instances of named emotion that survive the deletion test only because the enacted equivalent is absent. The showing is inconsistent. |
| 7–8 | Mostly shown. One or two named emotions that could be replaced with enacted equivalents. The enacted moments are specific and non-generic. |
| 9 | No named emotions (or extremely rare, and earned when they appear). All affect is implied through action, behavior, physical response, or dialogue. The reader constructs the emotional meaning without being told. |
| 10 | The enacted behavior is so specific and unexpected that the reader makes the emotional reconstruction with a sense of discovery. The physical detail is not the obvious first symptom of the emotion but the second or third — the one the character might not recognize as being about the emotion. |

---

## Dimension 8: Reference Fidelity

**What it measures:** whether the prose applies the specific techniques from the anchor
work's annotations. N/A if no `references.json` exists.

| Score | What it looks like |
|---|---|
| 0–2 | No correspondence to benchmark. Prose is generic literary fiction with no trace of the anchor work's specific techniques. The reference might as well not exist. |
| 3–4 | Surface correspondence. The prose acknowledges the genre/tone of the anchor work but doesn't apply the specific annotated techniques. Reads like an author who has heard of O'Brien but hasn't read him carefully. |
| 5–6 | One annotated technique present but underdeveloped. The form is there — accumulation, for example — but it doesn't accumulate to the weight the technique requires. |
| 7–8 | One or two annotated techniques clearly present and functioning. The prose shows signs of having been written in conversation with the anchor work. |
| 9 | Two or three annotated techniques clearly present, functioning as the anchor uses them, and integrated naturally into the prose. A reader of the anchor work would recognize the kinship. |
| 10 | All three annotated techniques present, applied with precision and adapted to this specific story's content. The prose is in conversation with the anchor work but is not imitation. It knows what the technique is for and uses it for that purpose. |

---

## Override Notes

**`literary-minimalism` preset:** intentional rhythm monotony is permitted. Do NOT
penalize score 9 or 10 for uniform short sentences in this preset. The monotone is
the style.

**Formal register:** do not penalize formal register as AI. Some characters and narrators
are legitimately formal. Score register *consistency*, not register *level*.

**Short passages:** do not penalize a short passage for lacking content it doesn't claim
to have. Score what is present; do not deduct for what is absent.
