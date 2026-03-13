# Contributing to Claude Skills Marketplace

Thank you for contributing! Here's how to add a new skill.

## Skill Structure

Each skill lives in its own directory under `skills/`:

```
skills/
└── your-skill-name/
    ├── command.md   ← The Claude Code slash command file (required)
    └── README.md    ← Documentation (required)
```

## command.md Format

Must include frontmatter:

```markdown
---
description: "One-line description of what this skill does"
argument-hint: "<ARGUMENT_HINT>"
allowed-tools: Read, Edit, Bash(git *), ...
---

# Skill Title

...instructions for Claude...
```

## README.md Requirements

- What the skill does
- Prerequisites
- Installation command (`curl` one-liner)
- Usage examples
- How it works (brief)

## Submission Steps

1. Fork this repository
2. Create your skill directory under `skills/your-skill-name/`
3. Add `command.md` and `README.md`
4. Update the table in the root `README.md`
5. Open a Pull Request

## Guidelines

- Skills should be focused on a single task
- `allowed-tools` should be as minimal as possible
- Always ask for user confirmation before destructive actions
- Support both MCP and CLI fallback where possible
