---
description: Create a new subagent definition for any Claude Code plugin
allowed-tools: Read, Write, Edit, Grep, Glob, Bash
---

# Create a New Agent

Create a new subagent definition for a Claude Code plugin. If the user provided a name or description inline, use it. Otherwise, ask.

## Process

### Step 0: Identify Target Plugin

Determine which plugin to create the agent in:

1. If the user provided a path as an argument (`$ARGUMENTS`), use it
2. Search upward from the current directory for `.claude-plugin/plugin.json`
3. If the current directory has `.claude-plugin/marketplace.json`, list its plugins and ask the user which one
4. If none found, ask the user for the plugin path

Read the target plugin's `plugin.json` to confirm the plugin name.

### Step 1: Gather Intent

Ask the user (one question at a time):
- What task does this agent perform? (one sentence)
- Is it read-only (review/audit) or read-write (generation/modification)?
- Which existing skills should be preloaded? (list from `<plugin-path>/skills/*/SKILL.md`)
- What model tier? (`haiku` for simple review, `sonnet` for generation, omit to inherit)
- Should it use persistent memory? (`project` for agents that learn over time)

### Step 2: Check for Duplicates

Search existing agents:

```bash
ls <plugin-path>/agents/*.md 2>/dev/null
```

Read each agent's `name` and `description`. If there's overlap, suggest modifying the existing agent or explain the differentiation.

### Step 3: Generate the Agent File

Create `<plugin-path>/agents/<agent-name>.md` following this template:

```yaml
---
name: <agent-name>
description: >
  <Primary purpose>. Use proactively when <trigger conditions>.
  Triggers on: "<phrase1>", "<phrase2>", "<phrase3>", ...
tools: <comma-separated tool list>
skills:
  - <skill-name>
model: <haiku|sonnet>
memory: <project|none>
---

<System prompt defining the agent's role, process, and constraints.>
```

### Step 4: Apply These Rules

**Naming:** kebab-case with `-agent` suffix (e.g., `review-agent`, `test-writer-agent`)

**Description:** same "pushy" rules as skills — 10+ trigger phrases, broad conditions

**Tool restrictions by access level:**

| Access | Tools |
|--------|-------|
| Read-only (review/audit) | `Read, Grep, Glob` |
| Read + git | `Read, Grep, Glob, Bash` (Bash for git commands only) |
| Full (generation) | `Read, Write, Edit, Grep, Glob, Bash` |

**Model selection:**

| Task Type | Model | Reason |
|-----------|-------|--------|
| Simple review, linting | `haiku` | Fast, cheap, sufficient for pattern matching |
| Code generation, complex analysis | `sonnet` | Better reasoning, higher quality output |
| Complex multi-step orchestration | omit (inherit) | Uses the parent session's model |

**System prompt must include:**
- Agent's role and expertise (e.g., "You are a senior frontend engineer...")
- What it does on invocation (standard process/steps)
- Constraints (especially for read-only agents: "Do NOT modify any files")
- References to preloaded skills if any

**Memory:**
- Use `memory: project` when the agent should learn preferences over time
- Omit for stateless agents

**Optional advanced fields:**

| Field | Description |
|-------|-------------|
| `permissionMode` | Override permission mode |
| `disallowedTools` | Explicitly block specific tools |
| `isolation` | `worktree` for isolated git operations |
| `mcpServers` | Scoped MCP server access |

### Step 5: Report

Present to the user:
- File created and location
- Preloaded skills and what context they provide
- Tool restrictions and why
- How to invoke: ask Claude to delegate to `<plugin-name>:<agent-name>`
