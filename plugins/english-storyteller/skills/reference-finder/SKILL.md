---
name: reference-finder
description: >
  Identify 3–5 comparable published English works to serve as style benchmarks for the
  active story project. Accept genre tags, tone descriptors, and themes; return ranked
  list of published works with excerpt and technique annotations. Auto-triggers when the
  user asks for style references, comparable works, or benchmark identification.
  Triggers on: "find references", "comparable works", "benchmark", "style anchor",
  "what should I read", "similar to", "works like", "comparable fiction", "style model",
  "reference works", "what writers", "who writes like", "published works", "influence",
  "identify style", "find authors", "style search", "reference search", "literary models".
allowed-tools: Read, WebSearch
---

# Reference Finder

Identify published English literary works that match the active story's genre, tone,
and thematic signals. Store results in `.story-bible/references.json` for use by the
orchestrator and judge.

**Step 1: Load style taxonomy**

Read `${CLAUDE_SKILL_DIR}/references/style-taxonomy.md` for vocabulary and method.

---

## Input Sources

Accept signals from any of:
1. `.story-bible/tone-profile.json` — tone preset and parameter values
2. User-provided descriptors in the current request
3. `01-ingest.md` output (if the skill chain was run)

Minimum required: at least one genre tag OR one tone descriptor OR a theme.

---

## Search Process

**Step 1: Construct the query**

From the available signals, extract:
- Genre: literary fiction / war fiction / psychological thriller / domestic drama / etc.
- Tone descriptors: spare / lyrical / fractured / ironic / melancholic / brutal / etc.
- Themes: combat trauma / class and violence / memory unreliability / grief / etc.
- POV: first-person unreliable / close third / second-person / omniscient
- Time period of the story's setting

**Step 2: Web search**

Run 2–3 targeted web searches using queries like:
- "[genre] literary fiction [tone descriptor] [theme] award nominees"
- "novels like [description of the work's feeling]"
- "best [genre] fiction [time period] critical acclaim"

Prefer: literary fiction with critical consensus. Avoid: genre bestsellers without
literary craft reputation.

**Step 3: For each candidate work**

1. Verify: Is this work genuinely well-regarded for its prose craft?
2. Find a short excerpt (50–100 words) that demonstrates a technique relevant to this project
3. Annotate: what 2–3 specific techniques does this excerpt demonstrate?
4. Rate relevance to the active project's signals (1–10)
5. State reasoning: why does this work match these specific signals?

**Step 4: Rank and select**

Return the top 3–5 works ranked by relevance. The #1 work becomes the `anchor_work`.

---

## Output

Write to `.story-bible/references.json`:

```json
{
  "query": {
    "genre": ["..."],
    "tone": ["..."],
    "themes": ["..."],
    "pov": "...",
    "time_period": "..."
  },
  "search_date": "ISO-date",
  "ranked_works": [
    {
      "rank": 1,
      "author": "...",
      "title": "...",
      "year": YYYY,
      "excerpt": "50–100 words of representative prose",
      "technique_annotations": [
        "Technique 1: what it is and how this excerpt uses it",
        "Technique 2: ...",
        "Technique 3: ..."
      ],
      "relevance_score": 9,
      "relevance_reasoning": "one sentence explaining the match"
    }
  ],
  "anchor_work": "Title (Author)",
  "composite_style_note": "2–3 sentences synthesizing what the top works share and what
    the prose should do"
}
```

Also display to user:
- Ranked list with author, title, year
- Brief excerpt from the #1 work
- Composite style note

---

## Constraints

- Always verify that the cited work is genuinely a published literary novel with prose
  craft reputation (use web search to confirm before citing)
- Do NOT cite academic papers, genre bestsellers without literary craft recognition,
  or self-published work
- The excerpt must demonstrate a technique relevant to the active project — do not cite
  a work for its theme alone if the prose style is irrelevant
- If the query signals are too vague to run a meaningful search, ask the user for one
  more specific descriptor before proceeding
- If web search returns low-confidence results, state that in the output and recommend
  the user review the corpus folder's golden examples instead
