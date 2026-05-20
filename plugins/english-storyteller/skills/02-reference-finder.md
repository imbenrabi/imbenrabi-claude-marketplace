---
name: english-storyteller-reference-finder
description: >
  Stage 2 of the English Storyteller pipeline. Use web search to find 3-5 published
  English works that match the source's genre, tone, and themes. Extract style excerpts
  and technique annotations. Rank by relevance. Produces a benchmark block consumed by
  03-rewrite. This is the key addition over other prose pipelines.
  Triggers as part of the english-storyteller pipeline or standalone when asked to find
  literary benchmarks, comparable works, or style references for English prose.
allowed-tools: Read, WebSearch
---

# Stage 02 — Reference Finder

Find the 3–5 published English works that are closest to the source's literary DNA. Extract style excerpts. Rank. Hand off to 03-rewrite.

**This stage is not optional.** The rewrite depends on the benchmarks. Without them, 03-rewrite produces generic literary prose, not genre-specific craft.

---

## Step 1: Read the Handoff Block from Stage 01

Consume the handoff block from 01-ingest:
- `genre_label` — use verbatim in first search query
- `subgenre_label` — add to second query
- `tone_labels` — add to targeted searches
- `themes` — use individual themes in author-focused searches
- `pacing_signature` — filter candidates by structural match
- `key_passage_for_matching` — use as exemplar for technique matching
- `source_language` — if not English, prioritize works that share diaspora/translation experience

If no handoff block exists (standalone use): run 01-ingest mentally on user-provided description before searching.

---

## Step 2: Build Search Queries

Generate 4–6 search queries in sequence. Each query targets a different facet:

**Query 1 — Genre + Tone match:**
`"[genre_label]" "[tone_labels[0]]" English fiction [year range: last 30 years]`

**Query 2 — Subgenre + Theme match:**
`"[subgenre_label]" "[themes[0]]" literary novel award OR acclaimed OR prize`

**Query 3 — Technique match (pacing/structure):**
`[pacing_signature key term] fiction "sentence fragment" OR "staccato" "[structural_feature]"`

**Query 4 — Thematic cluster:**
`"[themes[1]]" "[themes[2]]" literary fiction review OR recommendation`

**Query 5 (if source is military):**
`military fiction psychological realism "first person" acclaimed English novel`

**Query 6 (if source involves trauma/dissociation):**
`trauma dissociation literary fiction English "flat affect" OR "controlled" prose style`

Execute queries sequentially. Stop when you have 8+ candidate works.

---

## Step 3: Evaluate Candidates

For each candidate work found:

**Inclusion criteria (must have 3+):**
- [ ] English language, published
- [ ] Literary fiction (not thriller/genre fiction)
- [ ] Prose style matches tone label from 01-ingest
- [ ] Thematic overlap with at least 1 theme from 01-ingest
- [ ] Sentence-level pacing matches the signature from 01-ingest
- [ ] Has been reviewed/recommended by literary critics (not self-published)

**Disqualify if:**
- Primarily plot-driven with thin prose
- Comfort/redemption arc dominant (unless source has one)
- Sentimental register (emotion explained rather than enacted)
- More than 25 years old unless it's a canonical benchmark

**Score each candidate 1–10** on: genre fit (0–4) + tone fit (0–3) + pacing fit (0–3).

Select the top 3–5 by score. Minimum 3 required. If fewer than 3 pass the inclusion criteria, expand search before proceeding.

---

## Step 4: Extract Style Excerpts

For each selected work:

1. **WebSearch** for the author and title + "excerpt" OR "passage" OR "opening chapter"
2. Find a passage (50–150 words) that best demonstrates the relevant technique
3. Annotate with:
   - Technique demonstrated (specific, not generic — "staircase sentence shortening under fire" not "good pacing")
   - Why it applies to this source
   - One specific craft move to extract for the rewrite

If no excerpt is findable via web search: use your knowledge of the work to reconstruct a representative passage. Mark it `[reconstructed from knowledge]`.

---

## Step 5: Rank by Relevance

Rank the 3–5 final works on composite relevance to the specific source:

1. **Primary benchmark** — highest composite score; rewrite should default to this style
2. **Secondary benchmark** — structural or tonal technique to borrow specifically
3. **Tertiary benchmark(s)** — individual craft moves to extract

---

## Step 6: Synthesize Craft Moves

Produce a **Craft Move Inventory** — the specific, actionable techniques to apply in stage 03. Format:

```
CRAFT MOVE INVENTORY
─────────────────────
[Move 1]: [technique name] — from [author, work] — [one-sentence description of how to apply]
[Move 2]: ...
[Move 3]: ...
[Move 4]: ...
[Move 5]: ...
```

Limit to 5 craft moves. Each must be specific enough that a writer can apply it without rereading the source text.

---

## Output Format

```
REFERENCE FINDER RESULTS
═════════════════════════

SEARCH QUERIES EXECUTED:
1. [query]
2. [query]
...

TOP BENCHMARKS (ranked by relevance):

#1 — [Author, Title, Year] — Score: X/10
Genre fit: [explanation]
Tone fit: [explanation]
Pacing fit: [explanation]
Style excerpt:
  "[passage — 50–150 words]"
  [reconstructed from knowledge / found at: URL]
Key technique: [specific craft move name]

#2 — [Author, Title, Year] — Score: X/10
[same format]

#3 — [Author, Title, Year] — Score: X/10
[same format]

[#4, #5 if applicable]

CRAFT MOVE INVENTORY
─────────────────────
[Move 1]: [technique] — [author] — [how to apply]
[Move 2]: ...
[Move 3]: ...
[Move 4]: ...
[Move 5]: ...
```

```
─── HANDOFF → 03-rewrite ───
primary_benchmark: "[Author, Title]"
secondary_benchmark: "[Author, Title]"
tertiary_benchmarks: ["[Author, Title]", "[Author, Title]"]
craft_moves: [
  "[move 1 description]",
  "[move 2 description]",
  "[move 3 description]",
  "[move 4 description]",
  "[move 5 description]"
]
benchmark_score_primary: [1–10]
source_language: [from 01-ingest handoff]
adaptation_notes: [from 01-ingest handoff — pass through unchanged]
```
