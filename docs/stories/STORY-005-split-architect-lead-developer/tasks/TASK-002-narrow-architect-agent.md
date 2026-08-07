---
status: Done
assignee: orchestrator
story: STORY-005
---

# TASK-002: Narrow fast-sdlc-architect agent

## What to implement
Remove from `fast-sdlc-architect.md`: team/role composition setup (Mode 1 step 2 — superseded by orchestrator's Project Initiation per STORY-001), Feature Design mode's implementation planning and task creation (Mode 2 entirely — moves to lead-developer). Keep: architecture, API Contract, ADRs, CLAUDE.md, Project Setup (minus team setup), Reconcile (per TASK-001's split decision). Rewrite against the dynamic config model (`fast-sdlc-project-setup.md`), same as `fast-sdlc-business-analyst.md` — this agent doesn't need a separate "make it dynamic" pass afterward.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-architect.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] No team/role setup responsibility remains
- [x] No implementation planning or task creation remains
- [x] No hardcoded `docs/...` paths — resolves from `fast-sdlc-project-setup.md`

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-07
**What was done**: Rewrote `fast-sdlc-architect.md` end to end, going well beyond the original scope (see Deviations) through an extended interactive design pass with the user. Final shape:

- **Territory**: Technical Documentation (Architecture, API Contract, ADRs — always current) + Technical Roadmap (the plan). No team/role setup, no Implementation Plan/Tasks (lead-developer's job).
- **OWNERSHIP/BEFORE STARTING**: rewritten dynamic, mirrors `fast-sdlc-business-analyst.md` — locates `fast-sdlc-project-setup.md`, no `docs/team.md`.
- **MODES**: Project Setup (config not yet set on this project — produces the Detailed Technical Roadmap + `CLAUDE.md` only; Architecture/API Contract do NOT get written here, they're populated at the first Reconcile from what actually got built), Plan Next Scope (config already set; produces/updates the Technical Roadmap — Detailed or Incremental, architect's judgment call per scope, not fixed by mode), Reconcile (creates-or-updates Architecture/API Contract from the Roadmap, creates ADRs).
- **Technical Roadmap**: merged what were originally two artifacts ("Plan for next scope" and a separate "Technical Roadmap") into one, since the second was fully redundant with the first's own Dependencies/Platforms Involved sections. Two forms: Detailed (full Architecture + API Contract + a Steps section mapped to stories — used at Project Setup or for any substantial change) and Incremental (only the Architecture section names that actually change, for small deltas). Incremental form's sections were renamed to match Architecture's own section names 1:1, so Reconcile is a direct per-section merge, not a re-interpretation.
- **Architecture structure**: reworked from scratch after a series of challenges — dropped `System Modules`/`Module Relations` (code paths and internal call-graphs — lead-developer's domain, not cross-repo), dropped `State Management` and `Environment Configuration` (single-repo/implementation detail), dropped `Tailwind`/`SQLite vs Postgres`/library-named examples throughout (CONSTRAINTS, TECH DECISIONS POLICY) in favor of technology-agnostic principles. Landed on: Tech Stack, Services (grouped by **bounded context**, not by deployability — a bounded context can contain many independently-deployable units, e.g. an API Gateway + its Lambdas), Service Relations, Data Flow, Database, Authentication Flow, Error Handling Strategy, Logging/Observability, Deployment Topology.
- **API Contract scope rule**: mandatory only for Service Relations that cross a bounded-context boundary; architect's judgment for relations within one bounded context. Resolves the "15-20 microservices" / "every Lambda has an interface" scaling problem — deployability isn't the boundary that matters, bounded context (a DDD concept) is.
- **Templates externalized**: moved the entire "DOCUMENT SPECIFICATIONS" section out of the agent file into 5 standalone files under `fast-sdlc/fast-sdlc/templates/architect/` (architecture-format.md, api-contract-format.md, technical-roadmap-format.md, adr-format.md, claude-md-format.md), mirroring the business-analyst precedent exactly. Step 0 now points to each; only the ADR immutability rule and decision-criteria stayed inline in agent.md (behavioral policy, not document structure).
- **CLAUDE.md**: reconsidered mid-task — not lead-developer's exclusively, not a single project-wide file either. It's owned by whichever role's own folder a Claude Code session might be launched from; each role (architect included) maintains its own, scoped to what's actually in that folder (Commands section conditional — omitted for a docs-only location).
- **Config wiring**: Step 0 now also writes `**location**` (+ a short note) into `fast-sdlc-project-setup.md` > Artifacts > Architecture/API Contract/Technical Roadmap — no `**format**` field there, format stays purely in architect's own folder. Added a `### Documentation` chapter (location only) to the shared `fast-sdlc-project-setup.md` template, which had never gotten one since STORY-002 introduced the artifact.

**Deviations from plan**: Substantial. The original task scope ("remove team setup + Feature Design mode") turned into a full redesign of Architecture's own structure and the Technical Roadmap concept, driven by the user's repeated challenges (bounded context vs. deployability, Detailed-vs-Incremental as a judgment call rather than a mode-bound rule, MODE 1/2 triggers keyed off config state rather than artifact existence). None of this was anticipated in TASK-001.

**Technical decisions made**: See above — bounded context as the API Contract boundary, and Detailed/Incremental as an explicit per-scope decision (architect asks the user if unsure) are the two decisions most likely to need revisiting elsewhere (lead-developer's own scope, and `sdlc` skill's artifact descriptions in TASK-004).

**Code locations**: `fast-sdlc/fast-sdlc/agents/fast-sdlc-architect.md`; `fast-sdlc/fast-sdlc/templates/architect/*.md` (new); `fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md` (added Documentation chapter).

**Technical debt noted**: `fast-sdlc-business-analyst.md`'s own Step 0 has the same gap this task fixed for architect — it never explicitly writes `**location**` into its own config chapters, only confirms format. Not fixed here (out of scope for STORY-005). `sdlc` skill (TASK-004) still describes the pre-split model (architect owns Implementation Plan/Tasks, no lead-developer, old Technical Roadmap definition as "longer-horizon") and needs to catch up to everything decided here.
