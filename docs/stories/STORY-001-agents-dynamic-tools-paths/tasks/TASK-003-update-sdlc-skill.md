---
status: ToDo
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
- [ ] No remaining hardcoded `docs/...` path assumption that should instead be config-resolved
- [ ] `team.md` removed from Document Ownership; `fast-sdlc-project-setup.md` added with an owner
- [ ] Mapping between roles and agents (added in a prior story) still intact

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**:
**What was done**:
**Deviations from plan**:
**Technical decisions made**:
**Code locations**:
**Technical debt noted**:
