---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-015: Create file-based-task-management skill

## What to implement
A new skill, `file-based-task-management`, implementing Task Management as files/folders — one concrete option among possibly several (e.g. a future MCP-tool-backed skill) that a project's `fast-sdlc-project-setup.md` > Task Management can select. Holds what was removed from `sdlc/SKILL.md` during its abstraction: the Stories index table, Tasks index table, story description format, task file format, implementation plan format, and bug index/file format — as this skill's own conventions, not `sdlc`'s.

`fast-sdlc-project-setup.md` > Task Management, when this skill is selected, must also state the folder it operates in (e.g. `docs/stories/`).

## Scope
`fast-sdlc/fast-sdlc/skills/file-based-task-management/`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] Skill created with the file/folder formats previously in `sdlc/SKILL.md`'s "Key File Formats" (Stories index, Tasks index, story/task/bug file formats, implementation plan)
- [x] `sdlc/SKILL.md` content stays abstract (statuses, flags, content rules only) — no file-format detail duplicated between the two skills
- [x] `fast-sdlc-project-setup.md` template's Task Management section updated to name the skill + its operating folder

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-05
**What was done**: Created `fast-sdlc/fast-sdlc/skills/file-based-task-management/SKILL.md` holding the Layout, Stories/Tasks/Bugs index formats, story/task/bug file formats, and implementation plan format previously in `sdlc/SKILL.md`. Story Status Flags now render as table columns per active QA/Reconcile role, cross-referenced by name to `sdlc`. Updated `fast-sdlc-project-setup.md`'s Task Management section to a `**skill**` + `**location**` pair. Updated `sdlc/SKILL.md` > Project Config's Task Management bullet to say it names a skill + operating location.
**Deviations from plan**: None.
**Technical decisions made**: Test Cases format intentionally excluded — stays out of shared scope, to be defined in manual-qa's own agent (TASK-013), not duplicated here.
**Code locations**: `fast-sdlc/fast-sdlc/skills/file-based-task-management/SKILL.md`, `fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md`, `fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`
**Technical debt noted**: None.
