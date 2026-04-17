---
description: Create a new skill for any Claude Code plugin
allowed-tools: Read, Write, Edit, Grep, Glob, Bash
---

# Create a New Skill

Create a new skill for a Claude Code plugin. If the user provided a skill name, description, or target plugin inline, use it. Otherwise, ask.

## Process

### Step 0: Identify Target Plugin

Determine which plugin to create the skill in:

1. If the user provided a path as an argument (`$ARGUMENTS`), use it
2. Search upward from the current directory for `.claude-plugin/plugin.json`
3. If the current directory has `.claude-plugin/marketplace.json`, list its plugins and ask the user which one
4. If none found, ask the user for the plugin path

Read the target plugin's `plugin.json` to confirm the plugin name.

### Step 1: Gather Intent

Ask the user (one question at a time):
- What should this skill do? (one sentence)
- When should it trigger? (what phrases or actions)
- Does it need to modify files (generation) or just read/analyze (review)?
- Does it need reference files? (checklists, templates, patterns)

### Step 2: Check for Duplicates

Search existing skills to avoid overlap:

```bash
ls <plugin-path>/skills/*/SKILL.md 2>/dev/null
```

Read each SKILL.md's `name` and `description` fields. If there's significant overlap, inform the user and suggest extending the existing skill instead.

### Step 3: Generate the Skill Directory

Create:

```
<plugin-path>/skills/<skill-name>/
├── SKILL.md
└── references/          (if needed)
    └── <reference>.md
```

The SKILL.md must follow this exact structure:

```yaml
---
name: <skill-name>
description: >
  <Primary action>. Use this skill when <broad trigger conditions>.
  Triggers on: "<phrase1>", "<phrase2>", "<phrase3>", "<phrase4>",
  "<phrase5>", "<phrase6>", "<phrase7>", "<phrase8>", "<phrase9>", "<phrase10>".
allowed-tools: <tools>
---

# <Skill Title>

## Step 1: Load Context
<Read reference files from ${CLAUDE_SKILL_DIR}/references/>

## Step 2: <Core Action>
<Detailed instructions>

## Step 3: Validate
<Validation or output formatting>

## Output Format
<Exact output specification>
```

### Step 4: Apply These Rules

**Description must be "pushy":**
- Start with the primary action verb: "Generate...", "Review...", "Analyze..."
- Include "Use this skill when..." with broad conditions
- Include "Triggers on:" with 10+ quoted phrases (synonyms, related terms, domain words)
- Over-triggering is better than under-triggering — false positives are cheap to dismiss

**Tool restrictions (principle of least privilege):**

| Skill Type | Tools |
|------------|-------|
| Review / audit | `Read, Grep, Glob` |
| Generation | `Read, Write, Edit, Grep, Glob, Bash` |
| Analysis | `Read, Grep, Glob, Bash` |
| Planning | `Read, Grep, Glob` |

**Reference files:**
- Store in `references/` subdirectory
- One topic per file, standalone and readable
- Use `${CLAUDE_SKILL_DIR}/references/<filename>` in the SKILL.md body

**Output format — every skill must specify one:**
- Review skills: findings grouped by severity (Critical / Warning / Suggestion)
- Generation skills: file listing with locations and key decisions
- Analysis skills: summary with data points and recommendations

### Step 5: Report

Present to the user:
- Files created and their locations
- How to invoke: the skill auto-triggers from conversation context
- The trigger description (what phrases activate it)
- Any reference files the user needs to fill in
