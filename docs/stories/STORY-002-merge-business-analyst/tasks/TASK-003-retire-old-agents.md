---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-003: Retire fast-sdlc-product-strategist and fast-sdlc-spec-writer agents

## What to implement
Remove the two now-superseded agent files.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-product-strategist.md`, `fast-sdlc/fast-sdlc/agents/fast-sdlc-spec-writer.md`

## Dependencies
TASK-002

## Acceptance Criteria
- [x] Both files removed
- [ ] Nothing else in the plugin still references them by name (cross-checked in TASK-007)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Deleted both agent files. Verified via grep across `fast-sdlc/fast-sdlc/` that only `skills/sdlc/SKILL.md` still references the old role names — known, deferred to TASK-004.
**Deviations from plan**: None.
**Technical decisions made**: None.
**Code locations**: `fast-sdlc/fast-sdlc/agents/fast-sdlc-product-strategist.md` (deleted), `fast-sdlc/fast-sdlc/agents/fast-sdlc-spec-writer.md` (deleted)
**Technical debt noted**: `sdlc/SKILL.md` still references `product-strategist`/`spec-writer` by name until TASK-004 runs.
