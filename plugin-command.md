---
description: "Browse and install skills from the claude-skills marketplace"
argument-hint: "[list | install <skill-name> | info <skill-name>]"
allowed-tools: Bash(curl -s *), Bash(ls ~/.claude/commands*), Bash(cp *)
---

# Claude Skills Marketplace

The catalog is fetched from:
https://raw.githubusercontent.com/Enki-work/claude-skills/main/catalog.json

**Argument:** $ARGUMENTS

## Behavior

### No argument or "list"

Fetch the catalog with:
```bash
curl -s https://raw.githubusercontent.com/Enki-work/claude-skills/main/catalog.json
```

Display a table of all available skills:

```
╔══════════════╦══════════╦══════════════════════════════════════════════════════╗
║ Name         ║ Category ║ Description                                          ║
╠══════════════╬══════════╬══════════════════════════════════════════════════════╣
║ bot-reply    ║ git      ║ Auto-respond to PR bot comments ...                  ║
╚══════════════╩══════════╩══════════════════════════════════════════════════════╝

Usage: /plugin install <name>
```

### "install <skill-name>"

1. Fetch the catalog and find the entry where `name` matches `<skill-name>`.
2. If not found, show an error listing available skill names.
3. Check if `~/.claude/commands/<skill-name>.md` already exists:
   ```bash
   ls ~/.claude/commands/<skill-name>.md
   ```
   If it exists, ask the user: "Already installed. Overwrite? (y/n)"
4. Download the skill:
   ```bash
   curl -s <command_url> -o ~/.claude/commands/<skill-name>.md
   ```
5. Confirm success:
   ```
   ✓ Installed: <skill-name>
   You can now use /<skill-name> in Claude Code.
   ```

### "info <skill-name>"

Fetch the catalog, find the entry, and display:
- Name, category, description
- Author
- README URL
- Whether it is already installed (`ls ~/.claude/commands/<skill-name>.md`)

## Error Handling

- If `curl` fails (no internet / repo unavailable), show: "Could not reach the marketplace. Check your connection."
- If skill name not found in catalog, list all available names.
