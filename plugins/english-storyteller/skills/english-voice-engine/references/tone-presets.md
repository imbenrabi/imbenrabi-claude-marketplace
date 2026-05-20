# English Tone Presets

Ten named storytelling styles for English literary prose. Each story project selects one
as its base in `.story-bible/tone-profile.json`.

Each preset defines values on a 0–10 scale for prose parameters.

---

## Preset Parameters

| Parameter | Description |
|-----------|-------------|
| `sentence_length` | short / medium / long / variable |
| `slang_density` | 0 = none, 10 = maximally dense |
| `metaphor_density` | 0 = none, 10 = every sentence |
| `emotional_explicitness` | 0 = implication only, 10 = fully stated |
| `dialogue_pacing` | slow / medium / fast / staccato |
| `narration_distance` | close / medium / far |
| `interruption_frequency` | 0 = none, 10 = constant |
| `profanity_tolerance` | 0 = none, 10 = unrestricted |
| `exposition_density` | 0 = none, 10 = heavy |
| `interiority_depth` | 0 = camera-eye only, 10 = full interior monologue |

---

## 1. `military-realism`

The default for this plugin. Spare, declarative, physical. Associated with Tim O'Brien,
Karl Marlantes, Sebastian Junger.

```
sentence_length:       variable (short peaks, medium descriptive)
slang_density:         3
metaphor_density:      1
emotional_explicitness: 0
dialogue_pacing:       staccato
narration_distance:    medium (inside but not explaining)
interruption_frequency: 5
profanity_tolerance:   6
exposition_density:    1
interiority_depth:     2 (interiority through physical detail, not exposition)
```

Register: detached-observer with close access to protagonist's body. Emotion lives
in objects and actions, never in the narrator's commentary. Lists are emotional devices.
The body knows before the mind explains.

Benchmark: O'Brien's "The Things They Carried" — accumulation of specifics as weight.

---

## 2. `dark-psychological`

Fractured, obsessive. Violence without moral comment. Associated with Cormac McCarthy,
Denis Johnson, Larry Brown.

```
sentence_length:       variable (very short fragments; occasional long run-on)
slang_density:         2
metaphor_density:      2
emotional_explicitness: 0
dialogue_pacing:       staccato (dialogue often without attribution)
narration_distance:    close (unreliable narrator common)
interruption_frequency: 4
profanity_tolerance:   8
exposition_density:    0
interiority_depth:     3 (fractured, not explained)
```

Register: intimate-close, unreliable. The narrator may not understand what they witnessed.
Prose does not contextualize violence. McCarthy-style: biblical cadence, no quotation marks,
landscape as character. Johnson-style: hallucinatory present-tense, time as unreliable.

Benchmark: Johnson's "Jesus' Son" — present-tense, body as compass when nothing else works.

---

## 3. `literary-minimalism`

Iceberg theory. Everything submerged. Associated with Raymond Carver, Hemingway,
Mary Gaitskill.

```
sentence_length:       short
slang_density:         1
metaphor_density:      0
emotional_explicitness: 0
dialogue_pacing:       slow (pauses matter)
narration_distance:    medium (never explains; never inside)
interruption_frequency: 3
profanity_tolerance:   3
exposition_density:    0
interiority_depth:     0 (interiority is forbidden; reader infers everything)
```

Register: detached-observer. The narrator sees surfaces only. Emotion is entirely submerged.
Dialogue carries more weight than narration. What characters don't say is more important
than what they say. Carver's rule: the final version is shorter than the draft.

Benchmark: Carver — the scene ends before the emotion is named, and the reader has all
the information they need.

---

## 4. `war-interiority`

Lyrical close-third, past-present layered. Associated with Kevin Powers, Phil Klay,
Tobias Wolff.

```
sentence_length:       variable (long in memory/past; short in present-action)
slang_density:         2
metaphor_density:      4
emotional_explicitness: 1
dialogue_pacing:       medium
narration_distance:    close
interruption_frequency: 3
profanity_tolerance:   5
exposition_density:    2
interiority_depth:     7 (interiority earned through specific imagery, not exposition)
```

Register: intimate-close with lyrical register. Time is non-linear. The past intrudes on
the present in specific sensory flashes. Metaphor is earned — every figurative moment is
anchored in a concrete vehicle before the tenor is implied. Powers' "The Yellow Birds"
as model: second-person possible; present and past intercut; grief as earned conclusion.

