# Agent Savepoint: Auth Token Refresh Regression

Status: in_progress
Started: 2026-06-24 14:20 local
Last updated: 2026-06-24 15:05 local
Agent/provider: Codex
Continuation target: any capable coding agent

## Objective

Fix the expired-token refresh path so authenticated API calls recover once and then retry the original request.

## Acceptance Criteria

- [x] Expired-token behavior is reproduced or characterized.
- [x] Red/Green/Refactor or documented verification substitute is recorded.
- [ ] Auth regression tests pass.
- [ ] Final state and remaining work are documented here.

## TDD State

- Red: `npm test -- auth-refresh.test.ts` reproduced the failed retry after a 401 response.
- Green: refresh path now retries the original request in the focused test.
- Refactor: pending until the full auth suite passes.
- If test-first is not feasible, explain why and name the substitute verification: not applicable.

## Current Repository Facts

- Project/root: `apps/api`
- Relevant stack/frameworks: Node 20, Express, Vitest
- Existing conventions to follow: API clients return typed errors instead of throwing raw response objects.
- Repository instructions read: `AGENTS.md`
- Source-of-truth files: `src/auth/client.ts`, `test/auth-refresh.test.ts`
- Non-negotiable constraints: do not store refreshed tokens in localStorage.
- Forbidden approaches: do not skip the retry test or mask 401 responses globally.
- Important files/directories: `src/auth/`, `test/`
- Dirty worktree notes: only auth client and focused test changed.

## Plan

### Phase 1: Understand

- [x] Inspect relevant files and architecture.
- [x] Identify constraints and verification commands.
- [x] Reproduce or characterize current behavior.

### Phase 2: Red

- [x] Add or identify the smallest useful failing/targeted check.
- [x] Record why test-first is not feasible if no Red check can be created.

### Phase 3: Green

- [x] Make the smallest coherent implementation change.
- [x] Update checklist items as each durable task lands.

### Phase 4: Refactor And Verify

- [ ] Refactor only after behavior is covered or characterized.
- [ ] Run targeted checks.
- [ ] Fix regressions or record blockers.

### Phase 5: Savepoint

- [x] Update this file with final status.
- [x] Write a concise resume prompt for the next agent.

## Decisions And Assumptions

- Decision: Retry only the failed request once after a successful refresh to avoid loops.
- Assumption: Refresh endpoint keeps the existing response shape.

## Session Transfer Notes

- Context that existed only in chat and must be preserved: user reported this happens after leaving the app idle overnight.
- User preferences or latest instructions: keep the patch minimal.
- Provider/tool limitations observed: none.

## Files Touched

- `src/auth/client.ts`: add one-shot retry after refresh.
- `test/auth-refresh.test.ts`: focused regression for 401 refresh retry.

## Verification Log

| Command | Result | Notes |
| --- | --- | --- |
| `npm test -- auth-refresh.test.ts` | pass | focused regression passes |
| `npm test -- auth` | not run | next step |

## Failed Or Abandoned Paths

- Global response interceptor retry was rejected because it masked unrelated 401 errors.

## Activity Log

- 2026-06-24 14:20 - Created savepoint.
- 2026-06-24 14:35 - Added failing auth refresh test.
- 2026-06-24 15:05 - Focused test is green; full auth suite still pending.

## Blockers

- None currently.

## Resume Prompt

Continue this implementation by reading `.ai-savepoints/20260624-1420-auth-token-refresh-regression.md`, checking the current working tree, and running `npm test -- auth`. If it passes, refactor duplicated retry setup in `test/auth-refresh.test.ts`; if it fails, keep changes scoped to `src/auth/client.ts` and update this savepoint before stopping.
