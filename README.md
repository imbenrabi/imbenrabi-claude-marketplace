# imbenrabi Claude Marketplace

Personal Claude Code marketplace housing coding standards, meta-tools, and plugin templates.

## Quick Start

```bash
# 1. Clone the repo
git clone git@github.com:imbenrabi/claude-marketplace.git

# 2. In a Claude Code session, register the marketplace and install plugins
/plugin marketplace add /absolute/path/to/claude-marketplace
/plugin install coding-standards@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins

# 3. Verify — type /meta-tools: and confirm commands appear
/meta-tools:create-skill
```

That's it. Skills auto-trigger from conversation context; commands are invoked via `/meta-tools:<name>`.

## Installation

### Option 1: GitHub (recommended for consumers)

Add to `~/.claude/settings.json` under `extraKnownMarketplaces`:

```json
{
  "extraKnownMarketplaces": {
    "imbenrabi": {
      "source": {
        "source": "github",
        "repo": "imbenrabi/claude-marketplace"
      }
    }
  }
}
```

Then install the plugins:

```
/plugin install coding-standards@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

### Option 2: Local directory (for development)

```json
{
  "extraKnownMarketplaces": {
    "imbenrabi": {
      "source": {
        "source": "directory",
        "path": "/absolute/path/to/claude-marketplace"
      }
    }
  }
}
```

Then install the plugins:

```
/plugin install coding-standards@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

### Option 3: Interactive registration

In any Claude Code session:

```
/plugin marketplace add /absolute/path/to/claude-marketplace
/reload-plugins
/plugin install coding-standards@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

### Enabling plugins

After installation, enable in `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "coding-standards@imbenrabi": true,
    "meta-tools@imbenrabi": true
  }
}
```

Or install only the plugins you need — they are independent.

### Updating

For GitHub source, plugins update automatically. For local directory source, pull the latest changes and run `/reload-plugins`.

## Plugins

### coding-standards

Auto-triggering skills that enforce consistent coding standards across four languages. These activate automatically when you write or review code — no slash command needed.

| Skill | Triggers when | Standards enforced |
|-------|---------------|--------------------|
| `typescript-standards` | Writing or reviewing `.ts`/`.tsx` files | TSDoc, no `any`, pure functions |
| `python-standards` | Writing or reviewing `.py` files | Google docstrings, type hints, dataclasses |
| `go-standards` | Writing or reviewing `.go` files | GoDoc, proper structs, error wrapping |
| `rust-standards` | Writing or reviewing `.rs` files | Rustdoc, `Result`/`Option`, derive macros |

All languages share three core principles:
1. **Pure functions** — no side effects, single responsibility, small helpers
2. **Proper types** — no `any`/`interface{}`/`Box<dyn Any>`, explicit structs and interfaces
3. **Documentation** — every public function documented, no inline "what" comments

Each skill includes `references/examples.md` with before/after code samples showing poor vs correct implementations.

### meta-tools

Slash commands for scaffolding new plugin components in any marketplace:

| Command | What it creates |
|---------|-----------------|
| `/meta-tools:create-skill` | `skills/<name>/SKILL.md` with frontmatter, trigger phrases, and optional `references/` directory |
| `/meta-tools:create-agent` | `agents/<name>.md` with model selection, tool restrictions, and system prompt |
| `/meta-tools:create-command` | `commands/<name>.md` with argument handling and dynamic context |

Each command walks you through the process interactively:
1. Identifies the target plugin (asks if ambiguous)
2. Gathers intent
3. Checks for duplicates
4. Generates files following Claude Code conventions

### _plugin-template

Not an installable plugin. Copy this directory as a starting point when creating new plugins:

```bash
cp -r plugins/_plugin-template plugins/my-new-plugin
```

Then edit `my-new-plugin/.claude-plugin/plugin.json`, add it to `.claude-plugin/marketplace.json`, and populate with skills/commands/agents using the meta-tools.

## Repository Structure

```
imbenrabi-claude-marketplace/
├── .claude-plugin/
│   └── marketplace.json           # Root marketplace manifest
├── plugins/
│   ├── coding-standards/          # Auto-triggering language standards
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── README.md
│   │   └── skills/
│   │       ├── typescript-standards/
│   │       │   ├── SKILL.md
│   │       │   └── references/examples.md
│   │       ├── python-standards/
│   │       │   ├── SKILL.md
│   │       │   └── references/examples.md
│   │       ├── go-standards/
│   │       │   ├── SKILL.md
│   │       │   └── references/examples.md
│   │       └── rust-standards/
│   │           ├── SKILL.md
│   │           └── references/examples.md
│   ├── meta-tools/                # Scaffolding slash commands
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── README.md
│   │   └── commands/
│   │       ├── create-skill.md
│   │       ├── create-agent.md
│   │       └── create-command.md
│   └── _plugin-template/          # Copy-paste template (not installable)
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── README.md
│       ├── skills/.gitkeep
│       ├── commands/.gitkeep
│       └── agents/.gitkeep
└── README.md
```
