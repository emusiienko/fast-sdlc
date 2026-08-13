---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-005: Update fast-sdlc-project-setup.md template

## What to implement
Replace the `backend-developer`/`frontend-developer` Team rows with a `coder` row (per stack, same pattern as `lead-developer`'s row added in STORY-005).

## Scope
`fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] `coder` Team row present
- [x] `backend-developer`/`frontend-developer` Team rows removed

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Replaced the `backend-developer`/`frontend-developer` Team rows in `fast-sdlc-project-setup.md` with a single `coder` row (one folder, same pattern as `lead-developer`'s own row — per-stack scoping happens at the session/Artifacts level, not in the Team list itself).

**Deviations from plan**: None.

**Technical decisions made**: None new.

**Code locations**: `fast-sdlc/templates/fast-sdlc-project-setup.md`

**Technical debt noted**: None new.
