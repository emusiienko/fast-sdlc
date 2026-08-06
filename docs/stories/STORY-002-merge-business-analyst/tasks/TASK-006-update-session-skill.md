---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-006: Update session skill

## What to implement
Replace `product-strategist`/`spec-writer` in `session/SKILL.md`'s Callee list with `business-analyst`.

## Scope
`fast-sdlc/fast-sdlc/skills/session/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining mention of `product-strategist` or `spec-writer` as separate roles

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Found only one mention (an illustrative example, not an actual Callee list — that one lives in `orchestration/SKILL.md` and was already fixed in TASK-005), renamed `spec-writer` → `business-analyst`. Verified via grep — zero remaining.
**Deviations from plan**: The task's original scope description ("Callee list") was slightly off — no such list exists in this file; the fix location was a naming example instead.
**Technical decisions made**: None.
**Code locations**: `fast-sdlc/fast-sdlc/skills/session/SKILL.md`
**Technical debt noted**: None.
