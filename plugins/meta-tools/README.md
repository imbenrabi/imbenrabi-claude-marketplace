# meta-tools

Slash commands for scaffolding new skills, agents, and commands in any Claude Code plugin.

## Commands

| Command | Purpose |
|---------|---------|
| `/meta-tools:create-skill` | Generate a new skill with SKILL.md and optional references |
| `/meta-tools:create-agent` | Generate a new subagent definition |
| `/meta-tools:create-command` | Generate a new slash command |

## Usage

Each command interactively walks you through the creation process:

1. Identifies the target plugin (asks if ambiguous)
2. Gathers intent (what should it do, when should it trigger)
3. Checks for duplicates
4. Generates the files following Claude Code plugin conventions

## Target Plugin Discovery

Commands automatically find the target plugin by:

1. Checking if the current directory is inside a plugin (has `.claude-plugin/plugin.json`)
2. Checking if the current directory is a marketplace (has `.claude-plugin/marketplace.json`)
3. If neither, asking the user to specify a path

You can also pass the plugin path as an argument: `/meta-tools:create-skill /path/to/plugin`
