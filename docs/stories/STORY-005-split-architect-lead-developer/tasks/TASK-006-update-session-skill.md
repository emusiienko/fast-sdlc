---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-006: Update session skill

## What to implement
Check for and fix any reference to architect's planning/task-creation responsibilities; add lead-developer to the face-to-face Callee list if applicable.

## Scope
`fast-sdlc/fast-sdlc/skills/session/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining assumption that architect creates the Implementation Plan or Tasks

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-12
**What was done**: Checked `fast-sdlc/skills/session/SKILL.md` — it's role-agnostic (caller/callee mechanics only) and had no assumption anywhere about architect creating Implementation Plans or Tasks. It also has no dedicated face-to-face "Callee list" of its own — that list lives in `sdlc` skill (already updated with lead-developer in TASK-004). Only change: added `lead-developer` to the example agent names on the `<agent-name>` line (was `business-analyst`, `architect`).

**Deviations from plan**: None — task turned out smaller than anticipated since this file had nothing substantive to fix.

**Technical decisions made**: None.

**Code locations**: `fast-sdlc/skills/session/SKILL.md`

**Technical debt noted**: None new.
