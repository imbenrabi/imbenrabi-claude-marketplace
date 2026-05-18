# Tone Presets

Ten named storytelling styles for Hebrew prose. Each story project selects one as its base in `tone-profile.json`.

Each preset defines values on a 0–10 scale for prose parameters. These values guide the hebrew-voice-engine and hebrew-humanizer skills.

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

---

## 1. `contemporary-israeli-realism`

The default. Works for most modern Israeli fiction set in everyday life.

```
sentence_length:       medium
slang_density:         2
metaphor_density:      3
emotional_explicitness: 1
dialogue_pacing:       fast
narration_distance:    close
interruption_frequency: 4
profanity_tolerance:   3
exposition_density:    2
```

Register: everyday-conversational narrator. Characters vary. Subtext preferred over exposition. Tel Aviv default.

---

## 2. `minimalist-literary`

Sparse prose. Every word justified. Associated with Etgar Keret-style flash fiction and Yoel Hoffmann influence.

```
sentence_length:       short
slang_density:         0
metaphor_density:      1
emotional_explicitness: 0
dialogue_pacing:       slow
narration_distance:    medium
interruption_frequency: 1
profanity_tolerance:   0
exposition_density:    0
```

Register: literary-measured narrator. Almost no adjectives. Sentence fragments as full beats. Silence preferred over description.

---

## 3. `dark-psychological`

Long interior prose. Associated with psychological literary fiction, unreliable narrators, obsessive interiority.

```
sentence_length:       long
slang_density:         0
metaphor_density:      3
emotional_explicitness: 0
dialogue_pacing:       slow
narration_distance:    close
interruption_frequency: 2
profanity_tolerance:   1
exposition_density:    3
```

Register: intimate-close. Narrator deep inside protagonist's consciousness. Second-person monologue allowed. Long run-on sentences to mirror fragmented thought.

---

## 4. `sarcastic-urban`

Ironic, fast, post-modern. Associated with Israeli comedy writing, satirical fiction, urban short stories.

```
sentence_length:       short
slang_density:         8
metaphor_density:      0
emotional_explicitness: 0
dialogue_pacing:       staccato
narration_distance:    far
interruption_frequency: 7
profanity_tolerance:   6
exposition_density:    1
```

Register: detached-ironic narrator. Every emotional moment deflected or undercut. Characters speak mostly in incomplete sentences. Straight-faced delivery of absurd content.

---

## 5. `intimate-first-person`

Confessional first-person. Close, warm, reflective. Common in memoir-style fiction and coming-of-age stories.

```
sentence_length:       variable
slang_density:         2
metaphor_density:      4
emotional_explicitness: 3
dialogue_pacing:       medium
narration_distance:    close
interruption_frequency: 3
profanity_tolerance:   2
exposition_density:    3
```

Register: intimate-close. Narrator speaks directly to the reader. Occasional second-person address ("ואתם, אם הייתם שם..."). Memory and present interweave.

---

## 6. `cinematic-thriller`

Present-tense, action-forward. Minimal interiority. Associated with suspense, crime, political thrillers.

```
sentence_length:       short
slang_density:         1
metaphor_density:      0
emotional_explicitness: 0
dialogue_pacing:       staccato
narration_distance:    far
interruption_frequency: 5
profanity_tolerance:   3
exposition_density:    0
```

Register: detached-observer. Camera-eye narration. No emotional explanation. Present tense throughout. Scene cuts are abrupt. Dialogue is functional, not expressive.

---

## 7. `melancholic-literary`

Slow, lyrical, reflective. Associated with Amos Oz-era Israeli prose and Mediterranean nostalgia.

```
sentence_length:       long
slang_density:         0
metaphor_density:      5
emotional_explicitness: 2
dialogue_pacing:       slow
narration_distance:    medium
interruption_frequency: 1
profanity_tolerance:   0
exposition_density:    4
```

Register: literary-measured. Occasional biblical echo (used sparingly, not ironically). Extended sensory description. Past tense. Melancholy without melodrama.

---

## 8. `military-realism`

Gritty, operational, terse. Associated with Israeli military fiction, combat memoirs, security service drama.

```
sentence_length:       short
slang_density:         3
metaphor_density:      0
emotional_explicitness: 0
dialogue_pacing:       staccato
narration_distance:    far
interruption_frequency: 6
profanity_tolerance:   7
exposition_density:    0
```

Register: detached-observer with military-register characters. No emotional labor. Physical reality only. Silence where a civilian narrator would explain. Characters use military register locked.

---

## 9. `tel-aviv-conversational`

Fast, multilingual, present-oriented. The city as background noise. Associated with contemporary Tel Aviv social fiction.

```
sentence_length:       short
slang_density:         7
metaphor_density:      1
emotional_explicitness: 1
dialogue_pacing:       fast
narration_distance:    close
interruption_frequency: 6
profanity_tolerance:   5
exposition_density:    1
```

Register: slang-dense narrator with code-switching. English loanwords used naturally. Dialogue overlaps. Characters talk past each other. The city's rhythm is the pace.

---

## 10. `jerusalem-introspective`

Slower, more weighted. History presses into everyday scenes. Associated with Jerusalem-set literary fiction.

```
sentence_length:       variable
slang_density:         1
metaphor_density:      4
emotional_explicitness: 2
dialogue_pacing:       medium
narration_distance:    medium
interruption_frequency: 2
profanity_tolerance:   1
exposition_density:    3
```

Register: literary-measured with historical resonance. Occasional Arabic loanword in appropriate character dialogue. Past and present layer. Place described with weight. Introspective but not navel-gazing.

---

## Using Presets

Set the base preset in `.story-bible/tone-profile.json`. Override individual parameters as needed for specific chapters or scenes.

When a scene requires a register departure (e.g., a military scene in an otherwise realist novel), override `narration_distance` and `slang_density` for that scene only, then return to the base.
