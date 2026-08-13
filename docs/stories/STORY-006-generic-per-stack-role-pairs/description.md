---
status: Accepted
type: chore
priority: TBD
date: 2026-08-13
---

# STORY-006: Generic, configurable per-stack Lead+Developer role pairs

## Desired Outcome

Replace `backend-developer` and `frontend-developer` with a single stack-agnostic `coder` role (implementor tier) — the other half of the "Lead + Developer" pair, now that `lead-developer` (STORY-005) already exists and owns each stack's Coding Conventions. `coder` reads whichever stack's Coding Conventions apply to its session, the same way `lead-developer` already scopes itself to one stack per session, instead of being hardcoded per tech stack.

## Context

STORY-005 built the "Lead" half of this pair already: `lead-developer` is repo-bound, scoped to one stack per session, and owns that stack's Coding Conventions, Implementation Plan, and Tasks. The "Developer" half is still two hardcoded, stack-specific agents (`backend-developer`, `frontend-developer`) that each author their own conventions — a responsibility that no longer belongs to them (per STORY-005 TASK-001's decision, moved to `lead-developer`; flagged as technical debt during STORY-005 TASK-007's consistency check). This story finishes the generalization: one `coder` role, parameterized by stack the same way `lead-developer` is, that only *follows* Coding Conventions rather than authoring them.

`devops` is explicitly excluded from this generalization (per STORY-005 TASK-001) — it stays a separate, fixed role.

## Acceptance Criteria

- [x] `coder` role designed: boundary vs. `lead-developer` (coder implements, never authors Coding Conventions), stack-parameterization mirroring `lead-developer`'s BEFORE STARTING pattern, model tier (implementor/cheap — same tier as today's `backend-developer`/`frontend-developer`)
- [x] New `fast-sdlc-coder` agent created, merging `backend-developer` + `frontend-developer`'s Implement Task/Quick Fix/Bug Fix modes; drops the self-authored-conventions Setup mode entirely (superseded by `lead-developer`'s own Setup)
- [x] `sdlc` skill updated: `coder` replaces `backend-developer`/`frontend-developer` in Roles, Mapping, Pipeline, Task Lifecycle, Artifacts Ownership, Common Workflows
- [x] `orchestration` skill updated: spawns `coder` instead of `backend-developer`/`frontend-developer` (turned out to need no changes — already role-agnostic)
- [x] `fast-sdlc-project-setup.md` template updated: Team row for `coder` (per stack, same pattern as `lead-developer`); `backend-developer`/`frontend-developer` rows removed
- [x] `fast-sdlc-backend-developer.md` and `fast-sdlc-frontend-developer.md` retired
- [x] No leftover assumption anywhere that `backend-developer`/`frontend-developer` exist as separate roles

## Edge Cases & Error Scenarios

- [x] `devops` stays a separate, fixed role — explicitly not merged into `coder`
- [x] A repo with multiple stacks: one `coder` session per stack, same pattern as `lead-developer`
- [x] STORY-001's TASK-009 (`backend-developer` dynamic paths) and TASK-011 (`frontend-developer` dynamic paths) closed as superseded
