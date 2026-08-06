---
status: Done
assignee: orchestrator
story: STORY-002
---

# TASK-002: Create fast-sdlc-business-analyst agent

## What to implement
New agent file merging the content of `fast-sdlc-product-strategist.md` and `fast-sdlc-spec-writer.md` into the two modes designed in TASK-001.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-business-analyst.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] Agent frontmatter (`name`, `description`, examples) reflects both modes
- [x] Discovery Mode content ported from `fast-sdlc-product-strategist`
- [x] Spec Mode content ported from `fast-sdlc-spec-writer`, minus anything dropped in TASK-001

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-06
**What was done**: Created `fast-sdlc-business-analyst.md`, merging both source agents into three modes (New Project, New Scope, Reconcile) with Discovery→Spec as phases, not hand-offs. Tools trimmed to only what a non-technical, non-coding role needs. Ownership/Before Starting/Step 0 written against the dynamic config model (`fast-sdlc-project-setup.md`, `${CLAUDE_PLUGIN_ROOT}` template paths) rather than hardcoded paths — this agent is the first one built natively on STORY-001's model. Discovery redesigned around information-completeness (understand → validate → draft → self-review → challenge-until-clean) instead of a fixed "5 questions, 2 rounds" quota. Full consistency pass completed (7 issues found and fixed) after the redesign settled.
**Deviations from plan**: Extensive iteration beyond a straight port — territory/out-of-territory reframed as named categories, a validation step (scope sizing, competitor/alternatives research, risk generation) inserted between Discovery and Draft, Requirements format moved out of the agent into shipped template files, a Requirements/Documentation split introduced mid-task and threaded through every mode.
**Technical decisions made**: Default format templates ship as separate files under `templates/business-analyst/` (referenced via `${CLAUDE_PLUGIN_ROOT}`), not inlined in the agent — keeps the agent file the process definition, not the content template.
**Code locations**: `fast-sdlc/fast-sdlc/agents/fast-sdlc-business-analyst.md`, `fast-sdlc/fast-sdlc/templates/business-analyst/validated-idea-format.md`, `fast-sdlc/fast-sdlc/templates/business-analyst/requirements-format.md`
**Technical debt noted**: `fast-sdlc-product-strategist.md` and `fast-sdlc-spec-writer.md` not yet deleted — that's TASK-003.
