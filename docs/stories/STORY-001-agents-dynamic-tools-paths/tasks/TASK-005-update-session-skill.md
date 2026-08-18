---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-005: Update session skill to support the config

## What to implement
Audit `session/SKILL.md` for hardcoded doc paths (e.g. `docs/sessions/`). If found, make them config-resolved like the other skills; if none are found, note that explicitly and close the task with no changes.

## Scope
`fast-sdlc/fast-sdlc/skills/session/SKILL.md`

## Dependencies
TASK-001

## Acceptance Criteria
- [x] Audit completed and documented in the Implementation Report
- [x] Any hardcoded path found is made config-resolved

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-17
**What was done**: Audit found one hardcoded path: `docs/sessions/SESSION-NNN-<slug>.md`, appearing 4 times (the CALLEE-detection example, the caller's write step, the example launch command, and the Document ownership line). Per direct user request, this grew into treating **Sessions** as a first-class configurable mechanism, the same way Task Management already is — the user noted they already have multiple session-coordination options in mind (not only file-based), so a fixed "which skill" framing wasn't enough; it needed to be mechanism-agnostic (a skill, an MCP server, etc.):

- **`sdlc` skill > Project Config**: added `Sessions` to the "What's configurable" list and to the Owner line, phrased as "how face-to-face session coordination is actually mechanized (a skill, an MCP server, etc.)" — deliberately not "which skill", unlike Task Management's existing phrasing
- **`fast-sdlc-project-setup.md` template**: added a `## Sessions` chapter mirroring `## Task Management`'s shape (`mechanism` + `location`), defaulting to `file-based-session-management` / `docs/sessions/`
- **New skill `file-based-session-management`**: the default file-based mechanism — defines the session file's shape (frontmatter, Briefing/Report sections) and its Create/Start/Complete/Read-report flows, mirroring `file-based-task-management`'s own split between "concept" (owned by a protocol skill) and "file rendering" (owned by the file-based skill)
- **`session/SKILL.md` rewritten** to be mechanism-agnostic: it now defines only the CALLER/CALLEE protocol (who does what, when) and delegates every read/write of the briefing/report to "the configured Sessions mechanism," resolved via `fast-sdlc-project-setup.md` > Project Config > Sessions. The literal file format moved out entirely — it now lives in `file-based-session-management`.

**Deviations from plan**: Grew from a literal path audit into introducing Sessions as a new Project-Config-driven mechanism — directed explicitly by the user mid-task, same pattern as prior tasks in this story.

**Technical decisions made**: Sessions config uses "mechanism" rather than "skill" as its field name, since a Session Management implementation may not be a Claude Code skill at all (e.g. an MCP-backed system) — this differs from Task Management's current "skill"-only phrasing, which was not touched (out of scope here).

**Code locations**: `fast-sdlc/skills/sdlc/SKILL.md`, `fast-sdlc/skills/session/SKILL.md`, `fast-sdlc/skills/file-based-session-management/SKILL.md` (new), `fast-sdlc/templates/fast-sdlc-project-setup.md`

**Technical debt noted**: `fast-sdlc/skills/orchestration/SKILL.md` still hardcodes `docs/sessions/SESSION-NNN-<slug>.md` in two places (Document Ownership, Agent briefings) — TASK-004 (orchestration skill config support, still ToDo) should update these to resolve through the new Sessions config instead.
