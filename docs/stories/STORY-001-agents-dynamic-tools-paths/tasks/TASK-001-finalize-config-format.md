---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-001: Finalize fast-sdlc-project-setup.md format

## What to implement
Finalize the structure of `fast-sdlc-project-setup.md` beyond the current draft skeleton (Team, Working Folder, Task Management, ADR Location). Decide field-level detail per section: how "Working Folder" expresses a path that may be outside the repo or gitignored, how "Task Management" distinguishes file-based vs. an external tool (e.g. the AI Agent Coordination Utility via MCP), and how "ADR Location" expresses a path or a shared multi-repo location.

## Scope
`fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md`

## Dependencies
None

## Acceptance Criteria
- [x] Each of the four sections has a defined, unambiguous field structure
- [x] Default values are specified for every field (what applies when the user doesn't override)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-04
**What was done**: Finalized `fast-sdlc-project-setup.md` through iterative discussion. Final structure: **Team** (one `### <role>` chapter per active role, each with a `**folder**` pointing to that role's own `agent.md`, built separately per role in later tasks); **Configs Folder** (default base for role folders, default `docs/`); **Task Management** (file-based default, or external tool + connection details); **Artifacts** (one `### <name>` chapter per sdlc-defined, cross-role-consumed artifact — Validated Idea, Requirements, Architecture, API Contract, Technical Roadmap — each with a `**location**` field plus free-form instructions for non-trivial access, e.g. current-vs-history files or a wiki/skill-based read path).
**Deviations from plan**: Original AC scope (Working Folder / ADR Location as flat fields) was superseded during design. ADRs were dropped from Artifacts entirely — confirmed to be a project-level artifact, not consumed cross-role by any sdlc agent. Stories/Tasks were kept out of Artifacts and left under Task Management, since task management is a behavioral/mechanism choice (file edits vs. external tool calls), not just a doc location. Role-specific References (conventions/strategy docs) and Outputs (role-produced deliverables) were moved out of this file entirely into each role's own `agent.md`, to avoid every agent loading every other role's settings.
**Technical decisions made**: Artifacts list restricted to artifacts actually read cross-role, verified by a full pass over `sdlc/SKILL.md` (Pipeline, Common Workflows, Document Ownership) rather than the full "Key File Formats" catalog. `CLAUDE.md` excluded from Artifacts despite being read by all — its location is fixed by the Claude Code platform, not configurable via `sdlc`.
**Code locations**: `fast-sdlc/fast-sdlc/templates/fast-sdlc-project-setup.md`
**Technical debt noted**: Per-role `agent.md` files (referenced from Team > folder) don't exist yet — built task by task in later tasks of this story.
