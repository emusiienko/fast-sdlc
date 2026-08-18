---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-004: Update orchestration skill to support the config

## What to implement
Project Initiation asks the working-folder / task-management / ADR-location questions and writes the answers to `fast-sdlc-project-setup.md`. Dashboard check reads the new config file instead of `team.md`.

## Scope
`fast-sdlc/fast-sdlc/skills/orchestration/SKILL.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] Project Initiation scenario creates `fast-sdlc-project-setup.md` (not `team.md`) when missing, asking the new questions
- [x] Dashboard check reads `fast-sdlc-project-setup.md`

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-18
**What was done**: Grew from the literal dynamic-paths ask into a full redesign of `orchestration.md`'s coordination model, driven by direct follow-up conversation after the `sdlc` skill itself was redesigned around Tasks instead of story-status flags (see that work's own commit). Highlights:

- **Dynamic paths** (original ask): `docs/team.md` retired — Team composition now read from `fast-sdlc-project-setup.md`; Stories/Tasks/Bugs access goes through the configured Task Management mechanism; own state moved to the orchestrator's own Team folder; session briefings go through the configured Sessions mechanism instead of a hardcoded `docs/sessions/` path
- **Universal Task-coordination principle**: every role's work the orchestrator coordinates is now represented as a Task; face-to-face is just one of two ways to fulfill a Task (the other being an inline spawn), not a separate path around the Task system
- **Scenarios 2 and 3 merged** into "Plan New Work": a single-story scope creates the story directly with business-analyst/architect planning Tasks inside it; a multi-story scope wraps the same two Tasks in a chore "Plan next scope" story — replacing two separate, overlapping scenarios
- **Lead-developer planning split into two Tasks** ("produce the Implementation Plan" → user Checkpoint → "break the approved Implementation Plan into Tasks"), matching lead-developer's own existing two-mode split (Plan Implementation / Create Tasks) instead of glossing over it as one Task
- **New Acceptance phase**: orchestrator opens the Pull Request once a story is Accept-eligible, dispatches PR feedback to the responsible role via a Task, and only the user's explicit merge advances the story
- **Reconcile rewritten**: Task+`Dependencies`-driven (architect → business-analyst sequential, QA parallel), ending in orchestrator opening a second PR and setting the story `Done`
- **Stabilization loop resumability fix**: the next QA-role Task is now created *before* routing a bug fix, not after — so an interruption mid-fix always leaves an open Task to resume from, instead of a state where nothing signals the round isn't finished
- **Style rules formalized**: added the "vacuous negative" anti-pattern rule (established informally throughout this session) and a rule for translating the user's own "not" statements into positive instructions rather than copying them verbatim
- Full downstream consistency pass across every agent/skill this triggered: `sdlc.md` (Requirements no longer mandates active pruning, archival moved to orchestrator not business-analyst), `business-analyst.md`/`architect.md`/`qa-automation.md`/`manual-qa.md` Reconcile modes rewritten off flags onto Tasks, `file-based-task-management.md`'s Stories index table lost all 6 flag columns, `qa-automation.md`'s Bug Numbering deferred to the Task Management skill (was contradicting it)

**Deviations from plan**: Substantial — same pattern as every other task in this story. The trigger was a direct, iterative design conversation about making QA/Reconcile task-driven, which cascaded into `sdlc.md`'s core model and then required this file to catch up, plus a full consistency sweep across the rest of the plugin.

**Technical decisions made**: See above; most consequential is the universal Task-coordination principle — it's what makes every other change (merged scenarios, two-Task lead-developer planning, Task-driven QA/Reconcile) fit into one coherent model instead of a pile of special cases.

**Code locations**: `fast-sdlc/skills/orchestration/SKILL.md`, plus the downstream files listed above.

**Technical debt noted**: None remaining from this pass — a dedicated comprehensive consistency check found no further contradictions as of this task's closure.
