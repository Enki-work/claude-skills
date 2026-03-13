# Claude Skills Marketplace

A curated collection of [Claude Code](https://claude.ai/claude-code) slash command skills — install powerful `/commands` into your local workflow in seconds.

## Quick Start

### Step 1 — Install the `/plugin` command (one-time)

```bash
curl -o ~/.claude/commands/plugin.md \
  https://raw.githubusercontent.com/Enki-work/claude-skills/main/plugin-command.md
```

### Step 2 — Browse the marketplace

```
/plugin list
```

### Step 3 — Install any skill

```
/plugin install bot-reply
```

---

## Available Skills

| Name | Category | Description |
|------|----------|-------------|
| [bot-reply](./skills/bot-reply/) | git | Auto-respond to PR bot comments (coderabbitai / copilot / chatgpt-codex-connector) with fixes or explanations |

---

## Manual Install (without /plugin)

```bash
# bot-reply
curl -o ~/.claude/commands/bot-reply.md \
  https://raw.githubusercontent.com/Enki-work/claude-skills/main/skills/bot-reply/command.md
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI installed
- GitHub CLI (`gh`) for skills that interact with GitHub
- MCP GitHub plugin (optional, used as primary method when available)

## Contributing

Want to add your own skill? See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT
