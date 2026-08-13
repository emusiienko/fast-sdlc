---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-007: Consistency check

## What to implement
Grep the whole plugin (`fast-sdlc/fast-sdlc/`) for any remaining reference to `backend-developer`/`frontend-developer` as active, separate roles, and resolve each. Also close STORY-001 TASK-009 and TASK-011 as superseded by this story, same as STORY-005 closed STORY-001 TASK-008.

## Scope
`fast-sdlc/fast-sdlc/`

## Dependencies
TASK-002, TASK-003, TASK-004, TASK-005, TASK-006

## Acceptance Criteria
- [x] No remaining mention of backend-developer/frontend-developer as active roles anywhere under `fast-sdlc/fast-sdlc/`
- [x] STORY-001 TASK-009 and TASK-011 marked Done (superseded)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Grepped `fast-sdlc/fast-sdlc/` for `backend-developer`/`frontend-developer` and found 3 leftover files with stale mentions (all illustrative examples or cross-references, not structural assumptions): `skills/file-based-task-management/SKILL.md` (5 example table/frontmatter rows using `backend-developer`/`frontend-developer` as example assignee values — updated to `coder`), `agents/fast-sdlc-manual-qa.md` (one example assignment rule string — updated to `coder`), `agents/fast-sdlc-devops.md` (one cross-reference "follows the same task lifecycle as backend-developer" — updated to `coder`). Re-ran the grep after fixing — zero matches. Also marked STORY-001 TASK-009 and TASK-011 `Done` (superseded, same pattern as STORY-005's closure of STORY-001 TASK-008).

**Deviations from plan**: None — found exactly the kind of residue expected, nothing structural.

**Technical decisions made**: None new.

**Code locations**: `fast-sdlc/skills/file-based-task-management/SKILL.md`, `fast-sdlc/agents/fast-sdlc-manual-qa.md`, `fast-sdlc/agents/fast-sdlc-devops.md`

**Technical debt noted**: Same open items as noted in TASK-002: `qa-automation`'s role/responsibilities need reconsidering now that `coder` owns unit/integration testing directly (not addressed in STORY-006); `templates/lead-developer/conventions/` remains unpopulated.
