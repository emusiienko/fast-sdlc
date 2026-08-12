---
status: Planned
type: TBD
priority: TBD
date: 2026-08-12
---

# STORY-007: Design a feedback loop between agents

## Desired Outcome

## Context

`fast-sdlc-project-setup.md` has an `## Escalation/Feedback` chapter marked TBD — there is no defined mechanism today for one agent to raise something to another, or to the user, once it has already produced its output. Several places in the plugin already assume this exists without it being designed:

- architect's Deviation Check (pre-detail, in both Project Setup and Plan Next Scope modes) — presenting the Technical Roadmap for review
- lead-developer's Deviation Check (pre-detail) — raising to architect that the Technical Roadmap doesn't fit this repo's reality, then stopping until architect responds
- lead-developer's Deviation Check (post-implementation, in Reconcile) — raising an architecturally significant deviation to architect
- lead-developer's MODE 3 (Create Tasks) trigger — depends on an Implementation Plan being "approved," with no defined way to signal that

The core difficulty: most agents run as one-shot subagents that return final text and end (per `orchestration` skill: "Checkpoints belong to the orchestrator"). A "raise to X" or "wait for approval" instruction only works literally in a face-to-face session (`session` skill) — otherwise something else (the orchestrator? a record on disk? re-invocation?) has to carry it across turns.

## Acceptance Criteria

## Edge Cases & Error Scenarios
