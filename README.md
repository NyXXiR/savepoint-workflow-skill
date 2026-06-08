# Savepoint Workflow Skill

A small, repository-backed AI agent skill for keeping implementation work resumable across context limits, token exhaustion, account switches, model/provider switches, and interrupted sessions.

Inspired by game savepoints, the skill tells an agent to create and maintain a Markdown savepoint under `.ai-savepoints/` before and during non-trivial implementation work. The repository becomes the durable memory, so another agent such as Codex, Claude, Gemini, opencode, Hermes, or another provider can resume from the savepoint instead of relying on the previous chat session.

Each savepoint contains the objective, checklist, repository constraints, decisions, changed files, verification log, blockers, and a resume prompt for the next agent.

## Install

Copy the `savepoint-workflow/` folder into your agent app's skills directory.

For Codex-style local skills:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R savepoint-workflow "${CODEX_HOME:-$HOME/.codex}/skills/"
```

For other skill-enabled agent apps, add the same folder using that app's skill/plugin import flow. The skill has no runtime dependency and uses only plain Markdown.

## Invoke

Example prompt:

```text
Use $savepoint-workflow while implementing this change.
```

The skill is designed for agent apps such as Codex, Claude, Gemini, opencode, Hermes, and other multi-provider coding agents.
