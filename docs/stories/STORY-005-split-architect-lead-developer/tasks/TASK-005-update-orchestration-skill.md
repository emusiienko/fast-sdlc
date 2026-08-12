---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-005: Update orchestration skill

## What to implement
Replace references to architect doing implementation planning/task creation with lead-developer (Execute Story's task loop, Add Scope's architect Feature Design phase, any Reconcile step that assumed architect alone).

## Scope
`fast-sdlc/fast-sdlc/skills/orchestration/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining assumption that architect creates the Implementation Plan or Tasks

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-12
**What was done**: Updated `fast-sdlc/skills/orchestration/SKILL.md`:

- **Scenario 3 (Add Scope)**: Phase 2 "architect Feature Design" split into Phase 2 (architect Plan Next Scope — produces/updates the Technical Roadmap) and Phase 3 (lead-developer plans implementation per repo/repo+stack named in the Roadmap, sequential or batch per user preference)
- **Scenario 4 (Execute Story) Reconcile sequence**: inserted "Spawn lead-developer for each repo (or repo+stack) the story touched" before "Spawn architect → wait for Arch ✓", matching `sdlc` skill's TASK-004 update (lead-developer reconciles first, then architect, then business-analyst)
- **State file example**: `next_step: architect Feature Design for STORY-007` → `next_step: lead-developer plan for STORY-007`

Left untouched, deliberately out of scope: this file still uses `docs/team.md` and other hardcoded `docs/...` paths throughout (STORY-001 TASK-004's job, not STORY-005's) — did not conflate the two migrations.

**Deviations from plan**: None — scope matched the task description closely this time.

**Technical decisions made**: None beyond what TASK-001/002/003/004 already decided; this task only propagated it.

**Code locations**: `fast-sdlc/skills/orchestration/SKILL.md`

**Technical debt noted**: Same Escalation/Feedback TBD dependency as TASK-004 (STORY-007) — the Reconcile sequence here assumes lead-developer's own reconcile step completes before architect's, but nothing enforces that ordering mechanically yet.
