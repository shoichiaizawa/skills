# Skills

Portable coding agent skills, distributed via the
[`skills` CLI](https://github.com/vercel-labs/skills).

## Available skills

| Skill | Description |
| --- | --- |
| `/commit` | Create clean, atomic git commits with Conventional Commits format |
| `/parallel` | Orchestrate parallel Claude Code sessions in separate git worktrees |

## Install

```bash
npx skills add shoichiaizawa/skills -a claude-code -g -y
```

| Flag | Purpose |
| --- | --- |
| `-a <agents...>` | Target specific agents (see [Multi-agent support](#multi-agent-support)) |
| `-g` | Install globally (available in all projects) |
| `-y` | Skip confirmation prompts |
| `-s <name>` | Install a single skill by name |

To install a single skill:

```bash
npx skills add shoichiaizawa/skills -s commit -a claude-code -g -y
```

### Multi-agent support

The CLI supports many coding agents. Use `-a` to target one or more:

```bash
# Single agent
npx skills add shoichiaizawa/skills -a claude-code -g -y

# Multiple agents
npx skills add shoichiaizawa/skills -a claude-code cursor opencode -g -y

# All detected agents
npx skills add shoichiaizawa/skills -a '*' -g -y
```

Skills are stored once in a canonical location (`~/.agents/skills/<name>/`) and
symlinked into each agent's directory. Some agents — including Cursor, Codex,
Gemini CLI, GitHub Copilot, and OpenCode — read directly from
`~/.agents/skills/` (no symlink needed). Others — including Claude Code, Cline,
and Kiro — use their own directories (e.g. `~/.claude/skills/<name>/`) with
symlinks back to the canonical copy.

### Multiple environments

The CLI respects environment variables for agent-specific installation paths:

```bash
# Personal
npx skills add shoichiaizawa/skills -a claude-code -g -y

# Enterprise (separate Claude Code config directory)
CLAUDE_CONFIG_DIR=~/.claude-enterprise npx skills add shoichiaizawa/skills -a claude-code -g -y
```

Other environment variables: `CODEX_HOME` (Codex), `XDG_CONFIG_HOME` (OpenCode,
Goose, Amp, and others).

## Update

```bash
# Check for updates
npx skills check

# Apply updates
npx skills update
```

> **Note**: `update` does a full reinstall — local modifications to installed
> skill files will be overwritten.

## Remove

```bash
# Interactive selection
npx skills remove -g

# Remove a specific skill
npx skills remove commit -g
```

## Other useful commands

```bash
# List installed skills
npx skills list -g

# Preview available skills without installing
npx skills add shoichiaizawa/skills -l
```

## Repository structure

```
skills/
├── commit/
│   ├── SKILL.md
│   ├── scripts/
│   │   ├── analyse-changes.sh
│   │   ├── effort-level.sh
│   │   ├── pre-flight.sh
│   │   └── validate-message.sh
│   └── references/
│       └── message-format.md
└── parallel/
    ├── SKILL.md
    └── scripts/
        ├── cleanup.sh
        ├── launch.sh
        ├── merge.sh
        └── status.sh
```

The CLI discovers skills by scanning the `skills/` directory for subdirectories
containing a `SKILL.md` file.

## SKILL.md format

Each skill requires YAML frontmatter with `name` and `description`:

```yaml
---
name: my-skill
description: Brief explanation of what this skill does.
---

# /my-skill

Instructions for the agent.
```
