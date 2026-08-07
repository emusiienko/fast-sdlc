---
status: Planned
type: chore
priority: TBD
date: 2026-08-07
---

# STORY-005: Split architect into architect and lead-developer

## Desired Outcome

Narrow `architect` to system design only: architecture, interfaces (API contracts), ADRs, technical documentation — cross-repo, stack-agnostic, one instance per project. Introduce a new `lead-developer` role, repo-bound: plans code changes for a story within a specific repo (implementation plan, task breakdown, sequencing) — one instance per repo in a multi-repo setup.

## Context

`architect`'s current scope conflates two different kinds of work: stable, cross-cutting system design (architecture, API contract, ADRs) and per-story code-change planning (implementation plan, task creation) — the latter is inherently repo-bound and requires deep familiarity with a specific codebase. This wasn't distinguished originally because cross-repo orchestration (multiple repos per project) wasn't considered when architect was designed. Both roles benefit from stronger/more expensive models for planning; actual implementation runs on cheaper models — this mirrors the model-tier split already in place (architect on `opus`, implementors presumably on a cheaper tier).

## Acceptance Criteria

- [ ] `architect` narrowed: Mode 1 (Project Setup) drops team/role composition setup (already superseded by orchestrator's Project Initiation per STORY-001) and implementation planning/task creation; keeps architecture, API contract, ADRs, CLAUDE.md
- [ ] New `fast-sdlc-lead-developer` agent created: owns the Implementation Plan artifact, Task creation/breakdown, per-repo sequencing — repo-bound, one instance per repo
- [ ] `sdlc` skill updated: new role added (Roles, Mapping table, Pipeline), Artifacts Ownership adjusted (Implementation Plan and Tasks ownership move to lead-developer)
- [ ] `orchestration` skill updated: spawn lead-developer instead of architect for implementation planning steps
- [ ] `session` skill updated if it references architect's planning responsibilities
- [ ] No leftover assumption that architect creates task files or implementation plans

## Edge Cases & Error Scenarios

- [ ] Single-repo projects: default to one lead-developer instance, same as today's architect behavior
- [ ] Model tier: lead-developer defaults to the same tier as architect, not the implementor tier
