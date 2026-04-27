# AGENTS.md

This repository is a personal skill library following the [Anthropic Skill](https://docs.claude.com/en/docs/claude-code/skills) format.

## Where skills live

Reusable skills are under `skills/<domain>/<skill-name>/SKILL.md`. Each `SKILL.md` carries a YAML frontmatter with `name` and `description`. The `description` field states the trigger condition explicitly (e.g. "Use when the user provides one or more reference images and wants to extract the underlying visual style ..."). Match it against the user intent before invoking.

## Agent-specific entry points

The repo also maintains three flat symlink trees for direct agent discovery:

- `.claude/skills/<name>` — Claude Code
- `.agent/skills/<name>`  — Codex, OpenCode, and other agents that read `~/.agent`
- `.cursor/skills/<name>` — Cursor (repo-level)

Each symlink resolves to the same source under `skills/<domain>/<skill-name>/`. Edit the source, never the symlink.

## Things that are NOT skills

- `templates/SKILL.template.md` — scaffold for new skills, do not load it.
- `LICENSE`, `README.md`, `AGENTS.md` — repository docs.

## Adding or editing skills

See the "Adding a New Skill" section in [README.md](README.md). Keep `frontmatter.name` and the directory name identical kebab-case strings. Never commit credentials.
