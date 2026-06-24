# Savepoint Workflow Skill

TDD-first checklist execution for AI coding agents, backed by repository savepoints.

This skill tells an agent to turn non-trivial implementation work into a small, durable Markdown savepoint under `.ai-savepoints/`. The savepoint records the objective, acceptance criteria, Red/Green/Refactor state, checklist progress, decisions, touched files, verification results, blockers, and a resume prompt.

The goal is simple: if a session hits context limits, token exhaustion, account switching, provider switching, or interruption, the next capable agent can continue from the repository instead of reverse-engineering the previous chat.

## What You Get

- A TDD-first implementation checklist before edits begin.
- Explicit Red/Green/Refactor notes for code changes.
- A durable `.ai-savepoints/YYYYMMDD-HHMM-task-slug.md` handoff file.
- Verification logs that separate passed checks, failed checks, and skipped checks.
- A provider-neutral resume prompt for Codex, Claude, Gemini, opencode, Hermes, or another coding agent.

## When To Use It

Use this skill for:

- feature work that may span multiple sessions
- bug fixes where a failing test or reproduction matters
- refactors that need a safe checklist
- long-running coding tasks near context or token limits
- handoffs between agents, accounts, models, or providers

It is intentionally small. There is no runtime dependency, server, database, or package manager. The skill uses plain Markdown so the repository itself becomes the continuity layer.

## Install

Copy the `savepoint-workflow/` folder into a Codex skill directory.

For vanilla Codex user-level skills:

```bash
git clone https://github.com/NyXXiR/savepoint-workflow-skill.git
mkdir -p "$HOME/.agents/skills"
cp -R savepoint-workflow-skill/savepoint-workflow "$HOME/.agents/skills/"
```

For a single repository:

```bash
mkdir -p .agents/skills
cp -R /path/to/savepoint-workflow-skill/savepoint-workflow .agents/skills/
```

Restart Codex if the skill does not appear immediately.

For shared local skill trees, copy or link the same folder into the location your agent reads. Codex supports symlinked skill folders.

## Invoke

Example prompt:

```text
Use $savepoint-workflow while implementing this change.
```

The skill will ask the agent to:

1. Inspect repository instructions and current behavior.
2. Create or resume a savepoint under `.ai-savepoints/`.
3. Write acceptance criteria and a phased checklist.
4. Run the nearest useful Red step before implementation when feasible.
5. Implement in small Green steps and update the checklist as work lands.
6. Refactor only after behavior is protected or explicitly characterized.
7. Record verification results and a resume prompt before stopping.

## Example Savepoint

See [`examples/sample-savepoint.md`](examples/sample-savepoint.md) for a complete example of the artifact this skill asks agents to maintain.

Short excerpt:

```md
## TDD State

- Red: `npm test -- auth-refresh.test.ts` reproduced the expired-token failure.
- Green: refreshed access-token path now passes the focused test.
- Refactor: pending; keep changes scoped until the integration test passes.

## Checklist

- [x] Reproduce expired-token failure.
- [x] Add focused regression test.
- [ ] Run full auth suite.
```

## Repository Layout

```text
savepoint-workflow/
  SKILL.md
  agents/openai.yaml
  assets/savepoint-template.md
examples/
  sample-savepoint.md
```

## Design Rules

- Prefer TDD for code changes; document why when a failing test is not practical.
- Never hide failed attempts. Record abandoned paths with the reason.
- Keep secrets, tokens, and private chat-only context out of savepoints.
- Use repository-relative paths and exact commands.
- Make checkboxes honest: `[x]` only after the item is actually done.

## License

MIT
