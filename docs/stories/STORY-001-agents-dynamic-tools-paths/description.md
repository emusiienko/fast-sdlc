---
status: Planned
type: feature
priority: TBD
date: 2026-08-04
---

# STORY-001: Make agents dynamic in using tools and paths

## Desired Outcome

Agents and skills in this plugin stop hardcoding where SDLC artifacts live (ADRs, task/story management approach, working folder). These become project-level configuration, decided once during Project Initiation and stored in `fast-sdlc-project-setup.md` — every role reads its file locations from there instead of assuming a fixed path.

## Context

Today `sdlc`/`orchestration`/agent instructions assume fixed paths (e.g. `docs/tech_decisions/` for ADRs, `docs/stories/` for tasks). Some projects may want ADRs or task tracking outside `docs/`, gitignored, in a shared multi-repo location, or backed by an external tool (e.g. the AI Agent Coordination Utility via MCP) instead of files.

`fast-sdlc-project-setup.md` replaces `team.md` as the single SDLC config artifact (team + working folder + task management approach + ADR location). Its location is itself configurable — discoverable via a `### Fast SDLC` section in the target project's `CLAUDE.md` (auto-loaded for both the orchestrator and every subagent); if that section is absent, the default location is `fast-sdlc-project-setup.md` at the repo root.

## Acceptance Criteria

- [ ] `fast-sdlc-project-setup.md` format finalized (currently a draft at `templates/fast-sdlc-project-setup.md`: Team, Working Folder, Task Management, ADR Location)
- [ ] `CLAUDE.md` additions defined: `### Fast SDLC` section format pointing to the config file's actual path
- [ ] `sdlc` skill updated: file-format/ownership sections resolve locations from the config instead of hardcoded paths; `team.md` retired in favor of the new config file
- [ ] `orchestration` skill updated: Project Initiation asks the working-folder/task-management/ADR-location questions and writes them to the config file; dashboard check reads the new config file instead of `team.md`
- [ ] `session` skill updated, if it hardcodes any doc paths
- [ ] Each of the 8 role agents (product-strategist, spec-writer, architect, backend-developer, devops, frontend-developer, qa-automation, manual-qa) updated to resolve its file paths from the config
- [ ] End-to-end test in the target project

## Edge Cases & Error Scenarios

- [ ] No `fast-sdlc-project-setup.md` and no `### Fast SDLC` section in `CLAUDE.md` yet (first adoption / legacy project): architect creates the config during Setup, same as today's `team.md` flow, using the default root path
- [ ] `CLAUDE.md` has a `### Fast SDLC` section but the file it points to is missing or malformed
