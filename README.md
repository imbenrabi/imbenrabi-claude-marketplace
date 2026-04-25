# imbenrabi Claude Marketplace

Personal Claude Code marketplace housing coding standards, agent behavioral guidelines, meta-tools, and plugin templates.

## Quick Start

The most reliable path — works regardless of which Claude surface you use later. Run from any terminal:

```bash
claude plugin marketplace add imbenrabi/claude-marketplace
claude plugin install coding-standards@imbenrabi --scope user
claude plugin install agent-guidelines@imbenrabi --scope user
claude plugin install meta-tools@imbenrabi --scope user
claude plugin list
```

Plugins land in `~/.claude/plugins/` at user scope, so every Claude Code session on this machine picks them up — terminal, the Claude Code desktop app, IDE extensions.

Verify by starting a `claude` session and confirming the meta-tools commands appear:

```
/meta-tools:create-skill
```

Skills auto-trigger from conversation context; commands are invoked as `/<plugin>:<name>`.

## Installation

Claude Code runs in two surfaces that both use the same plugin system:

- **Claude Code (command line)** — the `claude` CLI in your terminal.
- **Claude Desktop** — the macOS/Windows desktop app.

Both surfaces share `~/.claude/settings.json` and both expose the `/plugin` interface in chat. The sections below show the install flow for each surface separately so you can pick the one that matches how you use Claude Code.

### Claude Code (command line)

Use this section if you run `claude` from a terminal.

#### Option A — GitHub source (recommended for consumers)

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

Then in a `claude` session:

```
/plugin install coding-standards@imbenrabi
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

#### Option B — Local directory (for development on this marketplace)

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
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

For pure local iteration without registering the marketplace, the CLI also accepts `claude --plugin-dir ./plugins/<plugin-name>` to load a single plugin directly from disk.

#### Option C — Interactive registration

In any `claude` session, register and install in one go:

```
/plugin marketplace add imbenrabi/claude-marketplace
/plugin install coding-standards@imbenrabi
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

For a local checkout, swap the first command for `/plugin marketplace add /absolute/path/to/claude-marketplace`.

#### Enabling plugins

After installation, plugins are enabled by default. To explicitly control state in `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "coding-standards@imbenrabi": true,
    "agent-guidelines@imbenrabi": true,
    "meta-tools@imbenrabi": true
  }
}
```

Install only the plugins you need — they are independent.

#### Updating

For GitHub source, plugins update automatically on session start (when auto-update is enabled for the marketplace). For local directory source, pull the latest changes and run `/reload-plugins`.

---

### Claude Desktop

Use this section if you use the Claude Code desktop app on macOS or Windows. The mechanics are the same as the CLI — the desktop app reads the same `~/.claude/settings.json` and exposes the same `/plugin` slash command in chat — but the entry points differ.

#### Option A — GitHub source via the `/plugin` UI

1. Open the Claude Code desktop app.
2. In a chat, run `/plugin marketplace add imbenrabi/claude-marketplace`.
3. Run `/plugin` to open the plugin manager. Tab to **Discover** to browse plugins from this marketplace, or install directly:
   ```
   /plugin install coding-standards@imbenrabi
   /plugin install agent-guidelines@imbenrabi
   /plugin install meta-tools@imbenrabi
   ```
4. Run `/reload-plugins` to activate.

#### Option B — GitHub source via `settings.json`

If you'd rather configure marketplaces declaratively, edit `~/.claude/settings.json` (the desktop app reads the same file as the CLI on macOS):

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

Then in the desktop chat:

```
/plugin install coding-standards@imbenrabi
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

#### Option C — Local directory (for development)

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

Then install via `/plugin install ...` in chat as above.

#### Managing plugins

Inside the desktop app, `/plugin` opens an interactive manager with four tabs (cycle with `Tab`):

- **Discover** — browse available plugins from your marketplaces.
- **Installed** — view, enable, disable, or uninstall plugins.
- **Marketplaces** — add, remove, or update marketplaces.
- **Errors** — view plugin loading errors.

#### Updating

Same as the CLI: GitHub-source marketplaces auto-update on session start; for local directory sources, pull the repo and run `/reload-plugins`.

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

### agent-guidelines

Auto-triggering skills that steer **how** the assistant approaches coding tasks (process), distinct from `coding-standards` which governs **what** the produced code looks like (output style).

| Skill | Triggers when | Guidelines enforced |
|-------|---------------|---------------------|
| `karpathy-guidelines` | Writing, reviewing, refactoring, or planning any non-trivial code change | Think before coding, simplicity first, surgical changes, goal-driven execution |

Derived from [Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876).

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
│   ├── agent-guidelines/          # Auto-triggering behavioral guidelines
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── README.md
│   │   └── skills/
│   │       └── karpathy-guidelines/
│   │           └── SKILL.md
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
