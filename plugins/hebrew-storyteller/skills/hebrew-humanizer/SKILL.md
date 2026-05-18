---
name: hebrew-humanizer
description: >
  Detect and rewrite AI-sounding Hebrew prose. Remove translated-English structures,
  emotional over-explanation, robotic rhythm, and connective repetition. Apply after
  any Hebrew generation pass, or when asked to polish, refine, or improve Hebrew text.
  Triggers on: "rewrite", "humanize", "polish", "refine", "AI-sounding", "robotic",
  "sounds translated", "fix Hebrew", "natural", "Israeli", "sounds formal", "רובוטי",
  "מתורגם", "לא טבעי", "שכתוב", "לשפר", "not natural", "sounds like AI", "clean up",
  "תקן את העברית", "שפר", "נשמע מתורגם", "נשמע רובוטי", "humanize Hebrew".
allowed-tools: Read
---

# Hebrew Humanizer

Detect AI-sounding patterns in Hebrew prose and rewrite them to read as natural Israeli text.

**Step 1: Load reference files**

Read before analyzing:
- `${CLAUDE_SKILL_DIR}/references/ai-patterns.md` — red flag inventory with severity levels
- `${CLAUDE_SKILL_DIR}/references/rewrite-heuristics.md` — per-pattern rewrite strategies

---

## Detection Pass

**Run a full red-flag scan before rewriting anything.**

Count red flags in the submitted text using the inventory from `ai-patterns.md`. Track:
- Total red flag count
- Severity distribution (HIGH / MEDIUM / LOW)
- Specific passages flagged

Threshold: 3+ flags of any severity = mandatory rewrite pass. Fewer = optional polish.

Report the flag count with the rewrite so the user understands what changed and why.

## 1. Emotional Over-Explanation

**The cardinal sin of AI Hebrew prose: stating emotions instead of implying them.**

Delete any sentence matching:
- הוא/היא הרגיש/ה ש...
- הוא/היא ידע/ה ש...
- הוא/היא הבין/ה ש...
- הוא/היא חש/ה ש...
- הוא/היא הבין/ה שעכשיו...

Replace with: physical action, sensory detail, behavioral implication, or silence.

Emotion is not told — it is recoverable from what the character does, what they notice, what they don't say.

## 2. Translated Structure Detection

**Identify English-to-Hebrew sentence artifacts and restructure.**

Common artifacts:
- Rigid SVO in narrative where Hebrew would use VSO or subject omission
- Nominal structures: "ביצוע הפעולה" where Hebrew would prefer verbal "לבצע"
- Direct conjunction chains: "הוא פתח את הדלת ואז הוא ראה אותה ואז הוא נעצר"
- Over-complete relative clauses where Hebrew would use ש-reduction or omit entirely
- Article (ה) overuse where Hebrew naturally omits

Restructure to Hebrew-native patterns. When uncertain: shorter is almost always more Israeli.

## 3. Dialogue Fragmentation

**Real Israeli dialogue is incomplete, interrupted, and implicit.**

Signs of artificial dialogue:
- Every turn is a complete grammatical sentence
- Characters finish each other's thoughts cleanly
- No hesitation, trailing off, or incomplete intent
- Characters explain their feelings explicitly ("אני כועס כי...")
- Every response directly addresses what was asked

Fixes:
- Add em-dash interruptions (—) where one character cuts off another
- Truncate sentences that real people wouldn't finish: "אני לא יודע אם—"
- Remove explicit emotional self-reporting from dialogue
- Let a character not answer the question asked — answer a different one, or say nothing
- Reduce dialogue tags to minimum; let rhythm carry the speaker

## 4. Rhythm Normalization

**Uniform rhythm is the acoustic signature of AI text.**

Break any run of:
- 4+ consecutive sentences starting with the same word class (especially הוא/היא)
- 4+ consecutive sentences of nearly identical length
- 3+ consecutive sentences opening with a connective (אבל/ואז/כי/ולכן)

Techniques:
- Vary sentence length deliberately
- Introduce fragments where real prose would have them
- Replace connective-opener sentences with structurally different formulations
- Scan for the most-repeated word in 150 words; reduce occurrences by at least half

## 5. Subtext Generation

**AI prose over-explains. Literary prose under-explains.**

When the text states what the reader can infer:
- Delete the explanation
- Trust the preceding action or image to carry the meaning
- If deletion creates confusion, add a physical detail instead of an emotional label

The test: remove the sentence. Does a reader lose *information* (keep it) or just lose the author *explaining that information* (delete it)?

## Output Format

Show:
1. **Flag count**: X flags found (Y HIGH, Z MEDIUM, W LOW)
2. **Rewritten passage**: the full rewritten text
3. **Change log**: bulleted list of what changed and why (one line per change)

Do NOT include the original in the output unless the user specifically asks for a side-by-side comparison.
