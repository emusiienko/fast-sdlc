---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-006: Retire fast-sdlc-backend-developer and fast-sdlc-frontend-developer

## What to implement
Once `fast-sdlc-coder.md` fully supersedes them, remove `fast-sdlc-backend-developer.md` and `fast-sdlc-frontend-developer.md`. Git preserves history — offer cleanup to the user rather than deciding unilaterally, same pattern as business-analyst's Reconcile "OFFER CLEANUP" step.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-backend-developer.md`, `fast-sdlc/fast-sdlc/agents/fast-sdlc-frontend-developer.md`

## Dependencies
TASK-002, TASK-003, TASK-004, TASK-005

## Acceptance Criteria
- [x] User confirmed deletion
- [x] Files removed if confirmed

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Offered cleanup, user confirmed, removed `fast-sdlc-backend-developer.md` and `fast-sdlc-frontend-developer.md` via `git rm`. History is preserved in git.

**Deviations from plan**: None.

**Technical decisions made**: None new.

**Code locations**: `fast-sdlc/agents/fast-sdlc-backend-developer.md` (removed), `fast-sdlc/agents/fast-sdlc-frontend-developer.md` (removed)

**Technical debt noted**: None new.
