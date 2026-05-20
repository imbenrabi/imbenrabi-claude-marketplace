# english-storyteller

Production-grade English literary prose framework. A narrative operating system — not a single prompt — built from cooperating skills, agents, and commands that enforce literary-native writing, persistent story continuity, reference-anchored style, and character voice consistency across long-form projects.

---

## What This Is

LLMs generate English that reads as competent content writing: adjective stacking, named emotions, uniform sentence rhythm, over-complete dialogue, and filter words that keep the reader at arm's length. This plugin fixes that.

The framework enforces:
- Reference-anchored style (comparable published works as the living benchmark)
- Register locking (narrator and each character maintain distinct, stable voices)
- AI pattern detection and surgical rewriting (filter words, named emotions, metronome rhythm, hedged prose)
- Persistent story continuity via a file-based story bible
- Character voice consistency across chapters via JSON voice profiles
- Quality-gated output (8-dimension QA scoring, all dimensions must reach 9/10 before delivery)

---

## Architecture

```
COMMANDS              → user entry points
  new-story           initialize story project
  new-chapter         plan + generate + QA-gate a chapter
  rewrite             humanize a passage
  check-canon         validate against story bible
  update-character    create or update character profiles
  find-references     identify comparable published works
  qa-calibrate        verify english-judge-agent against golden corpus

AGENTS                → orchestration layer
  story-orchestrator  main prose generator with 5-attempt iterate loop
  narrative-planner   structural planning (no prose output)
  character-memory    voice consistency enforcement
  canon-engine        read-only continuity validator
  english-judge       blind QA scorer (writer/judge separation)
  reference-finder    web-search agent for benchmark identification

SKILLS                → behavioral rules (auto-trigger)
  english-voice-engine    rhythm, register, POV, density enforcement
  english-humanizer       AI pattern detection and rewriting
  english-qa-validator    8-dimension quality scoring (all ≥ 9 to pass)
  scene-composer          in-media-res, subtext, cinematic pacing
  reference-finder        comparable works identification

PIPELINE SKILLS       → numbered sequential workflow
  01-ingest.md        read source material, extract signals
  02-reference-finder.md  find 3–5 comparable published works
  03-rewrite.md       apply benchmark style to source
  04-score.md         run the 8-dimension rubric
  05-iterate.md       iterate until all dimensions ≥ 9

STORY BIBLE           → file-based persistent store
  .story-bible/
    characters/*.json   voice profiles, psychology, relationships
    timeline.json       event sequence, causality, whereabouts
    world.json          locations, rules, atmosphere
    relationships.json  alliances, tensions, knowledge gaps
    tone-profile.json   active tone preset + parameter overrides
    references.json     identified benchmark works for this project
    chapters/*.json     per-chapter memory, open threads
```

---

## Skills

| Skill | Triggers on | What it enforces |
|-------|-------------|-----------------|
| `english-voice-engine` | Writing prose, dialogue, narrative | Rhythm, register lock, filter-word suppression, POV consistency, prose density |
| `english-humanizer` | Rewrite, polish, fix, humanize | AI pattern removal, named-emotion deletion, dialogue fragmentation, filter word deletion |
| `english-qa-validator` | Quality check, score, validate | 8-dimension scoring: rhythm, character voice, continuity, prose density, POV consistency, emotional register, show/tell, reference fidelity |
| `scene-composer` | Write scene, open scene, scene transition | In-media-res entry, sensory anchoring, subtext, pacing via sentence length |
| `reference-finder` | Find references, comparable works, benchmark style | Web search for published works; excerpt + technique annotation |

---

## Commands

| Command | Usage | What it does |
|---------|-------|-------------|
| `/english-storyteller:new-story` | `/new-story` | Scaffold `.story-bible/` with all JSON stores and tone preset |
| `/english-storyteller:new-chapter` | `/new-chapter` | Plan + generate + QA-gate a full chapter |
| `/english-storyteller:rewrite` | `/rewrite [text]` | Humanizer pass with flag count and change log |
| `/english-storyteller:check-canon` | `/check-canon chapters/03.md` | Validate file against story bible for continuity |
| `/english-storyteller:update-character` | `/update-character Maya` | Create or update character JSON store |
| `/english-storyteller:find-references` | `/find-references` | Identify 3–5 comparable published works for the active project |
| `/english-storyteller:qa-calibrate` | `/qa-calibrate` | Verify `english-judge-agent` against the golden corpus; report drift |

---

## Tone Presets

| Preset | Style | Benchmark Authors |
|--------|-------|-------------------|
| `military-realism` | Spare, tactile, declarative suppression | Tim O'Brien, Karl Marlantes |
| `dark-psychological` | Fractured, violence without comment, unreliable | Cormac McCarthy, Denis Johnson |
| `literary-minimalism` | Iceberg theory, flat affect, surface tension | Raymond Carver, Hemingway |
| `war-interiority` | Lyrical close-third, past-present layer | Kevin Powers, Phil Klay |
| `domestic-realism` | Precision, ordinary catastrophe, compression | Alice Munro, Richard Yates |
| `lyrical-literary` | Long rhythmic sentences, earned abstraction | Marilynne Robinson, Toni Morrison |
| `urban-grit` | Dialogue-heavy, street-level, class-conscious | Richard Price, George Pelecanos |
| `southern-gothic` | Grotesque, dark humor, regional specificity | Flannery O'Connor, Carson McCullers |
| `literary-thriller` | Paranoia, systems, cool precision | Don DeLillo, Patricia Highsmith |
| `historical-literary` | Period voice, research worn lightly | Hilary Mantel, Colm Tóibín |

