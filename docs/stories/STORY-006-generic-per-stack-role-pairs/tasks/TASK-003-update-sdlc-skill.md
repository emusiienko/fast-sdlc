---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-003: Update sdlc skill

## What to implement
Replace `backend-developer`/`frontend-developer` with `coder` in Roles, the Mapping table, and the Pipeline. Update Task Lifecycle, Artifacts Ownership, and Common Workflow steps (Implement Task, Quick Fix, Bug Fix, Reconcile) that name backend-developer/frontend-developer specifically.

## Scope
`fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] `coder` present in Roles, Mapping table, Pipeline
- [x] No remaining mention of `backend-developer`/`frontend-developer` as active roles (verified via grep, no matches)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Updated `fast-sdlc/skills/sdlc/SKILL.md` — merged `backend-developer` and `frontend-developer` into a single `coder` role everywhere they appeared: Roles (one combined entry, noting coder now writes its own unit/integration tests, per TASK-002's design), Mapping table, Pipeline Phase 5 (Implementation), Common Workflows > Setup, Implement Task / Quick Fix / Bug Fix "Applies to" lines, and the face-to-face Callee list. `devops` untouched throughout — stays a separate, fixed role in every one of these lists.

**Deviations from plan**: None — scope matched the task description closely.

**Technical decisions made**: None beyond what TASK-001/002 already decided; this task only propagated it into the shared skill.

**Code locations**: `fast-sdlc/skills/sdlc/SKILL.md`

**Technical debt noted**: None new.
