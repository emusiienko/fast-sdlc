---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-005: Update orchestration skill

## What to implement
Replace every reference to `product-strategist`/`spec-writer` in `orchestration/SKILL.md` with `business-analyst` (Project Initiation face-to-face defaults, Create Story delegation logic, Add Scope phases, Dashboard/checkpoint mentions, etc.).

## Scope
`fast-sdlc/fast-sdlc/skills/orchestration/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining mention of `product-strategist` or `spec-writer` as separate roles
- [x] Face-to-face default for `business-analyst` matches TASK-001's decision

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Replaced every `product-strategist`/`spec-writer` mention with `business-analyst` (verified via grep — zero remaining). Project Initiation's face-to-face default merged to one line. Create Story's two delegation bullets ("Vague idea" / "Clear feature") merged into one, since both now route to the same agent. Add Scope's Phase 1+2 (previously separate product-strategist/spec-writer face-to-face sessions) merged into one business-analyst phase — consistent with the agent no longer treating Discovery/Spec as separate hand-offs. Execute Story's Reconcile step and the example state-file both renamed.
**Deviations from plan**: None.
**Technical decisions made**: Add Scope collapsed from 3 phases to 2 (business-analyst, then architect) rather than keeping a vestigial middle phase.
**Code locations**: `fast-sdlc/fast-sdlc/skills/orchestration/SKILL.md`
**Technical debt noted**: Line 116 still references `docs/team.md` (superseded by `fast-sdlc-project-setup.md` per STORY-001) — out of this task's scope, belongs to STORY-001 TASK-004 (Update orchestration skill to support the config).
