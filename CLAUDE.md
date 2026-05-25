# CLAUDE.md

This repo is a collection of Claude Code skills, packaged as a single plugin.

## Layout

- `skills/<category>/<skill-name>/SKILL.md` — one skill per directory. Categories so far: `engineering`, `content`. Add a new category folder when an existing one doesn't fit.
- `commands/<name>.md` — optional companion slash commands (e.g. a thin wrapper that invokes a skill with `$ARGUMENTS`).
- `.claude-plugin/plugin.json` — plugin manifest. Lists every skill directory and points to `commands/`.

## Adding a skill

When you add a skill under `skills/`, it MUST also be:

1. Listed in `.claude-plugin/plugin.json` under `skills` (path to the skill directory).
2. Added as a row in the README's skill table (name · category · what it does).

Keep these three in sync. A skill that isn't in `plugin.json` won't ship; a skill that isn't in the README is undiscoverable.

## SKILL.md format

YAML frontmatter with `name` and `description`, then Markdown. The `description` is what triggers model auto-invocation, so make it say *when* to use the skill, not just what it is. Keep skills small and composable.

## Companion commands

If a skill should also be invokable explicitly with an argument, add a thin `commands/<name>.md` wrapper that delegates to the skill (don't duplicate the skill's body). See `commands/optimize-for-x.md`.
