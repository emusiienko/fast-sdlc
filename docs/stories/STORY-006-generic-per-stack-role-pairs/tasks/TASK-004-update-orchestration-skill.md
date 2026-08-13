---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-004: Update orchestration skill

## What to implement
Replace references to spawning `backend-developer`/`frontend-developer` with `coder` (per-stack, same pattern as spawning `lead-developer`).

## Scope
`fast-sdlc/fast-sdlc/skills/orchestration/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining assumption that backend-developer/frontend-developer are spawned as separate roles

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Checked `fast-sdlc/skills/orchestration/SKILL.md` — grepped for `backend-developer`/`frontend-developer` and for "implementor" generally. No hits for either old role name; the skill already refers to implementors generically ("spawn the appropriate implementor in Quick Fix mode") without naming specific roles. No changes needed.

**Deviations from plan**: Scope turned out smaller than expected — same outcome as STORY-005 TASK-006 (session skill), which also needed no real changes.

**Technical decisions made**: None.

**Code locations**: None changed.

**Technical debt noted**: None new.
