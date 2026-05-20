---
description: Identify 3–5 comparable published English literary works that match the active project's genre, tone, and themes. Saves results as the style anchor for the orchestrator and judge.
allowed-tools: Read, Write, Bash, WebSearch
---

# Find References

Identify comparable published English literary works for the active project and save
them as the style anchor in `.story-bible/references.json`.

## Input

`$ARGUMENTS` may contain explicit signals (genre, tone, themes) or be empty.

If empty and `.story-bible/tone-profile.json` exists: use the tone preset as the primary
signal and ask if the user wants to add any additional descriptors.

If no story bible and no `$ARGUMENTS`: ask the user for at least one of: genre, tone
descriptor, or primary theme.

## Process

**Step 1: Verify or gather signals**

If story bible exists:
- Read `.story-bible/tone-profile.json` — extract preset name
- If `.story-bible/references.json` already exists: show current anchor work and ask
  whether to replace or add to it

**Step 2: Delegate to reference-finder-agent**

Delegate the full search to `reference-finder-agent`:
- Pass: all available signals (genre, tone, themes, POV, time period)
- Receive: ranked list of works with excerpts and technique annotations, plus
  `.story-bible/references.json` written by the agent

**Step 3: Display and confirm**

Show the ranked list to the user. Ask:
> "These are the identified reference works. The anchor work for style is [Title] by [Author].
> Accept this list, adjust ranks, or add a specific work manually?"

If the user wants to add a specific work manually:
- Ask for: title, author, year
- Ask for: representative excerpt (50–100 words) — user provides it
- Ask for: what technique(s) this demonstrates
- Insert into the ranked list at the appropriate position
- Regenerate the `composite_style_note` to include the added work

**Step 4: Confirm save**

Report: "References saved to `.story-bible/references.json`. The story-orchestrator-agent
and english-judge-agent will use [anchor work] as the style benchmark for all subsequent
generation and scoring passes."
