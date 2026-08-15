---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-013: Update fast-sdlc-manual-qa agent

## What to implement
Update the agent to resolve test-case and strategy-doc locations from `fast-sdlc-project-setup.md` instead of assuming fixed `docs/` paths.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-manual-qa.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] No remaining hardcoded path this agent should instead resolve from config (verified via grep, no matches)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-15
**What was done**: Rewrote `fast-sdlc-manual-qa.md` to resolve every path dynamically, and folded in the consistency check flagged as technical debt by TASK-012 (whether manual-qa's own model needed adjustment given qa-automation's redesign):

- **BEFORE STARTING / OWNERSHIP**: added the same `fast-sdlc-project-setup.md` resolution pattern as `qa-automation` — own location, API Contract location (read-only), Task Management location — replacing every hardcoded `docs/qa-strategy.md`, `docs/qa/...`, `docs/stories/...` reference
- **Terminology aligned with the current `sdlc` skill**: `QA ✓` → Manual QA Passed flag, `QA Rec ✓` → Manual QA Reconciled flag (the shared skill now models these as Task Management status flags, not fixed table columns)
- **CHECKPOINT instructions removed** from all six modes — same fix already applied to `architect`/`qa-automation` this session (inline-spawned agents can't literally pause; checkpoints belong to the orchestrator)
- **MODE 1 (Setup)**: where story-level draft test cases live (inside the story itself vs. manual-qa's own location) is now an explicit Setup-time decision asked of the user, recorded in the QA strategy — resolves the actual design fork behind TASK-012's technical debt note; the file-based-task-management skill's Structure doesn't define a `test-cases/` subfolder under a story, so this was manual-qa's own bolt-on and needed an explicit decision rather than a silent default
- **MODE 1**: removed the "existing source code" branch entirely — manual-qa derives its initial test cases only from Documentation (never Architecture, API Contract, or source code), consistent with "not aware of how the system is implemented"
- **Execution Map**: corrected the WebFetch secondary-tool description — it observes which side (backend/frontend/other) a failure comes from, it does not "validate shapes" against anything
- **Modes 3/4/5**: corrected test execution to use only the primary tool set (per Execution Map) — the original wording bundled in secondary tools (WebFetch/Bash), which are for gathering extra detail on an already-found bug, not for running test cases
- **Bug Numbering**: deferred entirely to the active Task Management skill's own bug-creation flow, instead of duplicating/hardcoding a numbering scheme

**Deviations from plan**: Grew from the literal "dynamic paths" ask into resolving the design-fork technical debt TASK-012 flagged, per the same pattern as the last several tasks in this story.

**Technical decisions made**: Story-level draft TC location is a Setup-time decision, not a fixed default — recorded per-project in the QA strategy rather than assumed one way for every project.

**Code locations**: `fast-sdlc/agents/fast-sdlc-manual-qa.md`

**Technical debt noted**: None remaining from this consistency pass.
