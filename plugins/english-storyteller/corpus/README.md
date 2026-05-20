# English Style Benchmark Corpus

Five hand-selected excerpts from English literary fiction. Each excerpt demonstrates
specific, annotatable techniques that the plugin enforces. Use these as:

1. **Style benchmarks** — concrete proof of what the annotated technique looks like when
   done right
2. **Calibration anchors** — reference for what scores 9–10 look like in specific dimensions
3. **Reference-finder fallback** — if web search returns low-confidence results, the
   corpus provides pre-curated alternatives

---

## Corpus Contents

| File | Author / Work | Primary Technique | Tone Preset |
|------|--------------|-------------------|-------------|
| `01-military-realism.md` | Tim O'Brien, *The Things They Carried* | `accumulation_as_weight`, `declarative_suppression` | `military-realism` |
| `02-dark-psychology.md` | Denis Johnson, *Jesus' Son* | `body_as_compass`, `violence_without_comment` | `dark-psychological` |
| `03-war-interiority.md` | Kevin Powers, *The Yellow Birds* | `time_dislocation`, `earned_abstraction` | `war-interiority` |
| `04-literary-minimalism.md` | Raymond Carver, *Cathedral* | `iceberg_theory`, `declarative_suppression` | `literary-minimalism` |
| `05-combat-closeup.md` | Karl Marlantes, *Matterhorn* | `body_as_compass`, technical precision | `military-realism` |

---

## How to Use These Excerpts

### As Style Benchmark

When the `reference-finder-agent` identifies a work from this corpus as the anchor,
the excerpt here is used for technique annotation. The technique annotations in each file
are the specific targets for the `reference_fidelity` scoring dimension.

### As Calibration

The rubric's score-10 examples are anchored to techniques demonstrated in these corpus
files. When calibrating `english-judge-agent`, the evaluator should confirm that the
judge correctly identifies these techniques at the stated score levels.

### As Direct Reference

A writer can read these excerpts directly to understand what the target prose quality
looks like. The annotations explain WHY each excerpt works at the craft level, not just
what it demonstrates.

---

## Adding Passages

To add a corpus passage:
1. Identify the work and locate a representative excerpt (50–100 words)
2. Verify the excerpt demonstrates 2–3 specific, annotatable techniques
3. Create `{NN}-{slug}.md` with the excerpt and annotations
4. Update this README's corpus contents table
5. Optionally add entries to `eval/golden-corpus/index.json` for calibration

Corpus passages should be short enough to cite as craft examples, not long enough to
raise copyright concerns. 50–100 words is appropriate for criticism and teaching purposes.