---

## QA Scoring

All generated prose is scored on 8 dimensions before delivery. **All dimensions must reach 9/10.**

| Dimension | What it measures |
|-----------|----------------|
| Rhythm | Sentence length variation; breath landing; cadence audibility |
| Character Voice | Speech and thought distinct, consistent, could not belong to another character |
| Continuity | Timeline, object, and emotional-state consistency across sections |
| Prose Density | No filler; every sentence earns its place |
| POV Consistency | No unintentional head-hopping or register breaks |
| Emotional Register | Tone matches scene function; modulation is intentional |
| Show Don't Tell | Named emotions vs. enacted emotions ratio |
| Reference Fidelity | Prose reflects identified benchmark works' specific techniques |

**ALL_PASS** (all dimensions ≥ 9): delivered as-is.
**PARTIAL_PASS** (some dimensions ≥ 9, none < 7): show scores, offer targeted revision.
**FLAG** (any dimension 5–8): run targeted heuristic for that dimension; re-score.
**FAIL** (any dimension < 5): full humanizer pass runs automatically.

The iteration loop runs up to 5 attempts. If after 5 passes any dimension remains below 9,
the orchestrator returns the best attempt with a warning and explains which passage is the
specific blocker for that dimension.

---

## Reference Finder

The reference-finder skill solves the problem of stylistic anchoring. Rather than asking
the writer to specify "write like O'Brien," the skill:

1. Accepts genre tags, tone descriptors, and themes extracted from the source material
2. Searches for 5 candidate published works that match those signals
3. Returns a ranked list with: excerpt, technique annotations, relevance reasoning
4. Stores the result in `.story-bible/references.json`
5. The orchestrator and judge both load this for style anchoring and fidelity scoring

The `reference_fidelity` scoring dimension rewards prose that applies the specific
annotated techniques from the top-ranked works — not pastiche, but conscious craft borrowing.

---

## Eval Corpus

`eval/golden-corpus/` holds 18 hand-scored English passages spread across the 8 QA
dimensions and the PASS/FLAG/FAIL bands. `/english-storyteller:qa-calibrate` runs
`english-judge-agent` against the corpus and reports per-dimension MAE, verdict-band
agreement, and tolerance breaches. The judge must PASS calibration before it can be
trusted for production quality gating.

---

## AI Patterns Addressed

| Problem | Mitigation |
|---------|-----------|
| Named emotions | `english-humanizer` deletes "felt/realized/knew" clauses; replaces with action |
| Filter words | Deletes "she saw/heard/noticed" — puts reader directly in scene |
| Adverb stacking | Flags 2+ adverbs modifying same verb; forces concrete replacement |
| Uniform rhythm | Rhythm normalization; sentence length variance requirement |
| Over-complete dialogue | Fragmentation rules; non-answer modeling; silence as beat |
| Metronome opening | In-media-res entry enforced by scene-composer |
| Abstract noun clusters | Concrete grounding requirement before any abstraction |
| Hedged prose | Strips qualifiers ("kind of", "somewhat", "rather") |
| POV drift | POV consistency tracked per scene |
| Reference drift | Reference fidelity scoring holds prose to benchmark techniques |

---

## Weakness Analysis

1. **Reference search quality.** The reference-finder relies on web search. If the search
   returns low-quality results, the style anchor is weak. Always review `references.json`
   before generating prose.

2. **Reference fidelity is subjective.** Scoring whether prose "reflects" a benchmark
   is harder than scoring rhythm (which is measurable). The judge uses technique annotations
   as the target, which helps, but this dimension has higher variance than the others.

3. **9/10 threshold is demanding.** The all-≥-9 requirement can produce more iterations
   than necessary when a passage is publishable at 8/10 on one dimension. The threshold
   is intentionally strict; users can lower it via `.story-bible/tone-profile.json` override.

4. **Long-context drift.** Beyond ~100K tokens, agents may lose story bible details.
   Run `/check-canon` every 3–4 chapters.

5. **No real-time audio.** Rhythm scoring is heuristic — the system counts syllables and
   measures sentence length but cannot "hear" the prose. Human editorial ear remains essential.

---

## Future Improvements

- **Comparative stylometry**: score proximity to a target author's measurable style
- **Dialect and diction tracking**: mark anachronistic word choices in historical fiction
- **Multi-POV register manager**: track narrator shifts across POV chapters
- **Scene graph visualization**: timeline and relationship graph as visual output
- **Dialogue overlap simulation**: model interruption and simultaneous speech precisely
- **Regional voice profiles**: American South, Boston, rural Midwest, UK class registers
