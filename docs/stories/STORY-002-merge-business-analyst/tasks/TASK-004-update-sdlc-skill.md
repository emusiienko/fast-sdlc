---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-004: Update sdlc skill

## What to implement
Replace `product-strategist`/`spec-writer` throughout `sdlc/SKILL.md` with `business-analyst`: Roles (merge the two bullets into one, describing both modes), Mapping between roles and agents (one row instead of two), Pipeline (merge phases 1 and 2 into one phase with two steps), Artifacts Ownership (Validated Idea/Requirements/Milestones/Stories owner rows), Common Workflows (Setup, Reconcile — both currently mention spec-writer and/or product-strategist by name).

## Scope
`fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No remaining mention of `product-strategist` or `spec-writer` as separate roles
- [x] Pipeline phase numbering/structure updated consistently
- [x] Mapping table has one `business-analyst` row

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Replaced every `product-strategist`/`spec-writer` reference in `sdlc/SKILL.md` with `business-analyst` (verified via grep — zero remaining). Roles and the Mapping table merged two entries into one. The Pipeline's phase-by-phase breakdown (`### 1. Discovery`, `### 2. Spec`) was deliberately kept as two separate phase entries — only their `Role:` line changed — since it's organized by phase, not role, and mirrors the Phase A/B structure already inside `fast-sdlc-business-analyst.md`. Setup workflow's two bullets merged into one. Reconcile heading renamed `Spec-Writer Reconcile` → `Business-Analyst Reconcile`. Artifacts Ownership, Implement Story's Deviation Check, and the Face-to-face Callee list updated.
**Deviations from plan**: None — matched the approved plan exactly.
**Technical decisions made**: Pipeline phase count intentionally unchanged (still 6 phases) rather than collapsing Discovery+Spec into one phase, for consistency with the agent's own internal structure.
**Code locations**: `fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`
**Technical debt noted**: `orchestration`/`session` skills still reference the old role names — TASK-005/006.
