---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-009: Update fast-sdlc-backend-developer agent

## What to implement
Update the agent to resolve task-file and conventions-doc locations from `fast-sdlc-project-setup.md` instead of assuming fixed `docs/` paths.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-backend-developer.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] No remaining hardcoded path this agent should instead resolve from config

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Superseded/absorbed by STORY-006 — `fast-sdlc-backend-developer.md` was retired entirely (TASK-006) and replaced by the fully dynamic-path-aware `fast-sdlc-coder.md` (TASK-002). Same resolution pattern as STORY-005 TASK-008.

**Deviations from plan**: None to report here — the actual work happened under STORY-006.

**Technical decisions made**: None new — see STORY-006 TASK-001/002's own Implementation Reports.

**Code locations**: `fast-sdlc/agents/fast-sdlc-backend-developer.md` (removed), `fast-sdlc/agents/fast-sdlc-coder.md` (its replacement)

**Technical debt noted**: None new.
