---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-002: Define CLAUDE.md Fast SDLC section

## What to implement
Define the exact format/wording of the `### Fast SDLC` section added to a target project's `CLAUDE.md`, pointing to the actual location of `fast-sdlc-project-setup.md`. Define the fallback behavior when the section is absent (default: `fast-sdlc-project-setup.md` at the repo root).

## Scope
Documentation/template guidance consumed by `sdlc`/`orchestration` (exact home TBD — likely a snippet under `fast-sdlc/fast-sdlc/templates/`).

## Dependencies
TASK-001

## Acceptance Criteria
- [x] Section format defined (heading, content shape, how the path is expressed)
- [x] Fallback-to-default behavior documented for when the section is missing

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-05
**What was done**: Defined the `### Fast SDLC` section format in `sdlc/SKILL.md` > Project Config, with a concrete example (`Config: <path>`) and the fallback-to-default-root-path rule when the section is absent. Landed as part of the same Project Config work as TASK-003, not a separate template file.
**Deviations from plan**: Scope guess in the task ("likely a snippet under fast-sdlc/fast-sdlc/templates/") was wrong — it lives directly in `sdlc/SKILL.md` > Project Config instead, since that's the one place both orchestrator and every agent already read.
**Technical decisions made**: None beyond the format itself.
**Code locations**: `fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`
**Technical debt noted**: None.
