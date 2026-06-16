# skills

Antoni's collection of Claude Code skills — straight from my `.claude` directory. Small, composable, easy to adapt.

## Skills

| Skill | Category | What it does |
| --- | --- | --- |
| [atomic-commits](skills/engineering/atomic-commits/SKILL.md) | engineering | Enforces an atomic commit workflow — breaks implementation into small, self-contained units and commits after each one. |
| [linear-driven-development](skills/engineering/linear-driven-development/SKILL.md) | engineering | Makes Linear the source of truth — every task becomes a Linear issue before implementation, specs live in the issue, and phased work spanning multiple PRs/worktrees splits into sub-issues. |
| [optimize-for-x](skills/content/optimize-for-x/SKILL.md) | content | Optimizes a post for the X "For You" algorithm — drafts variants from a topic, or critiques and rewrites an existing draft. |

## Install

Install any skill with one line using the `skills` CLI:

```bash
npx skills@latest add bring-shrubbery/skills/atomic-commits
npx skills@latest add bring-shrubbery/skills/optimize-for-x
```

## Commands

Some skills ship with companion slash commands (in [`commands/`](commands/)):

- `/auto-commit` — enable automatic atomic commits for the session (pairs with `atomic-commits`).
- `/optimize-for-x <topic OR draft>` — invoke `optimize-for-x` explicitly with an argument.

## License

MIT — see [LICENSE](LICENSE).
