---
description: Validate a chapter or passage against the story bible for timeline contradictions, character state conflicts, relationship violations, and tone drift.
allowed-tools: Read, Bash
---

# Check Canon

Validate a file against the story bible for continuity and canon violations.

## Input

`$ARGUMENTS` contains the file path to validate (e.g., `chapters/chapter-03.md`).

If `$ARGUMENTS` is empty: ask the user which file to check.

## Process

**Step 1: Verify story bible**

```bash
ls .story-bible/ 2>/dev/null
```

If `.story-bible/` does not exist: stop. Tell user to run `/english-storyteller:new-story` first.

**Step 2: Read the file**

Read the submitted file at the path provided.

**Step 3: Delegate to canon-engine-agent**

Delegate the full validation to `canon-engine-agent`:
- Pass: the submitted file content
- Receive: structured violation report

**Step 4: Display report**

Show the full CANON VALIDATION REPORT from the canon-engine-agent.

If PASS: confirm "CANON VALIDATION: PASS" and stop.

If NEEDS REVISION or CANNOT PROCEED: show the report and ask the user how they'd like
to address the critical violations.

## Constraints

- Do NOT modify any files
- Do NOT rewrite the chapter
- Report violations only; let the user decide what to fix
