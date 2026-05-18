---
description: Initialize a new Hebrew story project with a full story bible scaffold — character stores, timeline, world, relationship graph, and tone profile.
allowed-tools: Read, Write, Bash
---

# New Story

Initialize a Hebrew story project in the current directory.

## Step 1: Gather story identity

Ask the user:
1. Story title (Hebrew or English — used for folder naming)
2. Genre (choose one): literary fiction / thriller / psychological drama / coming-of-age / military / urban realism / historical fiction / other
3. Tone preset (show list and ask user to pick):
   - `contemporary-israeli-realism` — everyday Israeli life, close narration
   - `minimalist-literary` — sparse, every word earns its place
   - `dark-psychological` — long interiority, unreliable narrator
   - `sarcastic-urban` — ironic, fast, slang-heavy
   - `intimate-first-person` — confessional, warm, reflective
   - `cinematic-thriller` — present tense, action-forward, no interiority
   - `melancholic-literary` — lyrical, slow, Mediterranean weight
   - `military-realism` — tactical, terse, physical
   - `tel-aviv-conversational` — fast, multilingual, city-paced
   - `jerusalem-introspective` — weighted, layered, historically resonant
4. Brief world premise (2–3 sentences): when, where, what kind of world

## Step 2: Create directory structure

```bash
mkdir -p .story-bible/characters .story-bible/chapters
```

## Step 3: Write story bible files

Create `.story-bible/tone-profile.json` using the selected preset values from the tone-presets reference. Set `base_preset` to the user's selection. Set `overrides` to `{}`.

Create `.story-bible/timeline.json`:
```json
{
  "events": [],
  "gaps": [],
  "flashbacks": [],
  "character_whereabouts": {}
}
```

Create `.story-bible/world.json` populated with the user's world premise:
```json
{
  "locations": {},
  "political_structures": [],
  "cultural_norms": [],
  "technologies": [],
  "rules": [],
  "atmosphere": {
    "sensory_defaults": {},
    "time_period": "[from user's premise]"
  }
}
```

Create `.story-bible/relationships.json`:
```json
{
  "pairs": {},
  "alliances": [],
  "tensions": [],
  "romantic_arcs": [],
  "betrayals": [],
  "knowledge_asymmetry": []
}
```

## Step 4: Confirm

Report to user:
- Files created: list the 4 JSON files + subdirectories
- Tone preset active: [name]
- Next steps:
  - Add characters: `/hebrew-storyteller:update-character [name]`
  - Start writing: `/hebrew-storyteller:new-chapter`
  - Or just tell Claude: "Write chapter 1 of [story title]" and the story-orchestrator-agent will take over

Do NOT create any chapter files. Do NOT generate any prose. Scaffold only.
