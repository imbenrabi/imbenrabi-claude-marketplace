---
description: Create or update a character's story bible entry with voice profile, psychology, biography, and relationship data.
allowed-tools: Read, Write, Edit
---

# Update Character

Create or update a character's JSON store in `.story-bible/characters/`.

## Input

`$ARGUMENTS` is the character's name. Example: `נועה` or `eli`

If `$ARGUMENTS` is empty: ask for the character's name.

## Step 1: Check for existing character

Check if `.story-bible/characters/{name}.json` exists.

**If exists:** Read and display the current profile. Ask: "Which field do you want to update?"

**If not exists:** This is a new character. Proceed to collect full profile.

## Step 2: Collect character data

For a new character, ask (one topic at a time, not all at once):

**Identity:**
- Full name (Hebrew)
- Age and biography summary (2–3 sentences)
- Current status in the story

**Voice profile** (most important — affects all generated dialogue):
- Register: which of these fits? ultra-formal / academic / professional / everyday / slang-dense / military / street / youth
- Slang density (0–10)
- Sentence length tendency: short / medium / long / variable
- Does this character typically finish their sentences, or trail off?
- 2–3 recurring expressions or phrases this character uses
- Profanity tolerance (0–10)

**Psychology:**
- Primary fear
- Core motivation
- One contradiction (something they believe vs. how they actually behave)
- One secret
- Ideological worldview in one sentence

**Physical:**
- Brief appearance description
- One distinctive physical detail

For an existing character being updated: only ask about the specific fields being changed.

## Step 3: Validate with canon-engine-agent

Delegate to `canon-engine-agent` (read-only mode):
- Pass: the proposed character data
- Pass: existing story bible (timeline, chapter memories)
- Ask: does this character's new data conflict with anything already in canon?

If conflicts found: present them to the user before saving.

## Step 4: Write the character file

Save `.story-bible/characters/{name}.json` using this schema:

```json
{
  "name": "",
  "biography": {
    "birth": "",
    "history": [],
    "current_status": ""
  },
  "physical": {
    "appearance": "",
    "distinctive_features": []
  },
  "voice": {
    "register": "",
    "slang_density": 0,
    "sentence_length_tendency": "",
    "recurring_expressions": [],
    "dialogue_rhythm": "",
    "interruption_frequency": "",
    "profanity_tolerance": "",
    "typical_sentence_openers": []
  },
  "psychology": {
    "fears": [],
    "motivations": [],
    "contradictions": [],
    "secrets": [],
    "internal_conflicts": [],
    "ideological_worldview": "",
    "emotional_arc": []
  },
  "relationships": {},
  "chapter_appearances": []
}
```

Populate all fields from the user's responses. Leave arrays empty `[]` for fields not yet defined — do not fill with placeholder text.

## Step 5: Confirm

Report:
- Character file saved: `.story-bible/characters/{name}.json`
- Key voice parameters: register, slang_density, sentence_length_tendency
- Note: "The character-memory-agent and story-orchestrator-agent will now use this profile to enforce voice consistency."

If relationships were defined, ask whether to update the `relationships.json` file to reflect them.
