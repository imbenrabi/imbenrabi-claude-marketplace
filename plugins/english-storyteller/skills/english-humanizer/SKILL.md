---
name: english-humanizer
description: >
  Detect and rewrite AI-sounding English prose. Remove filter words, named emotions,
  uniform rhythm, over-complete dialogue, and connective redundancy. Apply after any
  prose generation pass, or when asked to polish, refine, or improve English text.
  Triggers on: "rewrite", "humanize", "polish", "refine", "AI-sounding", "sounds robotic",
  "sounds generic", "fix prose", "natural", "sounds like AI", "clean up", "edit this",
  "filter words", "show don't tell", "named emotions", "telling not showing",
  "over-explained", "too wordy", "purple prose", "adverbs", "hedged", "passive".
allowed-tools: Read
---

# English Humanizer

Detect AI-sounding patterns in English prose and rewrite them to read as literary fiction.

**Step 1: Load reference files**

Read before analyzing:
- `${CLAUDE_SKILL_DIR}/references/ai-patterns.md` — red flag inventory with severity levels
- `${CLAUDE_SKILL_DIR}/references/rewrite-heuristics.md` — per-pattern rewrite strategies

---

## Detection Pass

**Run a full red-flag scan before rewriting anything.**

Count red flags using the inventory from `ai-patterns.md`. Track:
- Total red flag count
- Severity distribution (HIGH / MEDIUM / LOW)
- Specific passages flagged
- Which of the 8 scoring dimensions each flag threatens

Threshold: weighted score ≥ 3 (H×3 + M×1.5 + L×0.5) = mandatory rewrite pass.

Report the flag count alongside the rewrite so the user understands what changed and why.

---

## 1. Filter Word Deletion

**The cardinal sin of AI English prose: telling the reader what a character perceives instead of letting them perceive it directly.**

Delete or restructure any instance of:
- `she/he/they saw/noticed/observed/watched/spotted` (as a filter)
- `she/he/they heard/listened to` (as a filter)
- `she/he/they felt [abstract noun or state]`
- `she/he/they realized/understood/knew [emotion or revelation]`
- `she/he/they thought [that]` (as a distancing marker)
- `she/he/they sensed` (as a distancing filter)
- `it seemed to her/him/them`
- `she/he/they could see/hear/smell` (as a qualifier)

Replace with: direct perception (camera puts reader in the scene), free indirect discourse,
or action that implies the perception.

Distinction: "She felt the rough wood under her palm" = valid physical sensation.
"She felt confused" = named emotion, delete.

---

## 2. Named Emotion Deletion

**Literary prose enacts emotion. AI prose labels it.**

Delete any sentence that names a character's emotional state as a fact:
- "She was devastated."
- "He felt a surge of anger."
- "She experienced profound loneliness."
- "He was overwhelmed with grief."
- "She felt hope rising in her chest."
- "He was terrified."

Replace with: physical action, involuntary physical response, behavior, or silence.
The replacement should allow the reader to recover the emotion without being told it.

If the emotion is complex or ambiguous: use a physical action that ambiguously implies it —
let the reader hold the ambiguity.

---

## 3. Adverb and Qualifier Scrubbing

**Adverbs are usually a sign that the verb is wrong or the action is too vague.**

Flag and replace:
- `[verb] + ly adverb`: "walked slowly" → "moved to the window" or "walked without hurrying"
- `very/really/extremely/incredibly` + adjective: delete the intensifier; if the quality
  is important, show it, don't intensify it
- Hedging qualifiers: "kind of", "somewhat", "rather", "a bit", "fairly", "relatively",
  "pretty much", "almost as if", "seemed like it might be"

For adverbs: replace the weak verb + adverb with a single, precise verb when possible.
"She said softly" → "She said" (dialogue rhythm carries the softness) or let the content
of what she says imply tone.

---

## 4. Dialogue Fragmentation

**Real dialogue is incomplete, deflecting, and interrupted.**

Signs of artificial dialogue:
- Every turn is a complete grammatical sentence
- Each response directly addresses what was asked
- Characters explicitly state their emotions ("I feel angry because...")
- Characters complete each other's thoughts cleanly
- Dialogue tags carry emotion information ("he said angrily")

Fixes:
- Truncate: remove the part of the answer that directly answers the question
- Add non-answers: character responds to something other than what was asked
- Add physical beats instead of dialogue tags
- Use silence: "She didn't answer" is often more powerful than any answer
- Add interruptions with em-dash where characters cut each other off
- Remove redundant dialogue tags; let rhythm carry the speaker identification

---

## 5. Rhythm Normalization

**Uniform rhythm is the acoustic signature of AI text.**

Flag and break:
- 4+ consecutive sentences of approximately the same length
- 4+ consecutive sentences beginning with the same subject
- 3+ consecutive sentences beginning with a conjunction (But/And/So)
- Any paragraph that reads at a steady pace from first sentence to last

Techniques:
- Add a fragment at a tension point
- Follow a very long sentence with a very short one
- Move the shortest sentence to where the emotional peak is
- Vary subject position: put the object or complement first occasionally
- Remove every instance of the most-repeated word in 200 words; reduce by half

---

## 6. Purple Prose Detection

**Overwrought language signals that the writer doesn't trust the content.**

Flag:
- Cluster of 3+ abstract nouns in 30 words (soul/eternity/truth/beauty without concrete anchor)
- Extended metaphors that run more than 2 sentences without returning to the physical
- Adjective pairs where one does the work of both ("cold and icy", "dark and shadowy")
- Scene openings that dwell on atmosphere for more than 3 sentences before action

Fix: cut the abstraction and find the concrete detail that earns it. If you can't find one,
the abstraction is not ready to exist in this text.

---

## Output Format

Show:
1. **Flag count**: X flags found (Y HIGH, Z MEDIUM, W LOW) — weighted score: N
2. **Rewritten passage**: the full rewritten text
3. **Change log**: bulleted list of what changed and why (one line per change), including
   which scoring dimension each change improves

Do NOT include the original in the output unless the user specifically asks for side-by-side.
