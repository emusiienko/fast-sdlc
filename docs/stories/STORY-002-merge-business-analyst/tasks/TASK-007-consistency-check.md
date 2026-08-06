---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-007: Consistency check

## What to implement
Grep the whole plugin (`fast-sdlc/fast-sdlc/`) for any remaining `product-strategist`/`spec-writer` reference and resolve each.

## Scope
`fast-sdlc/fast-sdlc/`

## Dependencies
TASK-002, TASK-003, TASK-004, TASK-005, TASK-006

## Acceptance Criteria
- [x] Zero matches for `product-strategist` or `spec-writer` anywhere under `fast-sdlc/fast-sdlc/`

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Ran `grep -rn "product-strategist|spec-writer" fast-sdlc/fast-sdlc/` after TASK-002–006. Found two leftovers TASK-002–006 didn't cover: `fast-sdlc-architect.md`'s "Ownership notes" (referenced spec-writer for `Spec ✓`/archival ownership) and `templates/fast-sdlc-project-setup.md`'s Team section (still had separate `product-strategist`/`spec-writer` entries with their own folders). Fixed both — renamed the architect reference, merged the two Team entries into one `business-analyst` entry. Re-ran the grep: zero matches.
**Deviations from plan**: Scope was one file wider than the task's own dependency list implied (`fast-sdlc-architect.md` and the project-setup template weren't touched by any of TASK-002–006).
**Technical decisions made**: None.
**Code locations**: `fast-sdlc/fast-sdlc/agents/fast-sdlc-architect.md`, `fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md`
**Technical debt noted**: `fast-sdlc-architect.md`'s ownership notes still hardcode `docs/stories/index.md` and the `Arch ✓` column literally — belongs to STORY-001 TASK-008 (Update fast-sdlc-architect agent), not this story.
