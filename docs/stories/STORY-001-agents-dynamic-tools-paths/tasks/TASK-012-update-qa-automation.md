---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-012: Update fast-sdlc-qa-automation agent

## What to implement
Update the agent to resolve test-script and strategy-doc locations from `fast-sdlc-project-setup.md` instead of assuming fixed `docs/` paths.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-qa-automation.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] No remaining hardcoded path this agent should instead resolve from config (verified via grep, no matches)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-15
**What was done**: Grew well past the original dynamic-paths scope into a full redesign of `fast-sdlc-qa-automation.md`, prompted by a realization during STORY-006 (coder now writes its own unit/integration tests, making qa-automation's old "Write Tests" task-assignment mode redundant with what coder already does in the same repo):

- **Removed MODE 2 "Write Tests"** entirely (used to receive assigned coverage zones via a Task file) — replaced with **MODE 2: Check Story — Prepare**, mirroring `manual-qa`'s own "Prepare" mode: reads the API Contract + story acceptance criteria and decides its own coverage, independent of any Task/Implementation Report
- **Narrowed scope to black-box API/contract and cross-service testing only** — explicit everywhere (Your Role, Setup, SELF-VERIFICATION) that unit/integration tests inside an implementation repo are `coder`'s job, never qa-automation's
- **Tool default**: `@playwright/test` (TypeScript), using its `request` fixture for API-level testing without a browser — chosen because `manual-qa` already depends on Playwright in this plugin, no new tool to introduce; explicitly distinguished from the Playwright MCP browser tools (`manual-qa`'s interactive/live tooling) — qa-automation writes committed test code, run via Bash (`npx playwright test`), reproducible by a human or CI
- **Dedicated location**: its own folder/repo (via `fast-sdlc-project-setup.md` > Team > qa-automation), not inside any implementation repo — since these tests are cross-repo/cross-service by nature
- **Environments**: Setup mode now asks about local vs. remote/staging target environments, and to check with devops about what's actually provisioned
- **Execute mode simplified**: originally ran the test suite once, then re-ran each `fixed` bug's test individually (redundant double execution) — corrected to a single run whose results get cross-referenced against `fixed` bugs, scoped to either one story or the full suite depending on the request
- Fully dynamic-path-aware throughout (satisfies the original TASK-012 ask)

**Deviations from plan**: Substantial — same pattern as STORY-005/006's own TASK-002s. Scope grew from "make paths dynamic" into redesigning what the role even does, driven by a direct conversation about the redundancy `coder`'s new test-writing responsibility created.

**Technical decisions made**: See above. Most consequential: qa-automation no longer receives task assignments for what to test — it derives coverage independently from the API Contract and story acceptance criteria, the same model `manual-qa` already used. This makes the two QA roles structurally consistent with each other.

**Code locations**: `fast-sdlc/agents/fast-sdlc-qa-automation.md`

**Technical debt noted**: `fast-sdlc-manual-qa.md` hasn't been reviewed yet for consistency with this redesign (e.g., whether its own test-case model or bug-filing conventions need any adjustment given qa-automation's new shape) — next task.
