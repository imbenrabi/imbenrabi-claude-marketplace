# PLUGIN_NAME

TODO: Describe what this plugin does.

## Setup

1. Replace `PLUGIN_NAME`, `PLUGIN_DESCRIPTION`, and `AUTHOR_NAME` in `.claude-plugin/plugin.json`
2. Add this plugin to the marketplace's `.claude-plugin/marketplace.json` under the `plugins` array:
   ```json
   {
     "name": "your-plugin-name",
     "source": "./plugins/your-plugin-name",
     "description": "Your plugin description"
   }
   ```
3. Add your skills, commands, and agents

## Structure

```
your-plugin-name/
├── .claude-plugin/
│   └── plugin.json        # Plugin manifest
├── skills/                # Auto-triggering skills
│   └── skill-name/
│       ├── SKILL.md       # Skill definition with frontmatter
│       └── references/    # Optional reference materials
├── commands/              # User-invoked slash commands
│   └── command-name.md
└── agents/                # Subagent definitions
    └── agent-name.md
```

## Adding Components

Use the meta-tools commands to scaffold new components:

```
/meta-tools:create-skill
/meta-tools:create-agent
/meta-tools:create-command
```
