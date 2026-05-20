---
description: Create or update a character JSON voice profile in the story bible. Includes voice register, psychology, relationships, and dialogue rhythm.
allowed-tools: Read, Write, Bash
---

# Update Character

Create a new character or update an existing character's JSON store in the story bible.

## Input

`$ARGUMENTS` contains the character's name.

If `$ARGUMENTS` is empty: ask the user for the character's name.

## Process

**Step 1: Check for existing profile**

```bash
ls .story-bible/characters/${name}.json 2>/dev/null
```

If exists: read and display the current profile. Ask: "What do you want to update?"
If not exists: create a new profile. Ask the following questions.

**Step 2: Gather character information**

Ask the user (or confirm from existing profile):
1. Full name and any aliases
2. Role in the story (protagonist / antagonist / supporting)
3. Brief biography (2–3 sentences)
4. **Voice register** (show options from `register-profiles.md`):
   - `terse-military`, `educated-professional`, `everyday-american`, `street-vernacular`,
     `youth-digital`, `southern-regional`, `academic-formal`, `immigrant-english`
5. Sentence length tendency: short / medium / long / variable
6. Recurring expressions or phrases (if any)
7. Dialogue rhythm: staccato / fast / measured / slow
8. Interruption frequency: 0–10
9. Profanity tolerance: 0–10
10. Psychological contradictions (where behavior diverges from stated values)
11. What this character wants / fears / hides

**Step 3: Validate against existing canon**

If updating: delegate to `character-memory-agent` to check for conflicts with established
chapter appearances, biography, and relationship entries.

If creating: check that no existing character has the same name in `.story-bible/characters/`.

**Step 4: Write the character file**

Write `.story-bible/characters/{name}.json`:

```json
{
  "name": "...",
  "aliases": [],
  "role": "...",
  "biography": {
    "summary": "...",
    "history": []
  },
  "voice": {
    "register": "...",
    "sentence_length_tendency": "...",
    "recurring_expressions": [],
    "dialogue_rhythm": "...",
    "interruption_frequency": 0,
    "profanity_tolerance": 0,
    "typical_sentence_openers": [],
    "filter_word_tendency": "low"
  },
  "psychology": {
    "wants": "...",
    "fears": "...",
    "hides": "...",
    "ideological_worldview": "...",
    "contradictions": []
  },
  "relationships": {},
  "chapter_appearances": []
}
```

**Step 5: Confirm**

Report: "Character [name] saved to `.story-bible/characters/{name}.json`."
If new: "Add to a chapter with `/english-storyteller:new-chapter`."
If updated: "Character memory agent will enforce the updated profile going forward."
