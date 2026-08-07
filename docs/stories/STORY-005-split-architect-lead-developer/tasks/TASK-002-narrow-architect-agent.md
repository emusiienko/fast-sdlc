---
status: ToDo
assignee: orchestrator
story: STORY-005
---

# TASK-002: Narrow fast-sdlc-architect agent

## What to implement
Remove from `fast-sdlc-architect.md`: team/role composition setup (Mode 1 step 2 — superseded by orchestrator's Project Initiation per STORY-001), Feature Design mode's implementation planning and task creation (Mode 2 entirely — moves to lead-developer). Keep: architecture, API Contract, ADRs, CLAUDE.md, Project Setup (minus team setup), Reconcile (per TASK-001's split decision). Rewrite against the dynamic config model (`fast-sdlc-project-setup.md`), same as `fast-sdlc-business-analyst.md` — this agent doesn't need a separate "make it dynamic" pass afterward.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-architect.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [ ] No team/role setup responsibility remains
- [ ] No implementation planning or task creation remains
- [ ] No hardcoded `docs/...` paths — resolves from `fast-sdlc-project-setup.md`

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
