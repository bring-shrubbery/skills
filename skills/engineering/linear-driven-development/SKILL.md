---
name: linear-driven-development
description: Makes Linear the single source of truth for work — every task must exist as a Linear issue before implementation, specs are written into the issue, and phased work spanning multiple PRs/worktrees is split into sub-issues. Use when implementing features, fixing bugs, writing specs, or doing any non-trivial development work in a project that tracks work in Linear.
---

# Linear-Driven Development

Linear is the source of truth. No meaningful work happens that isn't represented by a Linear issue first, and the issue holds the **spec** — the what and the why. The detailed implementation plan stays out of Linear unless it becomes sub-issues.

## Linear access

This skill is tool-agnostic. Use whatever Linear integration is connected — an MCP server (`mcp__*` Linear tools), a CLI, or the API. If no Linear tooling is available, stop and ask the user to connect one before proceeding. Never fabricate issue IDs or pretend an issue exists.

## 1. The gate: work must exist in Linear first

Before doing any non-trivial work, check whether a Linear issue covers it.

- **Issue exists** → work against it. Confirm the issue with the user before starting.
- **No issue** → before touching code, create one. Write the task and its context into the issue, then confirm with the user that the captured task is correct.

**Exempt from the gate** (no issue required): answering questions, reading or explaining code, exploration, and truly trivial one-liners. When in doubt about whether something is trivial, ask rather than skipping the gate.

Do not begin implementation until the relevant issue exists and the user has acknowledged it.

## 2. Specs go into the issue — plans do not

When the user writes or co-authors a spec or plan, the **spec** is written into the Linear issue:

- The issue **description** holds the canonical spec — the problem, the desired behavior, constraints, and acceptance criteria.
- Revisions and discussion go in issue **comments**, so the spec's history is preserved.

The **detailed implementation plan** — the step-by-step breakdown, the checklist of edits — does **not** go into the Linear issue. Keep it in the working session (e.g. a local plan doc or your todo list). The plan only enters Linear when it is split into sub-issues (see below). Never paste an implementation checklist into the parent issue body.

## 3. Sub-issues: only when the work actually splits

Decide based on how the work ships, not on how many conceptual phases it has:

- **Multi-phase, but shipped in one PR / one worktree** → keep it a single issue. No sub-issues. Track the phases in your local plan, not in Linear.
- **Phased release spanning multiple PRs or multiple worktrees** → create one sub-issue per phase/PR under the parent issue. Each sub-issue gets its own scoped spec; the parent holds the overall spec and links its children.

In short: a sub-issue exists for each independently-shipped unit of work, and nothing else. This is the only form in which the implementation breakdown belongs in Linear.

## 4. Proactive fixes — always approval-gated

While working, watch for adjacent issues, and surface them — but never act without approval:

- **Surface related backlog** — if you notice open Linear tickets related to what you're touching (bugs, tech-debt), mention them and offer to address them too.
- **Offer to file new tickets** — if you discover a bug or tech-debt that isn't tracked, offer to create a Linear issue for it.
- **Convert TODO comments into tickets** — when you come across `TODO` comments in the codebase, offer to back them with Linear issues. If a ticket already exists for that TODO, reuse it; otherwise create a new one whose description captures the context of the issue. Add the ticket to the relevant project and link any blockers or other related issues. Once the ticket exists, link it back in the TODO comment (e.g. `// TODO(ABC-123): ...`).

In both cases: propose, then wait for explicit approval. Do not create issues or start fixing related work silently.

## 5. Parallel implementation via subagents

When several independent sub-issues are ready, you may implement them in parallel using subagents — pairs naturally with git worktrees for isolation.

- **Offer first.** Tell the user which sub-issues can run in parallel and propose dispatching subagents.
- **Dispatch only after confirmation.** Never spin up parallel subagents without explicit approval.
- Give each subagent a single sub-issue, its scoped spec, and an isolated workspace. Keep one PR per sub-issue.

## The rhythm

1. Gate: ensure a Linear issue exists for the work (create + confirm if not).
2. Write the spec into the issue; keep the implementation plan local.
3. If the work ships across multiple PRs/worktrees, split into sub-issues.
4. Surface related/new ticket opportunities — offer, don't act.
5. Offer parallel subagent execution for independent sub-issues; dispatch only on approval.
