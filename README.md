# Claude Skills Marketplace

A curated collection of [Claude Code](https://claude.ai/claude-code) slash command skills — install powerful `/commands` into your local workflow in seconds.

## Available Skills

| Skill | Description | Category |
|-------|-------------|----------|
| [bot-reply](./skills/bot-reply/) | Auto-respond to PR bot comments (coderabbitai / copilot / chatgpt-codex-connector) with fixes or explanations | Git / PR |

## How to Install a Skill

Each skill is a single Markdown file. Copy it to your `~/.claude/commands/` directory:

```bash
# Example: install bot-reply
curl -o ~/.claude/commands/bot-reply.md \
  https://raw.githubusercontent.com/Enki-work/claude-skills/main/skills/bot-reply/command.md
```

Then use it in Claude Code:

```
/bot-reply https://github.com/org/repo/pull/123
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI installed
- GitHub CLI (`gh`) for skills that interact with GitHub
- MCP GitHub plugin (optional, used as primary method when available)

## Contributing

Want to add your own skill? See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT
