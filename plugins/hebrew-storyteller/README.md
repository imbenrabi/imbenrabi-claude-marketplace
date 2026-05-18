# hebrew-storyteller

Production-grade Hebrew literary prose framework. A narrative operating system — not a single prompt — built from cooperating skills, agents, and commands that enforce Hebrew-native writing, persistent story continuity, and character voice consistency across long-form projects.

---

## What This Is

LLMs generate Hebrew that reads as translated English: rigid word order, register oscillation, emotional over-explanation, connective repetition, and dialogue that sounds grammatically perfect but human-free. This plugin fixes that.

The framework enforces:
- Hebrew-native syntax (VSO ordering, של-chain reduction, subject omission)
- Register locking (narrator and each character maintain distinct, stable registers)
- AI pattern detection and rewriting (subject loops, emotional exposition, uniform rhythm)
- Persistent story continuity via a file-based story bible
- Character voice consistency across chapters via JSON voice profiles
- Quality-gated output (QA scoring before any prose reaches the user)

---

## Architecture

```
COMMANDS              → user entry points
  new-story           initialize story project
  new-chapter         generate a chapter with planning + QA
  rewrite             humanize a passage
  check-canon         validate against story bible
  update-character    create or update character profiles

AGENTS                → orchestration layer
  story-orchestrator  main prose generator with iteration loop
  narrative-planner   structural planning (no prose output)
  character-memory    voice consistency enforcement
  canon-engine        read-only continuity validator
  hebrew-judge        blind QA scorer (writer/judge separation)

SKILLS                → behavioral rules (auto-trigger)
  hebrew-voice-engine register locking, syntax, rhythm, gender
  hebrew-humanizer    AI pattern detection and rewriting
  hebrew-qa-validator 5-dimension quality scoring
  scene-composer      in-media-res, subtext, cinematic pacing

STORY BIBLE           → file-based persistent store
  .story-bible/
    characters/*.json   voice profiles, psychology, relationships
    timeline.json       event sequence, causality, whereabouts
    world.json          locations, rules, atmosphere
    relationships.json  alliances, tensions, knowledge gaps
    tone-profile.json   active tone preset + overrides
    chapters/*.json     per-chapter memory, open threads
```

---

## Skills

| Skill | Triggers on | What it enforces |
|-------|-------------|-----------------|
| `hebrew-voice-engine` | Writing Hebrew prose, dialogue, narrative | Syntax, register lock, connective suppression, rhythm, gender agreement |
| `hebrew-humanizer` | Rewrite, polish, fix Hebrew, humanize | AI pattern removal, emotional exposition deletion, dialogue fragmentation |
| `hebrew-qa-validator` | Quality check, score, validate Hebrew | 5-dimension scoring: naturalness, AI detectability, register consistency, dialogue realism, rhythm variety |
| `scene-composer` | Write scene, open scene, scene transition | In-media-res entry, sensory anchoring, subtext, pacing via sentence length |

---

## Commands

| Command | Usage | What it does |
|---------|-------|-------------|
| `/hebrew-storyteller:new-story` | `/new-story` | Scaffold `.story-bible/` with all JSON stores and tone preset |
| `/hebrew-storyteller:new-chapter` | `/new-chapter` | Plan + generate + QA-gate a full chapter |
| `/hebrew-storyteller:rewrite` | `/rewrite [Hebrew text]` | Humanizer pass with flag count and change log |
| `/hebrew-storyteller:check-canon` | `/check-canon chapters/03.md` | Validate file against story bible for continuity |
| `/hebrew-storyteller:update-character` | `/update-character נועה` | Create or update character JSON store |
| `/hebrew-storyteller:qa-calibrate` | `/qa-calibrate` | Verify `hebrew-judge-agent` against the golden Hebrew corpus; report drift |

---

## Supported Tone Presets

| Preset | Style |
|--------|-------|
| `contemporary-israeli-realism` | Everyday Israeli life, close narration, fast dialogue |
| `minimalist-literary` | Sparse, fragments, silence as beat |
| `dark-psychological` | Long interiority, unreliable narrator, accumulation |
| `sarcastic-urban` | Ironic, staccato, slang-heavy, deadpan |
| `intimate-first-person` | Confessional, warm, direct address |
| `cinematic-thriller` | Present tense, action-forward, camera-eye |
| `melancholic-literary` | Lyrical, slow, Mediterranean weight |
| `military-realism` | Tactical brevity, silence as communication |
| `tel-aviv-conversational` | Code-switching, overlapping dialogue, city-paced |
| `jerusalem-introspective` | Layered, historical weight, measured |

