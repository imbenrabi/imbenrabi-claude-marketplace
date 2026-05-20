# english-storyteller — Implementation Plan

## 1. Plugin Architecture

### Overview

The plugin is a narrative operating system for English literary fiction, modeled on
`hebrew-storyteller` but with three meaningful additions: a reference-finder capability,
an 8-dimension quality rubric (vs. Hebrew's 5), and an all-dimensions-≥-9 pass threshold
(vs. Hebrew's average ≥ 7.0).

### Component Map

```
COMMANDS              → user entry points
  new-story           initialize story project with .story-bible/
  new-chapter         plan + generate + QA-gate a full chapter
  rewrite             humanize a passage using english-humanizer
  check-canon         validate file against story bible
  update-character    create or update character JSON voice profiles
  find-references     standalone: search for comparable published works
  qa-calibrate        verify english-judge-agent against golden corpus

AGENTS                → orchestration layer
  story-orchestrator  main prose generator with 5-attempt iterate loop
  narrative-planner   structural planning (no prose output)
  character-memory    voice consistency enforcement
  canon-engine        read-only continuity validator
  english-judge       blind QA scorer (writer/judge separation)
  reference-finder    web-search agent that identifies comparable works

SKILLS                → behavioral rules (subdirectory + SKILL.md)
  english-voice-engine    prose rules: rhythm, register, POV, density
  english-humanizer       AI pattern detection and rewriting
  english-qa-validator    8-dimension quality scoring
  scene-composer          in-media-res, subtext, cinematic pacing
  reference-finder        identify benchmark works from genre/tone signals

PIPELINE SKILLS       → numbered sequential workflow
  01-ingest.md        extract genre/tone/theme signals from source
  02-reference-finder.md  find 3–5 comparable published works
  03-rewrite.md       apply benchmark style to source material
  04-score.md         run the 8-dimension rubric
  05-iterate.md       iterate per-dimension fixes until all ≥ 9

STORY BIBLE           → file-based persistent store
  .story-bible/
    characters/*.json   voice profiles, psychology, relationships
    timeline.json       event sequence, causality, whereabouts
    world.json          locations, rules, atmosphere
    relationships.json  alliances, tensions, knowledge gaps
    tone-profile.json   active tone preset + reference-works anchor
    chapters/*.json     per-chapter memory, open threads
    references.json     identified benchmark works for this project

CORPUS                → style benchmark excerpts
  corpus/
    01-military-realism.md      O'Brien: accumulation as emotion
    02-dark-psychology.md       Johnson: fractured time, violence without comment
    03-war-interiority.md       Powers: lyrical close-third, time dislocation
    04-literary-minimalism.md   Carver: iceberg, surface tension
    05-combat-closeup.md        Marlantes: technical precision, body as text

EVAL                  → judge calibration
  eval/golden-corpus/
    index.json          18+ hand-scored passages with tolerances
    [dimension]/*.md    passages anchored to PASS/FLAG/FAIL bands
```

### How the Components Chain

1. User invokes `/english-storyteller:new-chapter`
2. Command loads story bible → delegates structure to `narrative-planner-agent`
3. For each scene: `story-orchestrator-agent` runs the write-judge-iterate loop:
   a. Loads `references.json` to anchor benchmark style
   b. Generates prose using `english-voice-engine` + `scene-composer` rules
   c. Runs `english-humanizer` self-review pass
   d. Delegates to `english-judge-agent` for blind 8-dimension scoring
   e. If any dimension < 9: applies targeted heuristics from `rewrite-heuristics.md`
   f. Re-delegates to `english-judge-agent`. Repeat up to 5 attempts.
   g. Returns best draft with iteration report and chapter memory delta
4. Command assembles scenes → saves chapter file + chapter memory JSON

---

## 2. Reference Identification Skill

### Purpose

The Hebrew plugin assumes a fixed cultural/linguistic context (Israeli Hebrew). The English
plugin serves a much wider stylistic surface — military realism, dark psychology, domestic
minimalism, lyrical literary. Rather than hard-coding style presets as the sole anchor, the
reference-finder gives the system a living benchmark: actual published prose the writer
considers a model for THIS project.

### How It Works

**Inputs** (from `01-ingest.md` or user-provided):
- Genre tags (e.g., "military fiction", "psychological thriller")
- Tone descriptors (e.g., "spare", "lyrical", "fragmented", "darkly comic")
- Themes (e.g., "combat trauma", "class and violence", "memory unreliability")
- Time period of story (affects diction register expectations)
- POV and narrative distance (first-person unreliable, close third, omniscient)

**Process** (`reference-finder-agent`):
1. Load `reference-finder/references/style-taxonomy.md` for vocabulary
2. Use web search to identify 5 candidate published works matching the input signals
3. For each candidate: fetch a short prose excerpt (50–100 words), extract 2–3 technique
   annotations (what this excerpt demonstrates), and rank by relevance to the input
4. Return ranked list: top 3–5 works with excerpt + annotation

**Output** (written to `.story-bible/references.json`):
```json
{
  "query": {"genre": [...], "tone": [...], "themes": [...], "pov": "..."},
  "ranked_works": [
    {
      "rank": 1,
      "author": "Tim O'Brien",
      "title": "The Things They Carried",
      "year": 1990,
      "excerpt": "...",
      "technique_annotations": [
        "Accumulation as emotional weight — listing objects implies trauma",
        "Declarative sentences as suppression — stating fact to avoid feeling",
        "Physical specificity substitutes for psychological explanation"
      ],
      "relevance_score": 9,
      "relevance_reasoning": "..."
    }
  ],
  "anchor_work": "The Things They Carried (O'Brien)",
  "composite_style_note": "Prose should carry weight through specificity, not adjectives..."
}
```

**Integration with Rewrite:**
The `story-orchestrator-agent` loads `references.json` before generating prose. The benchmark
excerpt and technique annotations become the style anchor — not "write like O'Brien" but
"apply these specific techniques this specific way."

The `english-judge-agent` receives the `references.json` anchor and uses it to score
the `reference_fidelity` dimension.

---

## 3. Scoring Rubric — 8 Dimensions (0–10)

All dimensions scored 0–10. **Pass threshold: all dimensions ≥ 9.**
If any dimension < 9 after 5 attempts, report why and what would unlock it.

### Dimension Definitions

**1. Rhythm (0–10)**
Sentence length variation, stress patterns, where breath lands. Tests cadence audibility
vs. metronome uniformity. A 9 passage has intentional variation: short sentences at peaks,
long at accumulation/interiority, fragments as emotional beats. A 5 varies length but
without structural purpose. A 2 is a metronome (all sentences similar length) or a
runaway (all sentences very long).

**2. Character Voice (0–10)**
Each character's speech and internal thought is distinct, consistent, and could not belong
to another character. Tests register differentiation, idiolect markers, and cross-scene
consistency. A 9 means you could remove dialogue tags and still know who is speaking.
A 5 means characters are distinguishable but occasionally drift. A 2 means everyone
sounds like the narrator.

**3. Continuity (0–10)**
Timeline, object, and emotional-state consistency across sections. Tests for contradictions
in where things are, what characters know, and what emotional trajectory makes sense.
A 9 has no continuity errors and explicit handling of time jumps. A 5 has minor
inconsistencies recoverable by context. A 2 has contradictions that break the story logic.

**4. Prose Density (0–10)**
No filler; every sentence earns its place. Tests the ratio of load-bearing prose to
connective tissue, throat-clearing, and redundancy. A 9 can survive aggressive deletion
attempts — nothing can come out without loss. A 5 has some padding but mostly lean.
A 2 is bloated: multiple sentences saying the same thing, over-connected transitions,
unnecessary context-setting.

**5. POV Consistency (0–10)**
No unintentional head-hopping or register breaks. Tests whether the narrative perspective
is locked and applied consistently. A 9 holds its distance and register throughout; any
shift is clearly intentional. A 5 drifts occasionally (narrator accidentally knows something
they shouldn't, or the distance shifts without purpose). A 2 shifts POV freely within
scenes without craft justification.

**6. Emotional Register (0–10)**
Tone matches the scene's function; modulation is intentional. Tests whether the prose's
emotional temperature is calibrated to what the scene needs. A 9 has a stable emotional
temperature that shifts with purpose — the reader feels the register change as meaningful.
A 5 is mostly stable with accidental warm/cold spots. A 2 is tonally incoherent: comic
and tragic cues collide without irony.

**7. Show Don't Tell (0–10)**
Named emotions vs. enacted emotions ratio. Tests the balance between stating what a
character feels ("she felt grief") and enacting it through physical detail, action, and
behavior. A 9 has almost no named emotions; affect is 100% implied. A 5 has some telling
but mostly showing. A 2 is predominantly told: characters' inner states narrated directly.

**8. Reference Fidelity (0–10)**
Does the prose reflect the identified benchmark works? Tests whether the specific techniques
annotated in `references.json` are present and applied correctly. If no references have
been identified (no `references.json` exists), this dimension is scored N/A. A 9 clearly
applies 2–3 of the annotated techniques. A 5 applies them inconsistently. A 2 ignores
the benchmarks entirely and defaults to generic literary prose.

---

## 4. Iteration Loop Design

### Loop Contract

```
attempt = 1                    # 1-indexed
best = null                    # {draft, scores_json, attempt}
log = []                       # [{attempt, scores_json, failing_dimensions}]
MAX_ATTEMPTS = 5

loop:
  if attempt == 1:
    draft = initial_prose_from_steps_4_5
  else:
    draft = targeted_rewrite(previous_draft, failing_dimensions)

  scores = delegate_to(english-judge-agent, {
    passage: draft,
    tone_preset: ...,
    references_json: ...,      # new: passed to judge for reference_fidelity scoring
    attempt_number: attempt,
    previous_scores: [entry.scores_json for entry in log]
  })

  failing = [dim for dim, score in scores if score < 9]
  log.append({attempt, scores, failing})

  if best is null or scores.average > best.scores.average:
    best = {draft, scores, attempt}

  if len(failing) == 0:
    break with status = "ALL_PASS_ON_ATTEMPT_" + attempt

  if attempt == MAX_ATTEMPTS:
    break with status = "MAX_ATTEMPTS_REACHED"

  if attempt >= 2:
    prev_failing_count = len(log[-2].failing)
    curr_failing_count = len(failing)
    if curr_failing_count >= prev_failing_count and scores.average <= log[-2].scores.average:
      break with status = "NO_IMPROVEMENT_ABORT"

  attempt += 1
```

### Targeted Rewrite Strategy

On each non-passing attempt, apply only the heuristics mapped to the failing dimensions.
Do not re-run the full humanizer pass — that is too blunt and can introduce new failures
while fixing old ones.

| Failing dimension | Heuristic section |
|---|---|
| `rhythm` | Rhythm normalization in `rewrite-heuristics.md` |
| `character_voice` | Voice differentiation rules |
| `continuity` | Timeline and object tracking |
| `prose_density` | Deletion audit pass |
| `pov_consistency` | POV lock enforcement |
| `emotional_register` | Register calibration rules |
| `show_dont_tell` | Action/implication replacement |
| `reference_fidelity` | Technique application from `references.json` annotations |

### Regression Prevention

After each targeted rewrite:
- Re-score ALL 8 dimensions (not just the targeted ones)
- If a previously-passing dimension drops below 9, treat it as a new failure
- Prioritize fixing regressions before continuing to work on original failures
- If fixing one dimension consistently breaks another, note the tension in the iteration
  report and halt with `NO_IMPROVEMENT_ABORT`

### Stopping Conditions

1. **ALL_PASS**: all 8 applicable dimensions ≥ 9 — return best draft, status PASS
2. **MAX_ATTEMPTS_REACHED**: 5 attempts exhausted — return best draft, warning, explain
   which dimensions remain below 9 and what specific passage is the blocker
3. **NO_IMPROVEMENT_ABORT**: two consecutive attempts fail to improve average or reduce
   failing count — return best, warning, explain root tension

---

## 5. Corpus / Golden Examples

### Purpose

The corpus serves two functions:
1. **Style benchmark** — concrete examples of the techniques the plugin enforces, used
   by the `reference-finder` skill to calibrate what "good" looks like
2. **Judge calibration** — hand-scored passages that `english-judge-agent` must agree with
   within tolerance for production trust

### Selected Works

All excerpts are short (50–100 words), chosen because they demonstrate a specific,
annotatable technique that the scoring rubric tests.

| File | Author / Work | Technique Demonstrated |
|------|--------------|----------------------|
| `corpus/01-military-realism.md` | Tim O'Brien, *The Things They Carried* | Accumulation as emotional weight; declarative suppression |
| `corpus/02-dark-psychology.md` | Denis Johnson, *Jesus' Son* | Fractured time; violence without moral comment |
| `corpus/03-war-interiority.md` | Kevin Powers, *The Yellow Birds* | Lyrical close-third; past-present layer |
| `corpus/04-literary-minimalism.md` | Raymond Carver, *Cathedral* | Iceberg theory; flat affect implying depth |
| `corpus/05-combat-closeup.md` | Karl Marlantes, *Matterhorn* | Technical precision; body as text, not metaphor |

### Calibration Corpus (eval/)

18+ hand-scored English passages across the 8 dimensions, organized in the same
PASS/FLAG/FAIL band structure as the Hebrew eval corpus. The pass threshold for the
calibration judge is stricter: the judge must agree on whether all dimensions reach 9
(not just the average band).

---

## Status

PLAN COMPLETE — proceeding to implementation
