---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-004: Update sdlc skill

## What to implement
Add `lead-developer` to Roles, the Mapping table, and the Pipeline. Move Implementation Plan and Tasks ownership from architect to lead-developer in Artifacts Ownership. Update any Common Workflow steps that assume architect creates the Implementation Plan or Tasks (e.g. Implement Story's Plan step).

## Scope
`fast-sdlc/fast-sdlc/skills/sdlc/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] `lead-developer` present in Roles, Mapping table, Pipeline
- [x] Implementation Plan and Tasks ownership reassigned
- [x] No remaining mention of architect creating Tasks or the Implementation Plan

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-12
**What was done**: Updated `fast-sdlc/skills/sdlc/SKILL.md` (the repo-local copy — noted the global `~/.claude/skills/sdlc/SKILL.md` is a stale, unrelated older copy, left untouched as out of scope) to reflect everything decided in TASK-002/003:

- **Artifacts**: redefined Technical Roadmap (Detailed/Incremental, no longer "optional"/"longer-horizon"); added Coding Conventions and Repo-level ADRs as new artifacts; scoped Implementation Plan to "within a single repo (or repo+stack)"
- **Task Statuses**: `ToDo` no longer attributed solely to architect — generalized to "the creator (usually lead-developer; an implementor may self-create one, e.g. via Quick Fix)"
- **Roles**: added lead-developer (Phase 4, Repo Planning); trimmed architect's role description to drop the vacuous "does not do X, that's Y's job" tail (per the same simplification applied throughout TASK-002/003) rather than enumerating everything it no longer does; renumbered backend-developer/devops/frontend-developer to Phase 5, qa-automation/manual-qa to Phase 6
- **Mapping table**: added lead-developer row
- **Pipeline**: 6 phases → 7 (inserted "4. Repo Planning" for lead-developer between Architecture and Implementation); phase 3's Artifacts trimmed to Architecture/API Contract/Technical Roadmap only; phase 7 (Reconcile) sequence extended to lead-developer → architect → business-analyst
- **Common Workflows > Setup**: added lead-developer row (Coding Conventions, CLAUDE.md, own agent.md)
- **Implement Story**: split the old single "Plan" step into "Roadmap" (architect) + "Plan" (lead-developer, per repo/stack, with a Deviation Check to architect first); Implement step now spans "all repos' Implementation Plans"; post-implementation Deviation Check now routes through lead-developer first (repo-level ADR vs. escalate to architect) before reaching business-analyst
- **Implement Task**: added Coding Conventions to the required reading list
- **Reconcile workflow**: added a new "Lead-Developer Reconcile" sub-section (runs first, per repo) before Architect Reconcile; Architect Reconcile's step 2 now explicitly says "fold the Technical Roadmap into Architecture, API Contract" rather than the vaguer "update to reflect what was built"
- **Artifacts Ownership**: Implementation Plan and Tasks reassigned to lead-developer; added Coding Conventions and Repo-level ADRs rows; Technical Roadmap's "(optional)" dropped
- **Face-to-face sessions**: added lead-developer to the callee list

**Deviations from plan**: Scope was larger than the task's literal wording (Roles/Mapping/Pipeline/Ownership) — also touched Implement Story, Implement Task, and the Reconcile workflow, since leaving those pointed at "architect produces the Implementation Plan" would have directly contradicted the ownership reassignment made two sections earlier in the same file.

**Technical decisions made**: Reconcile now runs lead-developer → architect → business-analyst (was architect → business-analyst) — matches `fast-sdlc-lead-developer.md`'s own MODE 4, which explicitly surfaces deviations to architect "before architect's own Reconcile runs."

**Code locations**: `fast-sdlc/skills/sdlc/SKILL.md`

**Technical debt noted**: The face-to-face session mechanism and the Deviation Check "raise to architect" instructions still don't have a defined transport (see STORY-007, `Escalation/Feedback: TBD` in `fast-sdlc-project-setup.md`) — `sdlc` skill's Reconcile/Implement Story wording describes the same TBD-dependent flow now propagated into the shared skill, not just the two agent files.
