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

Plugins are a Claude Code feature. They live in `~/.claude/plugins/` and are registered in `~/.claude/settings.json`. Every Claude Code surface on this machine — terminal CLI, the Claude Code desktop app, IDE extensions — reads the same state.

Install commands have two equivalent forms:

- **Shell form** (`claude plugin …`) — runs in any terminal, including a non-Claude one. Most reliable: works regardless of the surface you'll use later, including surfaces that don't expose slash commands.
- **Slash form** (`/plugin …`) — runs inside a Claude Code session. Available in the terminal CLI and the Claude Code desktop app. **Not** available in the consumer Claude desktop chat app at claude.ai/download.

The sections below cover the install flow for each surface separately, but the `claude plugin install …` shell form works for both.

### Claude Code (command line)

Use this section if you run `claude` from a terminal.

#### Option A — GitHub source (recommended for consumers)

From any terminal:

```bash
claude plugin marketplace add imbenrabi/claude-marketplace
claude plugin install coding-standards@imbenrabi --scope user
claude plugin install agent-guidelines@imbenrabi --scope user
claude plugin install meta-tools@imbenrabi --scope user
```

Equivalent slash-command form inside a `claude` session:

```
/plugin marketplace add imbenrabi/claude-marketplace
/plugin install coding-standards@imbenrabi
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

If you'd rather declare the marketplace in `~/.claude/settings.json` so it persists without an `add` step:

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

You still need to run the install commands above — `extraKnownMarketplaces` only registers the catalog; it does not install plugins from it.

#### Option B — Local directory (for development on this marketplace)

From any terminal:

```bash
claude plugin marketplace add /absolute/path/to/claude-marketplace
claude plugin install coding-standards@imbenrabi --scope user
claude plugin install agent-guidelines@imbenrabi --scope user
claude plugin install meta-tools@imbenrabi --scope user
```

Or via `settings.json`:

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

…then run the `claude plugin install …` commands above.

For pure local iteration without registering the marketplace at all, `claude --plugin-dir ./plugins/<plugin-name>` loads a single plugin directly from disk for one session (not persisted).

#### Enabling and disabling installed plugins

Plugins are enabled by default after install. To toggle state:

```bash
claude plugin disable agent-guidelines@imbenrabi
claude plugin enable agent-guidelines@imbenrabi
```

`claude plugin install / uninstall / enable / disable` writes to `~/.claude/settings.json` under `enabledPlugins`. Editing that key by hand only flips the state of plugins that are already installed — it does **not** install missing ones.

#### Updating

For GitHub source, plugins auto-update on session start (when auto-update is enabled for the marketplace). For local directory source, pull the latest changes and run `claude plugin marketplace update imbenrabi` from a terminal, or `/reload-plugins` inside a session.

---

### Claude Desktop

Two different products are commonly called "Claude Desktop", and plugin support differs:

| Product | Where you get it | `/plugin` slash command in chat? | Recommended install path |
|---------|------------------|----------------------------------|--------------------------|
| **Claude Code desktop app** | distributed alongside Claude Code | ✓ | Use the in-chat `/plugin` UI, or `claude plugin install …` from a terminal — both work. |
| **Claude (consumer chat app)** | claude.ai/download | ✗ | Use `claude plugin install …` from a terminal. The chat app does not expose `/plugin`. |

In both cases, plugins live in `~/.claude/plugins/` once installed at user scope.

#### Claude Code desktop app

Inside a chat:

```
/plugin marketplace add imbenrabi/claude-marketplace
/plugin install coding-standards@imbenrabi
/plugin install agent-guidelines@imbenrabi
/plugin install meta-tools@imbenrabi
/reload-plugins
```

`/plugin` (no args) opens the interactive manager with four tabs (cycle with `Tab`):

- **Discover** — browse plugins from your marketplaces.
- **Installed** — view, enable, disable, or uninstall plugins.
- **Marketplaces** — add, remove, or update marketplaces.
- **Errors** — view plugin loading errors.

You can also configure marketplaces declaratively in `~/.claude/settings.json` (see Option A of the CLI section above), but you still need to run the install commands afterward.

#### Claude consumer desktop chat app

The `/plugin` slash command is **not** available in this surface. Install from a terminal instead:

```bash
claude plugin marketplace add imbenrabi/claude-marketplace
claude plugin install coding-standards@imbenrabi --scope user
claude plugin install agent-guidelines@imbenrabi --scope user
claude plugin install meta-tools@imbenrabi --scope user
```

Then start (or restart) a chat. Whether installed plugins/skills are loaded by the consumer chat app depends on how the embedded Claude Code agent is wired in your version — verify by checking whether `karpathy-guidelines` auto-triggers when you ask Claude to "refactor this function" in a fresh chat. If they don't load, plugins management is currently a CLI / Claude Code desktop app feature only; use those surfaces.

#### Updating

GitHub-source marketplaces auto-update on session start (when auto-update is enabled). For local directory sources, run `claude plugin marketplace update imbenrabi` from a terminal, or `/reload-plugins` inside a Claude Code session.

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
