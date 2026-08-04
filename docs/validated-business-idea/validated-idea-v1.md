# Validated Idea: Fast SDLC

> **Status:** Validated — core problem and Phase 1 scope confirmed through discovery session
> **Date:** 2026-08-03
> **Prepared by:** Product Strategist

## Problem Statement
Evgeniy's current AI-agent-driven development setup lives entirely in global, untracked local configuration (agent and skill definitions outside any repository) and runs sequentially, under constant manual supervision. There is no way today to scope several stories, hand them to agents working in isolation, and return later to a set of ready-to-review pull requests. Producing 3–4 stories' worth of agent-driven work to a mergeable, reviewable state realistically takes multiple days of active supervision, not a single self-paced day.

## Target Users
- **Evgeniy — Solo Developer / Orchestrator** — sole intended beneficiary; runs a multi-agent SDLC pipeline across several projects; technically fluent; wants to move from "one story at a time, supervised" to "several stories worked in parallel, reviewed once, in a day."
- **AI Subagents** (product-strategist, architect, spec-writer, backend-developer, frontend-developer, qa-automation, devops, etc.) — the workforce this framework defines and orchestrates. Not a persona with independent needs; the mechanism through which Evgeniy's goal is delivered.

## Core Value Proposition
Turning the SDLC agent/skill definitions into first-class, versioned, in-repo artifacts — instead of untracked global config — lets the process evolve deliberately toward one North Star outcome: Evgeniy scopes 3–4 stories, agents work autonomously in parallel, and by day's end there are 3–4 pull requests ready for his review, manual per-branch testing, and merge. Unlike today's file-based, single-repo-coupled, sequential setup, Fast SDLC decouples the process from any one target project and makes it a maintainable product with its own roadmap.

## MVP Scope (Phase 1 — validated in this document)

### ✅ IN
- Relocate the current agent and skill definitions (SDLC roles, orchestration, session flow) from global/untracked local config into this dedicated repository, under top-level `agents/` and `skills/` folders — version-controlled and evolvable
- Integration with the existing AI Agent Coordination Utility (task manager) as the state backend the agents read/write against, via its existing MCP surface
- Continuation of the current human-in-the-loop orchestration model (face-to-face sessions, role definitions) exactly as it works today — just repo-hosted instead of global

### ❌ OUT
- **Docker-based isolated execution (Phase 2)** — *Rationale: required for safe unattended parallel story execution, but not needed to prove the definitions can live and evolve as repo-tracked artifacts. Sequenced next.*
- **PR-based validation and approval workflow (Phase 3)** — *Rationale: depends on Phase 2's isolation to be safe and useful across concurrently-worked branches; premature before parallel execution exists.*
- **Packaging as a distributable plugin + automated install/deployment script** — *Rationale: explicitly undecided/aspirational; revisit once the in-repo structure and process stabilize.*
- **Running multiple stories fully unattended in parallel within one day** — *Rationale: this is the product's North Star once all three phases exist, not a Phase 1 deliverable.*
- **Any beneficiary beyond Evgeniy** — *Rationale: single-beneficiary by design for now; multi-user distribution out of scope until a concrete second user exists.*

## User Roles & Permissions
| Role | Can Do | Cannot Do |
|------|--------|-----------|
| Human Orchestrator (Evgeniy) | Defines/evolves agent and skill definitions; runs face-to-face sessions; reviews and approves all outcomes | Nothing restricted — sole operator |
| AI Subagents | Operate within their defined role/skill scope; read/write state via the task manager | Act outside assigned role or story/task scope; approve or merge their own work (reserved for the human orchestrator, and — once Phase 3 exists — the PR gate) |

## Key Assumptions
- [ ] The current global agent/skill setup can be relocated into a repo-hosted structure without behavior regressions
- [ ] The AI Agent Coordination Utility's existing MCP surface is sufficient for Phase 1's integration — no new API surface is required from that product
- [ ] Docker-based isolation (Phase 2) will provide enough safety for autonomous, unattended, parallel story execution
- [ ] GitHub pull requests (Phase 3) are the right review/approval mechanism for this workflow
- [ ] A single beneficiary (Evgeniy) is sufficient justification to productize this as a separate, versioned framework rather than leaving it as personal dotfiles

## Risks & Mitigations
| Risk | Type | Likelihood | Impact | Mitigation |
|------|------|------------|--------|------------|
| Scope creep — all four capabilities (subagents, orchestration, quality gates, PR validation) pulled into one big-bang effort | Business | Medium | High | Enforce the 3-phase sequencing; each phase gets its own validation/spec cycle |
| Migration breaks the currently-working pipeline mid-transition (global config → repo-hosted) | Technical | Medium | High | Run repo-hosted and global definitions side by side until relocation is proven equivalent, then retire the global copies |
| Phase 1 alone doesn't move the needle on the North Star metric, risking the impression the investment isn't paying off | Adoption | Medium | Medium | Communicate Phase 1's success criterion explicitly as "definitions relocated and working," not "3-4 parallel PRs/day" |
| No decided packaging/distribution mechanism — "deployment" stays a manual, undocumented step | Technical | Low | Low | Explicitly deferred; revisit once the in-repo structure stabilizes |

## Success Metrics
- **Phase 1 (this MVP)**: 100% of current agent and skill definitions relocated from global config into the repo's `agents/`/`skills/` folders, functioning with no regression in any existing SDLC role or session flow — before Phase 2 work starts
- **North Star (full product, post Phase 2+3)**: Evgeniy scopes 3–4 stories, agents work autonomously in parallel, and he returns within the same day to 3–4 open pull requests ready for visual/manual check and merge — baseline today: sequential, manually supervised, no PR gate, effectively one story at a time

## Out of Scope for V1
- **Docker-based parallel execution** — Deferred to Phase 2, once Phase 1's repo relocation is proven stable.
- **PR-based validation/approval workflow** — Deferred to Phase 3, once Phase 2 provides safe isolation.
- **Plugin packaging / automated deployment script** — Deferred, undecided; revisit once the framework's shape stabilizes.
- **Any second beneficiary or distribution to other developers** — Deferred indefinitely; single-beneficiary by design for now.

## Open Questions
- [ ] Packaging/distribution mechanism (plugin vs. script vs. manual copy) — undecided
- [ ] Target timeframe for reaching the North Star metric (3–4 parallel PRs/day) — not yet set
- [ ] Transition management: will global (`~/.claude/agents`, `~/.claude/skills`) and repo-hosted definitions run side by side, and for how long?
