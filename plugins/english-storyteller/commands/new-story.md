---
description: Initialize a new English story project with a full story bible scaffold — character stores, timeline, world, relationship graph, tone profile, and optional reference anchor.
allowed-tools: Read, Write, Bash
---

# New Story

Initialize an English literary fiction project in the current directory.

## Step 1: Gather story identity

Ask the user:
1. Story title (used for folder naming)
2. Genre: literary fiction / war fiction / psychological / domestic / thriller / southern gothic / other
3. Tone preset (show list, ask user to pick):
   - `military-realism` — spare, declarative, physical (O'Brien, Marlantes)
   - `dark-psychological` — fractured, violence without comment (McCarthy, Johnson)
   - `literary-minimalism` — iceberg theory, flat affect (Carver, Hemingway)
   - `war-interiority` — lyrical close-third, past-present (Powers, Klay)
   - `domestic-realism` — precision, ordinary catastrophe (Munro, Yates)
   - `lyrical-literary` — earned abstraction, long rhythm (Robinson, Morrison)
   - `urban-grit` — dialogue-heavy, class-conscious (Price, Pelecanos)
   - `southern-gothic` — grotesque, regional (O'Connor, McCullers)
   - `literary-thriller` — cool precision, paranoia (DeLillo, Highsmith)
   - `historical-literary` — period voice, research as texture (Mantel, Tóibín)
4. Brief world premise (2–3 sentences): when, where, what kind of world
5. Would you like to run the reference finder now? (yes / no / skip for now)

## Step 2: Create directory structure

```bash
mkdir -p .story-bible/characters .story-bible/chapters
```

## Step 3: Write story bible files

Create `.story-bible/tone-profile.json`:
```json
{
  "base_preset": "[user's selection]",
  "overrides": {},
  "pass_threshold": 9,
  "max_iteration_attempts": 5
}
```

Create `.story-bible/timeline.json`:
```json
{
  "events": [],
  "gaps": [],
  "flashbacks": [],
  "character_whereabouts": {}
}
```

Create `.story-bible/world.json` populated with the user's world premise.

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

## Step 4: Optional reference finding

If user said yes to reference finding:
- Delegate to `reference-finder-agent` with the genre + tone preset as input signals
- Save result to `.story-bible/references.json`

## Step 5: Confirm

Report:
- Files created: list the JSON files + subdirectories
- Tone preset active: [name]
- Reference anchor: [anchor work if found, or "not yet identified"]
- Next steps:
  - Add characters: `/english-storyteller:update-character [name]`
  - Find style references: `/english-storyteller:find-references`
  - Start writing: `/english-storyteller:new-chapter`

Do NOT create chapter files. Do NOT generate prose. Scaffold only.