---

## QA Scoring

All generated prose is scored before delivery:

| Dimension | What it measures |
|-----------|----------------|
| Naturalness | Does it sound like real Israeli Hebrew? |
| AI Detectability | How hard is it to tell an AI wrote this? |
| Register Consistency | Is the tone stable throughout? |
| Dialogue Realism | Do characters sound like humans? |
| Rhythm Variety | Does sentence length vary meaningfully? |

**PASS** (avg ≥ 7.0): delivered as-is.
**FLAG** (5.0–6.9): user is shown score and asked whether to revise.
**FAIL** (< 5.0): humanizer pass runs automatically before delivery.

Scoring runs in a disciplined, capped iteration loop. The `story-orchestrator-agent` generates prose, then delegates to `hebrew-judge-agent` — a blind judge that sees only the passage, the tone preset, and the rubric (never the story bible, never the writer's reasoning, never prior drafts). If the judge returns FLAG or FAIL, the orchestrator selectively re-applies humanizer heuristics matched to the judge's top issue tags and re-submits. The loop is capped at 3 attempts; if two consecutive attempts fail to improve the average score by ≥0.3, the orchestrator aborts and returns the best attempt with a warning. The judge itself is calibrated against a hand-scored Hebrew corpus via `/hebrew-storyteller:qa-calibrate`, so rubric drift is detectable.

---

## Eval Corpus

`eval/golden-corpus/` holds 18 hand-scored Hebrew passages spread across the 5 QA dimensions and the PASS / FLAG / FAIL bands, plus 3 multi-dimension stressors (one of which tests the minimalist-literary preset's rhythm exception). `/hebrew-storyteller:qa-calibrate` runs `hebrew-judge-agent` against the corpus and reports per-dimension MAE, verdict-band agreement, and tolerance breaches. The judge must PASS calibration before it can be trusted for production quality gating; re-calibrate after any change to the rubric, the judge instructions, or the underlying model version. See `eval/golden-corpus/README.md` for the corpus schema and the methodology for adding passages.

---

## Hebrew Constraints Addressed

| Problem | Mitigation |
|---------|-----------|
| Translated sentence structure | VSO syntax rules, של-reduction, clause inversion |
| Register oscillation | Register lock per scene, narrator ≠ character |
| Emotional over-explanation | Humanizer deletes הרגיש/ידע/הבין clauses |
| Connective repetition | Connective suppression rules, structural alternatives |
| Uniform rhythm | Rhythm normalization, sentence length variance requirement |
| Over-complete dialogue | Dialogue fragmentation rules, non-answer modeling |
| Fake literary depth | Metaphor density control, concrete grounding |
| Gender agreement failure | Entity gender tracking in voice engine |

---

## Weakness Analysis

**Known limitations:**

1. **No real-time audio feedback.** Rhythm validation is heuristic — it does not read prose aloud. A human editor should verify cadence.

2. **Slang currency.** Israeli slang evolves. The preset slang profiles encode general tendencies; specific current slang must be added via character voice profiles or manual input.

3. **Dialect specificity.** The framework distinguishes broad registers but does not handle fine-grained dialectal variation (specific Mizrahi community speech patterns, diaspora Hebrew accent markers).

4. **Long-context drift.** Beyond ~100K tokens of story, the agents may lose fine-grained story bible details. Recommend running `/check-canon` every 3–4 chapters.

5. **First-chapter cold start.** With no prior chapter memory, the narrative planner has less to work with. Richer world and character setup in `new-story` improves first chapter quality.

6. **Judge drift over model versions.** The `hebrew-judge-agent` is calibrated against the golden corpus, but the rubric anchors are Hebrew passages whose interpretation can shift across model versions. Run `/hebrew-storyteller:qa-calibrate` after upgrading the underlying model, and re-tune the rubric or judge instructions if calibration breaks.

---

## Future Improvements

- **Dialogue overlap simulation**: model simultaneous speech and interruption more precisely
- **Regional dialect profiles**: Haifa, Beer Sheva, Mizrahi community-specific registers
- **Scene graph visualization**: timeline and relationship graph as visual output
- **Continuous memory**: beyond chapter JSON, a rolling summary of the full novel's state
- **Comparative stylometry**: score how closely output matches a target author's style (Amos Oz, Etgar Keret, Dorit Rabinyan)
- **Arabic loanword density tracking**: for Jerusalem-introspective and street registers
- **Multi-POV register manager**: tracking narrator shifts across POV chapters
