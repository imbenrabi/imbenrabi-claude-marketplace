---
description: Validate a chapter or passage against the story bible for continuity violations, timeline contradictions, and character consistency.
allowed-tools: Read, Bash
---

# Check Canon

Run the canon-engine-agent against a chapter or passage to detect continuity violations.

## Input

`$ARGUMENTS` is the path to the file to validate. Example: `chapters/chapter-03.md`

If `$ARGUMENTS` is empty: ask the user for the file path or to paste the passage.

## Step 1: Verify story bible

```bash
ls .story-bible/ 2>/dev/null
```

If `.story-bible/` does not exist: stop and explain that canon validation requires a story bible. Ask the user to run `/hebrew-storyteller:new-story` first.

## Step 2: Load and validate

Read the submitted file or passage.

Delegate full validation to the `canon-engine-agent`:
- Pass: full contents of `.story-bible/` (all JSON files)
- Pass: the submitted text
- Receive: structured violation report

## Step 3: Output

Deliver the canon-engine-agent's report verbatim:
- CRITICAL violations listed first
- WARNINGS second
- NOTICES third
- Summary verdict: PASS / NEEDS REVISION / CANNOT PROCEED

If verdict is PASS: confirm the chapter is continuity-clean and ready to save.

If NEEDS REVISION or CANNOT PROCEED: do not save the chapter. Ask the user how they want to address the violations.

## Do Not

- Do NOT rewrite the chapter
- Do NOT fix violations automatically
- Do NOT modify any story bible files
- Report only — the user decides how to resolve
