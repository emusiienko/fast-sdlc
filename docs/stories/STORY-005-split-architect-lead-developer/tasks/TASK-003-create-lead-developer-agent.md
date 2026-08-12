---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-003: Create fast-sdlc-lead-developer agent

## What to implement
New agent, repo-bound, owning the Implementation Plan artifact and Task creation/breakdown per TASK-001's design. Built dynamic-path-aware from the start (`fast-sdlc-project-setup.md`), same pattern as `fast-sdlc-business-analyst.md`.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-lead-developer.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] Agent frontmatter (`name`, `description`, examples, model tier per TASK-001) complete
- [x] Implementation Plan and Task creation content ported from `fast-sdlc-architect`'s Feature Design mode
- [x] No hardcoded `docs/...` paths

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-12
**What was done**: Created `fast-sdlc-lead-developer.md` (model: opus, mirrors architect's tier) and ported/redesigned the old architect Feature Design content around it, through an extended interactive design pass that went well beyond the original task text:

- **5 modes** (grew from the originally-envisioned 3): Setup (repo/stack exploration → Coding Conventions, grounded in the real codebase or a matching starter template — see below), Plan Implementation (Implementation Plan only), Create Tasks (split out from planning — triggered only once the plan is approved, so tasks are never created before the plan itself is reviewed), Reconcile (own Implementation Plan vs. Implementation Reports, Repo-level ADR creation), Implement (explicit-request-only exception where lead-developer writes code itself, reusing `sdlc`'s Quick Fix/Bug Fix workflows rather than inventing a new one).
- **Stack parameterization**: lead-developer is repo-bound but scoped to exactly one stack per session (resolved in BEFORE STARTING — from briefing, or asked if the repo has more than one and it's ambiguous). A repo with multiple stacks (e.g. a monorepo with backend + frontend) gets one lead-developer session per stack, each with its own Coding Conventions, Implementation Plans, and Repo-level ADRs. Setup mode checks a new `templates/lead-developer/conventions/` folder for a pre-written starter matching the detected stack (e.g. `python-fastapi.md`) before falling back to research.
- **Repo-level ADRs**: a new artifact, parallel to architect's project-level ADRs but scoped to decisions internal to one repo/stack (library choices, internal structure) — mandatory-vs-architect's-call split decided via a repo-scoped TECH DECISIONS POLICY mirroring architect's.
- **API/Architecture boundary consistency**: the design repeatedly cross-checked against architect's just-finished bounded-context model (TASK-002) — e.g. Coding Conventions deliberately stay code-aware/concrete (exact modules/files, no forced abstraction) since lead-developer, unlike architect, actually explores the real repo.
- **Config wiring**: added a `lead-developer` row to `fast-sdlc-project-setup.md` > Team (was missing entirely); added Coding Conventions / Implementation Plan / Repo-level ADRs chapters to Artifacts (location only — format intentionally excluded from the shared config, since nobody but lead-developer itself reads a Coding-Convention's structure, only its content); added a new top-level `## Escalation/Feedback` chapter, marked TBD, since no mechanism exists yet for cross-role or user-approval signaling (surfaced while designing the Create-Tasks trigger and the architect Deviation Check).
- **Checkpoints removed**: after tracing how "CHECKPOINT — wait for the user's go-ahead" instructions actually get enforced (they don't, uniformly — depends on face-to-face session vs. inline orchestrator spawn per `orchestration` skill's own rule), removed all inline CHECKPOINT markers from this agent in favor of structural mode boundaries (Create Tasks mode's own trigger, gated on approval — mechanism TBD, see Escalation/Feedback) doing the same job without the ambiguity.
- **Templates externalized**: `templates/lead-developer/{implementation-plan,coding-conventions,claude-md,adr}-format.md`, mirroring the business-analyst/architect precedent; Implementation Plan/CLAUDE.md/ADR templates get copied into lead-developer's own folder (reused repeatedly, need consistency across invocations); Coding Conventions is the deliberate exception — generated once per stack, so no copy step, no format confirmation with the user, only the final content goes through review.

**Deviations from plan**: Substantial — scope grew from "port Feature Design content" into a full ground-up design covering stack-scoping, a 5th (later split into distinct 3rd/5th) mode, Repo-level ADRs, and two config additions (Team row, Escalation/Feedback chapter) never anticipated in TASK-001.

**Technical decisions made**: See above. The two most consequential for later tasks: (1) Create Tasks is its own mode, not a step inside Plan Implementation — TASK-004 (sdlc skill) needs to reflect this in the Task Lifecycle section; (2) "approval" and cross-role escalation have no defined mechanism yet (Escalation/Feedback: TBD) — worth resolving before relying on it operationally.

**Code locations**: `fast-sdlc/agents/fast-sdlc-lead-developer.md`; `fast-sdlc/templates/lead-developer/*.md` (new); `fast-sdlc/templates/fast-sdlc-project-setup.md` (added Team row + 3 Artifacts chapters + Escalation/Feedback chapter).

**Technical debt noted**: `templates/lead-developer/conventions/` referenced but left empty — user will populate stack-specific starters later. Escalation/Feedback mechanism is TBD — both lead-developer's Deviation Checks with architect and MODE 3's "approved" trigger depend on it eventually being defined. `fast-sdlc-business-analyst.md`'s own Step 0 still doesn't write its own location into config (same gap noted in TASK-002, still unfixed, still out of scope).
