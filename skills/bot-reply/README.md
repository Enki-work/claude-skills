# bot-reply

Automatically review, respond to, and (when needed) fix code based on unresolved bot comments on a GitHub Pull Request.

Supports comments from:
- **coderabbitai**
- **github-copilot** / **copilot**
- **chatgpt-codex-connector**

## Prerequisites

- Claude Code CLI
- GitHub CLI (`gh`) authenticated
- MCP GitHub plugin (optional but recommended)

## Install

```bash
curl -o ~/.claude/commands/bot-reply.md \
  https://raw.githubusercontent.com/Enki-work/claude-skills/main/skills/bot-reply/command.md
```

## Usage

```
/bot-reply https://github.com/org/repo/pull/123
```

## What It Does

1. **Fetches** all unresolved bot comments (inline review + general comments)
2. **Classifies** each comment as actionable or ignorable
3. **Fixes** the code if needed → commits → pushes
4. **Replies** to each comment with a clear explanation (in the comment's language)
5. **Reports** a summary of all actions taken

## Decision Logic

| Situation | Action |
|-----------|--------|
| Real bug / security issue | Fix code, commit, push, reply with fix summary |
| Style / quality improvement | Fix code, commit, push, reply with fix summary |
| False positive / out of scope | Reply explaining why no change was made |
| Already addressed | Reply confirming it was handled |

## Example Output

```
## 対応完了

- 対応して修正・push: 2件
- 対応不要として返信: 1件
- 合計: 3件
```

## How It Works

- Uses `mcp__plugin_github_github__pull_request_read` as the primary method
- Falls back to `gh api` CLI when MCP is unavailable
- One commit per addressed comment for clean history
- Replies match the language of the original bot comment
