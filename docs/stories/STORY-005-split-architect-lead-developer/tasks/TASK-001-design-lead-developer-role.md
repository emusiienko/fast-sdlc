---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-001: Design lead-developer role definition

## What to implement
Define the boundary precisely: exactly what stays with `architect` (architecture, API Contract, ADRs, CLAUDE.md, technical documentation) vs. what moves to `lead-developer` (Implementation Plan, Task creation/breakdown, per-story sequencing). Define `lead-developer`'s repo-bound semantics (one instance per repo in multi-repo setups — how it's identified/scoped), its model tier (same as architect, not implementor tier), and its one-line pipeline description for `sdlc`. Decide whether Reconcile-mode responsibilities split too (architect reconciles architecture/ADRs; does lead-developer reconcile the implementation plan against what was built, or does that fold into architect's Reconcile?).

## Scope
Design decisions only — no file writes beyond notes needed to brief the later tasks.

## Dependencies
None

## Acceptance Criteria
- [x] architect vs. lead-developer boundary stated explicitly, artifact by artifact
- [x] Repo-bound semantics defined (what "one instance per repo" means operationally)
- [x] Reconcile-mode split decided
- [x] Model tier decided

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-07
**What was done**: Designed the split through discussion, arriving at a three-layer "what vs. how" model consistent with business-analyst's Requirements/Documentation split:

- **business-analyst** → Requirements (business "what")
- **architect** → Technical Documentation (Architecture, API Contract, ADR — always exist, continuously kept current, no versioning ceremony) + **Plan for next scope** (a *new* artifact: high-level, concrete, incremental changes to Technical Documentation — Models, API Contract deltas, infrastructure decomposition, dependencies, platforms involved — scoped per milestone or story group, not per single story; exists only while there's unreconciled Requirements to plan against)
- **lead-developer** → repo-bound: on first run for a repo, creates that repo's coding conventions (moved off backend-developer/frontend-developer, which now only *follow* conventions instead of authoring them — the smarter/planning-tier model should make these judgment calls, not the implementor); then, per story, Implementation Plan + Task files (the repo-specific "how")

Rejected along the way: an earlier "architect creates some tasks directly (Protected Tasks)" idea — didn't survive scrutiny (no real example distinguished it from an ordinary task a lead-developer would write faithfully from the contract); cross-repo coordination is handled by the existing Technical Roadmap artifact instead, which lead-developers read, not by architect reaching into a repo's task tracker. Also rejected: a shared "Documentation" artifact co-owned by business-analyst and architect — architect's technical documentation role is already served by the existing Architecture/API Contract artifacts, no new artifact needed on that side; "Documentation" (as introduced in STORY-002) stays business-analyst-only.

**Modes**:
- architect: Project Setup (initial Architecture/API Contract/CLAUDE.md + first Plan for next scope — no team/role setup, that's orchestrator's job per STORY-001), Plan Next Scope (produce/update Plan for next scope for new Requirements, includes the pre-detail Deviation Check round-trip with affected lead-developers), Reconcile (merge Plan for next scope into Technical Documentation, create ADRs)
- lead-developer: Plan Implementation (conventions on first run, then Implementation Plan + Tasks per story), Reconcile (reconcile own Implementation Plan against Implementation Reports; raise architecturally-significant deviations to architect)

**Deviation Check loop** (mirrors business-analyst/Requirements): pre-detail, before lead-developer creates Tasks — and post-implementation, using Implementation Reports — both routed lead-developer → architect.

**Repo-bound semantics**: one lead-developer instance operates against one repo; multi-repo coordination (spawning multiple instances, assigning them to repos) is orchestrator's concern, out of scope here — true parallel/multi-repo execution is itself a future capability (Phase 2/3 in the project's own validated idea), not something to build into this agent's own design now. Lead-developer just needs to be repo-aware (resolve its own repo's config/conventions), not repo-orchestration-aware.

**Model tier**: lead-developer same tier as architect (currently `opus`) — both are planning-tier work; implementors stay on the cheaper tier.

**Devops**: explicitly out of scope for this split — stays as-is, no lead role introduced for it.
**Deviations from plan**: Scope grew from "define the boundary" to actually renaming/introducing two new artifacts (Plan for next scope, and clarifying Technical Documentation as an umbrella term) and one ownership shift (conventions: developer → lead-developer) beyond what the task originally anticipated.
**Technical decisions made**: See above — the artifact model and mode mapping are the key decisions.
**Code locations**: Design only, to be materialized in TASK-002/003.
**Technical debt noted**: Format/location for Plan for next scope, Implementation Plan, and lead-developer's conventions file still need Step 0 (Confirm Artifact Formats) treatment when TASK-002/003 write the actual agents — same pattern as business-analyst.
