# Claude Skills Marketplace

A curated marketplace of [Claude Code](https://claude.ai/claude-code) skills — install powerful slash commands via the native plugin system.

## Install via Claude Code Plugin System

### Step 1 — Register this marketplace

```bash
claude plugin marketplace add Enki-work/claude-skills
```

### Step 2 — Browse available skills

```
/plugin marketplace list
```

### Step 3 — Install a skill

```bash
claude plugin install bot-reply@claude-skills
```

Or interactively in Claude Code:

```
/plugin install bot-reply@claude-skills
```

---

## Available Skills

| Name | Description |
|------|-------------|
| [bot-reply](./plugins/bot-reply/) | Auto-respond to PR bot comments (coderabbitai / copilot / chatgpt-codex-connector) |

---

## Manual Install (without plugin system)

```bash
curl -o ~/.claude/commands/bot-reply.md \
  https://raw.githubusercontent.com/Enki-work/claude-skills/main/plugins/bot-reply/commands/bot-reply.md
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI
- GitHub CLI (`gh`) for skills that interact with GitHub
- MCP GitHub plugin (optional, recommended)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT
