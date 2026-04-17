---
description: Create a new slash command for any Claude Code plugin
allowed-tools: Read, Write, Edit, Grep, Glob, Bash
---

# Create a New Command

Create a new slash command for a Claude Code plugin. Commands are simpler than skills — single .md files with no directory structure.

## When to Use a Command vs a Skill

- **Command:** Simple prompt template, no reference files, single focused task, user-invoked via slash command
- **Skill:** Complex workflow with reference files, specific tool restrictions, auto-triggering via "pushy" descriptions

## Process

### Step 0: Identify Target Plugin

Determine which plugin to create the command in:

1. If the user provided a path as an argument (`$ARGUMENTS`), use it
2. Search upward from the current directory for `.claude-plugin/plugin.json`
3. If the current directory has `.claude-plugin/marketplace.json`, list its plugins and ask the user which one
4. If none found, ask the user for the plugin path

Read the target plugin's `plugin.json` to confirm the plugin name.

### Step 1: Gather Intent

Ask the user:
- What should this command do? (one sentence)
- What's the command name? (kebab-case, e.g., `review`, `estimate`, `changelog`)
- Does it need arguments from the user? (use `$ARGUMENTS` placeholder)
- Does it need dynamic context from shell commands? (use `` !`command` `` syntax)

### Step 2: Check for Duplicates

Search existing commands and skills:

```bash
ls <plugin-path>/commands/*.md <plugin-path>/skills/*/SKILL.md 2>/dev/null
```

If there's overlap, suggest using the existing one or explain the differentiation.

### Step 3: Generate the Command File

Create `<plugin-path>/commands/<name>.md`. Commands can take three forms:

**Basic command (no arguments):**

```markdown
---
description: Brief description of what this command does
---

# Command Title

<Instructions for Claude to follow when this command is invoked.>
```

**Command with user arguments:**

```markdown
---
description: Brief description
---

# Command Title

Perform the task described below:

$ARGUMENTS

Follow these rules:
1. <Rule 1>
2. <Rule 2>
```

**Command with dynamic shell context:**

```markdown
---
description: Brief description
---

# Command Title

Use this context:
!`git log --oneline -10`

Based on the above, <instructions>.
```

### Step 4: Apply These Rules

- File name = command name (kebab-case)
- Keep commands under 50 lines — if it needs more, create a skill instead
- `description:` frontmatter helps users understand the command in listings
- Use `$ARGUMENTS` to capture user input after the command name
- Use `$ARGUMENTS[0]`, `$ARGUMENTS[1]` for positional args
- Use `` !`command` `` for dynamic shell injection (runs at invocation time)
- Commands should complete a single focused task
- No `allowed-tools` needed — commands inherit the session's tool access (but you can restrict with `allowed-tools` if desired)

**Optional frontmatter fields:**

| Field | Description |
|-------|-------------|
| `description` | Short description shown in /help |
| `argument-hint` | Hints for command arguments shown to user |
| `allowed-tools` | Pre-approved tools for this command |
| `model` | Override the model (e.g., "haiku", "sonnet") |

### Step 5: Report

Present to the user:
- File created and location
- How to invoke: `/<plugin-name>:<name>` or `/<plugin-name>:<name> <arguments>`
- Example usage showing a realistic invocation
