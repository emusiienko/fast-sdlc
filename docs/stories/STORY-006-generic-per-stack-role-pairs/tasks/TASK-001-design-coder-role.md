---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-001: Design coder role definition

## What to implement
Define the boundary precisely: what `coder` does (implements Tasks, follows Coding Conventions) vs. what stays with `lead-developer` (authors Coding Conventions, plans, creates Tasks). Define `coder`'s stack-parameterization (one session per stack, mirroring `lead-developer`'s BEFORE STARTING pattern — resolve from briefing, or ask if the repo has more than one stack and it's ambiguous). Confirm model tier (implementor/cheap, same as today's `backend-developer`/`frontend-developer` — not `lead-developer`'s `opus` tier). Confirm `devops` stays out of scope (already decided in STORY-005 TASK-001, restate here for this story's own record). Decide whether `coder` supports the Deviation Check pattern (e.g. raising to `lead-developer` if a Task doesn't match reality) the same way `lead-developer` does with `architect`.

## Scope
Design decisions only — no file writes beyond notes needed to brief the later tasks.

## Dependencies
None

## Acceptance Criteria
- [x] coder vs. lead-developer boundary stated explicitly, artifact by artifact
- [x] Stack-parameterization mechanics defined (mirrors lead-developer's own pattern)
- [x] Model tier decided
- [x] devops exclusion confirmed
- [x] Deviation Check applicability decided
- [x] coder's other boundary (vs. the user) stated: coder has no Reconcile mode of its own — it makes fixes per direct user feedback during implementation, not a self-driven reconciliation step; any discrepancy the user escalates gets recorded in the Implementation Report, for lead-developer's own Reconcile to pick up later

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Designed the `coder` role through discussion, confirming `backend-developer`/`frontend-developer` both currently run on `sonnet` (checked their frontmatter directly rather than assuming) before deciding `coder`'s own tier.

- **Boundary vs. lead-developer, artifact by artifact**: Coding Conventions — lead-developer authors, coder only follows; Implementation Plan — lead-developer authors, coder only reads; Tasks — lead-developer creates (its own MODE 3), coder implements + fills its own Implementation Report + advances status; Repo-level ADRs — lead-developer owns exclusively, coder never touches them directly; `CLAUDE.md` — lead-developer owns the file, but coder keeps write access to the structural/code-map section only, carrying forward the sub-ownership split `backend-developer`/`frontend-developer` already had (`## Code Map` only, full ownership elsewhere)
- **Stack-parameterization**: identical mechanic to `lead-developer` — resolved in coder's own BEFORE STARTING (from briefing, or asked if the repo has more than one stack and it's ambiguous), scoping the whole session to it
- **Model tier**: `sonnet` — unchanged from `backend-developer`/`frontend-developer`
- **devops**: confirmed out of scope, stays separate (restates STORY-005 TASK-001's original decision for this story's own record)
- **Deviation Check**: coder doesn't plan, so it has no pre-detail Deviation Check the way `lead-developer`/`architect` do — but it needs the equivalent at implementation time: if a Task doesn't match what's actually in the code, stop and raise it to `lead-developer` rather than guessing. Framed as an extension of the existing Bug Fix "Domain check" pattern, not a new mechanism.
- **Boundary vs. the user**: coder has no Reconcile mode of its own (per the acceptance criterion already written into this task) — it fixes per direct user feedback, and any escalated discrepancy lands in the Implementation Report for `lead-developer`'s own Reconcile to pick up
- **Modes** (for TASK-002): Implement Task, Quick Fix, Bug Fix (ported/generalized from `backend-developer`/`frontend-developer`) plus a new PR Review Response mode wired to the `pr-review-response` skill (TASK-008, already built)

**Deviations from plan**: None — scope matched the task description closely; the PR Review Response mode addition was already anticipated from the TASK-008 work done just before this.

**Technical decisions made**: See above — the CLAUDE.md sub-ownership split and the "Deviation Check as extended Domain check" framing are the two decisions most likely to matter when TASK-002 actually writes the agent file.

**Code locations**: Design only, to be materialized in TASK-002.

**Technical debt noted**: None new.
