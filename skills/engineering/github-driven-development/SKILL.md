---
name: github-driven-development
description: Makes GitHub Issues the single source of truth for work — every task must exist as a GitHub issue before implementation, specs are written into the issue, and phased work spanning multiple PRs/worktrees is split into sub-issues. Use when implementing features, fixing bugs, writing specs, or doing any non-trivial development work in a project that tracks work in GitHub Issues.
---

# GitHub-Driven Development

GitHub Issues is the source of truth. No meaningful work happens that isn't represented by a GitHub issue first, and the issue holds the **spec** — the what and the why. The detailed implementation plan stays out of GitHub unless it becomes sub-issues.

## GitHub access

This skill is tool-agnostic. Use whatever GitHub integration is available — the `gh` CLI (preferred when installed), a GitHub MCP server (`mcp__*` GitHub tools), or the REST/GraphQL API. If no GitHub tooling is available, stop and ask the user to set one up (e.g. `gh auth login`) before proceeding. Never fabricate issue numbers or pretend an issue exists.

Resolve the target repository from the current git remote (`gh repo view`). If the work belongs in a different repo than the one you're in, confirm the repo with the user first.

## 1. The gate: work must exist in GitHub first

Before doing any non-trivial work, check whether a GitHub issue covers it (`gh issue list --search "..."`).

- **Issue exists** → work against it. Confirm the issue with the user before starting.
- **No issue** → before touching code, create one. Write the task and its context into the issue, then confirm with the user that the captured task is correct.

**Exempt from the gate** (no issue required): answering questions, reading or explaining code, exploration, and truly trivial one-liners. When in doubt about whether something is trivial, ask rather than skipping the gate.

Do not begin implementation until the relevant issue exists and the user has acknowledged it.

## 2. Specs go into the issue — plans do not

When the user writes or co-authors a spec or plan, the **spec** is written into the GitHub issue:

- The issue **body** holds the canonical spec — the problem, the desired behavior, constraints, and acceptance criteria. Edit the body in place as the spec evolves so it stays canonical.
- Revisions and discussion go in issue **comments**, so the spec's history is preserved.

The **detailed implementation plan** — the step-by-step breakdown, the checklist of edits — does **not** go into the GitHub issue. Keep it in the working session (e.g. a local plan doc or your todo list). The plan only enters GitHub when it is split into sub-issues (see below). Never paste an implementation checklist into the parent issue body.

## 3. Sub-issues: only when the work actually splits

Decide based on how the work ships, not on how many conceptual phases it has:

- **Multi-phase, but shipped in one PR / one worktree** → keep it a single issue. No sub-issues. Track the phases in your local plan, not in GitHub.
- **Phased release spanning multiple PRs or multiple worktrees** → create one sub-issue per phase/PR under the parent issue. Each sub-issue gets its own scoped spec; the parent holds the overall spec and links its children.

Prefer GitHub's native **sub-issues** to attach children to the parent. If native sub-issues aren't available through your tooling, fall back to a task list in the parent body (`- [ ] #123`) — GitHub tracks those as linked issues and shows progress on the parent. Either way, every child body should start with a reference back to the parent (`Part of #<parent>`).

In short: a sub-issue exists for each independently-shipped unit of work, and nothing else. This is the only form in which the implementation breakdown belongs in GitHub.

## 4. Link PRs to issues

Every PR must reference the issue it implements. Put a closing keyword in the PR body (`Closes #123`) so GitHub closes the issue on merge. One PR per sub-issue; a PR that closes a sub-issue should not also close the parent — the parent closes when all its children are done.

## 5. Proactive fixes — always approval-gated

While working, watch for adjacent issues, and surface them — but never act without approval:

- **Surface related backlog** — if you notice open GitHub issues related to what you're touching (bugs, tech-debt), mention them and offer to address them too.
- **Offer to file new issues** — if you discover a bug or tech-debt that isn't tracked, offer to create a GitHub issue for it.
- **Convert TODO comments into issues** — when you come across `TODO` comments in the codebase, offer to back them with GitHub issues. If an issue already exists for that TODO, reuse it; otherwise create a new one whose body captures the context. Apply the relevant labels and milestone (or add it to the relevant GitHub Project), and reference any blocking or related issues in the body. Once the issue exists, link it back in the TODO comment (e.g. `// TODO(#123): ...`).

In both cases: propose, then wait for explicit approval. Do not create issues or start fixing related work silently.

## 6. Parallel implementation via subagents

When several independent sub-issues are ready, you may implement them in parallel using subagents — pairs naturally with git worktrees for isolation.

- **Offer first.** Tell the user which sub-issues can run in parallel and propose dispatching subagents.
- **Dispatch only after confirmation.** Never spin up parallel subagents without explicit approval.
- Give each subagent a single sub-issue, its scoped spec, and an isolated workspace. Keep one PR per sub-issue, each closing its own sub-issue.

## The rhythm

1. Gate: ensure a GitHub issue exists for the work (create + confirm if not).
2. Write the spec into the issue body; keep the implementation plan local.
3. If the work ships across multiple PRs/worktrees, split into sub-issues.
4. Reference the issue from every PR with a closing keyword.
5. Surface related/new issue opportunities — offer, don't act.
6. Offer parallel subagent execution for independent sub-issues; dispatch only on approval.