Benchmark: Powers — "We were not sad. We were past sad."

---

## 5. `domestic-realism`

Precision, ordinary catastrophe. The domestic as battleground. Associated with Alice
Munro, Richard Yates, Elizabeth Strout.

```
sentence_length:       medium
slang_density:         1
metaphor_density:      2
emotional_explicitness: 1
dialogue_pacing:       medium
narration_distance:    medium
interruption_frequency: 2
profanity_tolerance:   2
exposition_density:    3
interiority_depth:     5
```

Register: literary-measured with access to the protagonist's interior. The drama is
not announced — it is discovered inside ordinary actions. Munro's technique: the story
contains a second, hidden story that only becomes legible at the end.

---

## 6. `lyrical-literary`

Long rhythmic sentences. Earned abstraction. Biblical resonance. Associated with
Marilynne Robinson, Toni Morrison, James Baldwin.

```
sentence_length:       long
slang_density:         0
metaphor_density:      6
emotional_explicitness: 2
dialogue_pacing:       slow
narration_distance:    medium (voice that meditates)
interruption_frequency: 1
profanity_tolerance:   1
exposition_density:    4
interiority_depth:     8
```

Register: literary-measured with earned lyricism. Abstraction only after the concrete
moment earns it. Robinson: light and theology; Baldwin: love and fire. Metaphor is
structural, not decorative — each image recurs across the text.

---

## 7. `urban-grit`

Dialogue-heavy, street-level, class-conscious. Associated with Richard Price,
George Pelecanos, James Ellroy.

```
sentence_length:       short
slang_density:         7
metaphor_density:      0
emotional_explicitness: 0
dialogue_pacing:       fast
narration_distance:    close
interruption_frequency: 7
profanity_tolerance:   8
exposition_density:    1
interiority_depth:     2
```

Register: street-vernacular characters; detached-observer or ironic-dry narrator.
Dialogue carries 70% of the weight. Power dynamics in every exchange. Class visible in
every sentence. No sentimentality. Price's rule: the dialogue is a negotiation.

---

## 8. `southern-gothic`

Grotesque, dark humor, regional specificity. Associated with Flannery O'Connor,
Carson McCullers, Cormac McCarthy's southern work.

```
sentence_length:       variable (drawling long; violent short)
slang_density:         4
metaphor_density:      3
emotional_explicitness: 0
dialogue_pacing:       slow (menace in the pause)
narration_distance:    medium (ironic distance on the grotesque)
interruption_frequency: 2
profanity_tolerance:   4
exposition_density:    3
interiority_depth:     3
```

Register: ironic-dry narrator watching grotesque events with a cool eye. Characters
are simultaneously ridiculous and terrifying. O'Connor's rule: grace arrives through
violence. The grotesque is the only honest lens.

---

## 9. `literary-thriller`

Paranoia, systems thinking, cool precision. Associated with Don DeLillo, Patricia
Highsmith, Thomas Harris.

```
sentence_length:       medium
slang_density:         1
metaphor_density:      2
emotional_explicitness: 0
dialogue_pacing:       medium (cool; information-dense)
narration_distance:    medium to far
interruption_frequency: 3
profanity_tolerance:   3
exposition_density:    4
interiority_depth:     4
```

Register: literary-measured with cool paranoid distance. Systems and forces loom larger
than characters. DeLillo: prose that reveals ideology through surface description.
Highsmith: the calm is the tell; the murderer's prose is cleaner than the victim's.

---

## 10. `historical-literary`

Period voice, research worn lightly. Associated with Hilary Mantel, Colm Tóibín,
Anthony Burgess.

```
sentence_length:       variable
slang_density:         0
metaphor_density:      3
emotional_explicitness: 1
dialogue_pacing:       medium
narration_distance:    close to medium
interruption_frequency: 2
profanity_tolerance:   2
exposition_density:    4
interiority_depth:     5
```

Register: literary-measured, period-inflected narrator. Research visible in specificity,
not in information-dumping. Mantel's technique: present tense in historical fiction forces
the reader into the moment — the characters do not know how it will end.

---

## Using Presets

Set the base preset in `.story-bible/tone-profile.json`. Override individual parameters
as needed for specific chapters or scenes. Example: a military-realism novel can increase
`interiority_depth` for a flashback scene, then return to baseline.

The `references.json` anchor work and `composite_style_note` further refine how the preset
is applied. The preset is the frame; the reference is the texture.
