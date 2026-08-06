---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-001: Design business-analyst role definition

## What to implement
Define the merged role: what Discovery Mode and Spec Mode each do, what carries over unchanged from `product-strategist`/`spec-writer`, what's explicitly dropped (e.g. any technical/API-level concern), how the two modes sequence (Discovery optional/skippable, same as today), and the role's one-line pipeline description for `sdlc`.

## Scope
Design decisions only — no file writes beyond notes needed to brief the later tasks.

## Dependencies
None

## Acceptance Criteria
- [x] Discovery Mode and Spec Mode responsibilities clearly separated
- [x] Explicit list of what's dropped/changed relative to the two source roles
- [x] Face-to-face default decided

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Designed the merged role through discussion: Discovery and Spec became sequential phases within unified New Project / New Scope / Reconcile modes (not two standalone modes), rather than kept as separate hand-offs. API Contract responsibility dropped entirely (moved to architect) — Requirements keeps only behavior-level content (data model, validation rules, access framed as business terms, error scenarios as user-facing alternatives to the happy path, optional business-driven usage limits). Further discussion also produced a broader redesign: Requirements redefined as future-only (never describes what's already built), with a new optional Documentation artifact as its counterpart (co-owned with architect) for as-built state.
**Deviations from plan**: Scope grew well beyond the original merge — introduced Documentation as a new sdlc Artifact, a `${CLAUDE_PLUGIN_ROOT}`-based mechanism for shipping default format templates outside the agent file, a "minor change" bypass path in Mode 2 (New Scope) for additions that don't affect the Validated Idea, and two new parked stories (STORY-003 UI/UX Designer, STORY-004 Documentation default templates).
**Technical decisions made**: Face-to-face default — business-analyst defaults to face-to-face, inherited from both source roles (`orchestration`'s Project Initiation already defaults `product-strategist` and `spec-writer` to yes); to be wired into `orchestration` in TASK-005.
**Code locations**: Discussion only, materialized in TASK-002.
**Technical debt noted**: Milestones artifact still has no concrete format/workflow — mentioned in discussion (a multi-story Requirements proposal implies one) but not formally specified anywhere.
