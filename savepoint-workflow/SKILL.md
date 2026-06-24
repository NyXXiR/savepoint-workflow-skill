---
name: savepoint-workflow
description: TDD-first checklist execution with repository-backed savepoints for continuing implementation work across AI agents, accounts, sessions, and providers such as Codex, Claude, Gemini, opencode, Hermes, and other coding agents. Use when starting non-trivial implementation work, bug fixes, refactors, long-running or multi-phase code changes, tasks likely to exceed context or token budget, provider/model/account switching, interrupted sessions, checkpointed progress, resumable implementation, task savepoints, implementation checklists, handoff notes, or another AI/provider continuing the work.
---

# Savepoint Workflow

## Purpose

Keep implementation work resumable outside the chat session by creating a small Markdown savepoint in the target repository. The repository becomes the durable memory: a savepoint records the objective, acceptance criteria, TDD state, checklist, repository constraints, decisions, files, verification results, blockers, and resume prompt so another agent, account, provider, model, or session can continue without reconstructing the whole task from prior conversation.

The operating model is TDD-first checklist execution:

- Characterize or reproduce current behavior before changing it.
- Add or identify the smallest useful Red check when feasible.
- Implement the smallest Green change.
- Refactor only after behavior is protected or explicitly characterized.
- Keep the checklist current enough that a fresh agent can resume from the first unchecked item.

## Start Protocol

Before making implementation edits:

1. Inspect the repository enough to understand scope, existing conventions, and likely verification commands.
2. Look for repository instructions and source-of-truth rules before planning. Common examples: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `opencode.json`, `.cursorrules`, `.cursor/rules/*`, `docs/domain-rules.md`.
3. Create or update a savepoint file under `.ai-savepoints/`.
4. Name new files as `.ai-savepoints/YYYYMMDD-HHMM-task-slug.md` using local time when available.
5. If an existing savepoint clearly belongs to the current task, resume from it instead of creating a duplicate.
6. Use `assets/savepoint-template.md` as the structure unless the repository already has a stronger local convention.

The first savepoint must include:

- Objective and acceptance criteria.
- TDD state: Red, Green, Refactor, and the reason if test-first is not feasible.
- Current repository facts that matter.
- Repository instructions read, source-of-truth files, and non-negotiable constraints.
- Phased checklist with small, verifiable tasks.
- Known constraints, assumptions, and open questions.
- Exact files or areas expected to change.
- Planned verification commands, even if tentative.

## TDD Protocol

For code changes, default to a Red/Green/Refactor loop:

1. Red: reproduce the bug, characterize existing behavior, or add a focused failing test before implementation.
2. Green: make the smallest coherent change that satisfies the focused check.
3. Refactor: improve structure only after the behavior is covered or the user explicitly accepts characterization-only verification.

If a failing test cannot be written first, record the reason in the savepoint and choose the nearest meaningful verification, such as a smoke command, type check, lint check, scripted reproduction, or manual QA path.

For documentation-only or metadata-only changes, use content/structure verification as the TDD substitute and record that substitution.

## Checklist Rules

Keep the checklist honest and useful for a fresh agent:

- Use `[ ]` for pending work and `[x]` only after the task is actually complete.
- Split vague items until another agent can continue without reconstructing the whole plan.
- Prefer phase headings with task checkboxes beneath them.
- Add new checklist items when implementation reveals hidden work.
- Update checklist items immediately after each durable task lands, not only at the end.
- Do not delete failed paths or abandoned attempts; move them to notes with the reason.
- If blocked, record the blocker, evidence, and the smallest next question or external action needed.

## Update Cadence

Update the savepoint at each durable transition:

- After initial repository survey.
- Before the first implementation edit.
- After each completed task or phase.
- After changing direction or making a meaningful design decision.
- After running verification commands, including failures.
- Before stopping for any reason.
- Immediately when context, token budget, time, or tool reliability looks risky.

When the budget is low, stop implementing and spend the remaining time making the savepoint accurate. A useful savepoint is more valuable than one extra partial edit.

## Resume Protocol

When continuing existing work:

1. Find the latest relevant `.ai-savepoints/*.md` file.
2. Read it before inspecting or editing code.
3. Verify the recorded state against the working tree and tests; treat stale notes as clues, not truth.
4. Continue from the first unchecked task that still matches reality.
5. Append the new agent/session to the activity log.
6. Preserve previous decisions unless code evidence or user instructions justify changing them.

## Provider-Neutral Notes

Write the savepoint for any capable coding agent, not for a specific vendor or UI:

- Use plain Markdown, repository-relative paths, exact commands, and concise results.
- Avoid references to private context that is not in the repository or savepoint file.
- Record tool limitations as observations, not assumptions about future agents.
- Include enough command output summary for another provider to know what passed or failed.
- Keep secrets, tokens, private credentials, and unrelated user data out of the savepoint.

## Final Protocol

Before final response:

1. Mark completed checklist items.
2. Record verification commands and results.
3. Add remaining unchecked work, if any, with concrete next steps.
4. Update the `Resume Prompt` section so another agent can continue from the current state.
5. Mention the savepoint file path and current status to the user.
