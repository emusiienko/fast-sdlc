---
status: ToDo
assignee: orchestrator
story: STORY-001
---

# TASK-008: Update fast-sdlc-architect agent

## What to implement
Update the agent to resolve ADR location, architecture docs, and task-file locations from `fast-sdlc-project-setup.md` instead of assuming fixed `docs/` paths. This is the primary agent affected by the ADR-location decision.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-architect.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [ ] ADR location resolved from config, not hardcoded to `docs/tech_decisions/`
- [ ] No other remaining hardcoded path this agent should instead resolve from config

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
