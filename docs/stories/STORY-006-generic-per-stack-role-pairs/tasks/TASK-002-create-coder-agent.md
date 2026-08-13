---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-002: Create fast-sdlc-coder agent

## What to implement
New agent, merging `fast-sdlc-backend-developer.md` and `fast-sdlc-frontend-developer.md` per TASK-001's design. Stack-parameterized from the start (one session per stack, same mechanic as `fast-sdlc-lead-developer.md`). Drops the self-authored-conventions Setup mode entirely — reads Coding Conventions from `lead-developer`, never writes them. Keeps Implement Task, Quick Fix, Bug Fix modes (generalized, not backend/frontend-specific). Adds a mode for responding to PR review comments, triggered on request via orchestrator (not face-to-face, not automatic) — uses the `pr-review-response` skill (TASK-008) for the mechanics; this mode's own trigger and "fix per Coding Conventions" framing are all that's role-specific.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-coder.md`

## Dependencies
TASK-001, TASK-008

## Acceptance Criteria
- [x] Agent frontmatter (`name`, `description`, model tier per TASK-001) complete
- [x] Implement Task, Quick Fix, Bug Fix modes ported and generalized from backend-developer/frontend-developer
- [x] No self-authored-conventions Setup mode
- [x] No hardcoded `docs/...` paths — resolves from `fast-sdlc-project-setup.md` (verified via grep, no matches)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Created `fast-sdlc/agents/fast-sdlc-coder.md`, merging `fast-sdlc-backend-developer.md` and `fast-sdlc-frontend-developer.md` per TASK-001's design, then refined through a long interactive pass:

- **Structure**: 4 modes (Implement Task, Quick Fix, Bug Fix, PR Review Response — no Setup mode); dynamic-path-aware from the start, mirroring `fast-sdlc-lead-developer.md`'s BEFORE STARTING/stack-resolution pattern exactly
- **Dropped the entire "CONVENTIONS TEMPLATE" block** (Generic/Node.js/Python/React/Next.js content that `backend-developer`/`frontend-developer` used to author their own conventions) — not migrated anywhere; still recoverable from git history if it's useful later to seed `templates/lead-developer/conventions/`
- **OWNERSHIP correction**: initially wrote Tasks as "lead-developer creates them" only, missed that Quick Fix requires coder to self-create a Story+Task — fixed to reflect both paths
- **GIT POLICY reversed** from backend-developer/frontend-developer's "never commit automatically" to "commit as the final step of each unit of work, one commit per Task/Quick Fix/Bug Fix" — safe because it's always on a branch, decided earlier in the same session (STORY-006 design conversation, before TASK-001)
- **CLAUDE.md code-map ownership removed entirely** — originally carried forward from backend-developer/frontend-developer's sub-permission, but doesn't survive scrutiny: `lead-developer`'s own Reconcile already updates `CLAUDE.md` once, with the full picture, after all Tasks are done; coder writing to the same file per-Task risks divergent writes across parallel/multi-branch sessions for no real benefit
- **Domain/reality check** added to MODE 1 and MODE 3 — coder's equivalent of `lead-developer`'s pre-detail Deviation Check, raising to `lead-developer` if a Task's premise doesn't match the actual code, rather than guessing
- **New `## TESTING` section** — a mid-session realization: `qa-automation` writing tests in the same repo as coder is redundant, and denying coder its own test suite as a verification tool was already visibly pushing it toward bad habits (inline snippets) despite explicit instructions not to. Writing unit/integration tests is now coder's own responsibility; TDD-or-not and coverage expectations are this stack's Coding Conventions' call, not fixed here. Added 4 guidelines after several rounds of correction: tests must not *contradict* requirements (not "trace directly to" them — many legitimate tests, e.g. for internal modules, don't map to acceptance criteria at all); no coverage-padding tests; when requirements change existing covered behavior, rewrite the test's assertions from the new requirement itself, not by adjusting to whatever the changed code now outputs; don't invent throwaway code just to have something to test.
- Updated `templates/lead-developer/coding-conventions-format.md`'s "Testing Approach" section to explicitly prompt for TDD-or-not, unit/integration frameworks, and depth of coverage — matching what coder.md now defers to it

**Deviations from plan**: Substantial, in the same pattern as TASK-002 of STORY-005 — scope grew well past "merge two files" into a real design pass (GIT POLICY reversal, CLAUDE.md ownership removal, the whole TESTING section) driven by direct critique during review.

**Technical decisions made**: See above. The two most consequential for later work: (1) coder now writes its own tests — `qa-automation`'s role likely needs reconsidering as a result, not addressed here, out of this task's scope; (2) `CLAUDE.md` code-map updates are `lead-developer`-only now, a correction that should probably also be checked against `fast-sdlc-lead-developer.md`'s own Reconcile wording to make sure it's actually doing this consistently (not verified in this task).

**Code locations**: `fast-sdlc/agents/fast-sdlc-coder.md` (new); `fast-sdlc/templates/lead-developer/coding-conventions-format.md` (Testing Approach section expanded)

**Technical debt noted**: `qa-automation`'s role and responsibilities need reconsidering now that coder owns unit/integration testing directly — not designed here, flagged for a future task/story. `templates/lead-developer/conventions/` remains unpopulated (user will add stack-specific starters later) and the old backend/frontend-developer CONVENTIONS TEMPLATE content was not migrated into it.
