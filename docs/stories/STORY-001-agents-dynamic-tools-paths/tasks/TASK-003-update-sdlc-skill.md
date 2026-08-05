---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-003: Update sdlc skill to support the config

## What to implement
Replace hardcoded location assumptions across `sdlc/SKILL.md` (Document Ownership, Key File Formats, workflows) with references resolved from `fast-sdlc-project-setup.md`. Retire `team.md` in favor of the new config file and document the new file's ownership.

## Scope
`fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`

## Dependencies
TASK-001, TASK-002

## Acceptance Criteria
- [x] No remaining hardcoded `docs/...` path assumption that should instead be config-resolved
- [x] `team.md` removed from Document Ownership; `fast-sdlc-project-setup.md` added with an owner
- [x] Mapping between roles and agents (added in a prior story) still intact

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-05
**What was done**: Rewrote `sdlc/SKILL.md` end to end: added Glossary, Project Config, Artifacts, and Task Management sections; converted Pipeline/Common Workflows to reference artifact names instead of hardcoded `docs/...` paths; renamed Document Ownership → Artifacts Ownership, scoped to the 10 canonical artifacts only; renamed Key File Formats → Artifact Content Rules, abstracted to content requirements (no file/frontmatter detail — that moved to the new `file-based-task-management` skill, TASK-015); removed the now-obsolete ADR Rules and `docs/team.md` sections; added Bug Statuses and an Implement Story workflow (with deviation checks against Requirements). `fast-sdlc-project-setup.md` given an explicit owner (orchestrator) and the `### Fast SDLC` CLAUDE.md section a concrete example (closing TASK-002 alongside this).
**Deviations from plan**: Scope grew significantly beyond the original AC through discussion — new Artifacts (Milestones, Implementation Plan) and a new sibling skill (`file-based-task-management`, TASK-015) were introduced; Stories/Tasks/Bugs ended up as Artifacts too, not a separate category.
**Technical decisions made**: Task Management is a pluggable skill reference (name + operating location) rather than a fixed file-based/external-tool choice, decoupling `sdlc` entirely from any one Task Management implementation.
**Code locations**: `fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`
**Technical debt noted**: `session` skill (TASK-005) not yet audited for the same hardcoded-path issue.
