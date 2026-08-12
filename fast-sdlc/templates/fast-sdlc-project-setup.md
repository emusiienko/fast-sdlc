<!--
Template — filled during Project Initiation.
Default location: <repo root>/fast-sdlc-project-setup.md, unless CLAUDE.md's "### Fast SDLC" section points elsewhere.

This file is the single source of truth for the project: the team and where
each member's own config lives, where task management happens, and where each
sdlc-defined artifact actually lives (Artifacts). Each role's own `agent.md`
(References/Outputs, conventions, etc.) lives in its own folder — built
separately, task by task.
-->

# Project Setup

---

## Team

### orchestrator
**folder**: `<Configs Folder>/orchestrator/`

### business-analyst
**folder**: `<Configs Folder>/business-analyst/`

### architect
**folder**: `<Configs Folder>/architect/`

### lead-developer
**folder**: `<Configs Folder>/lead-developer/`

### backend-developer
**folder**: `<Configs Folder>/backend-developer/`

### frontend-developer
**folder**: `<Configs Folder>/frontend-developer/`

### devops
**folder**: `<Configs Folder>/devops/`

### qa-automation
**folder**: `<Configs Folder>/qa-automation/`

### manual-qa
**folder**: `<Configs Folder>/manual-qa/`

---

## Configs Folder

`docs/`

---

## Task Management

**skill**: `file-based-task-management` (default) | <name of an alternative Task Management skill>
**location**: `docs/stories/`

---

## Escalation/Feedback

**mechanism**: TBD — how one role raises something to another mid-session (e.g. architect ↔ lead-developer Deviation Checks) is not yet defined.

---

## Artifacts

<!--
One ### chapter per sdlc-defined artifact, free form. An artifact isn't just a
path — say however a reading agent actually needs to access it: which file is
current vs. history, a folder read via a specific skill, a wiki/external
system and how to query it, etc.
-->

### Validated Business Idea
**location**: `<path>`
**versioning**: `file-based-versioning` (default) | <name of an alternative versioning skill> | none
**format**: `<Configs Folder>/business-analyst/validated-idea-format.md` (default — accept as-is or customize with business-analyst)

### Requirements
**location**: `<path>`
**format**: `<Configs Folder>/business-analyst/requirements-format.md` (proposal, not a confirmed default — walk the user through it and confirm/adjust with business-analyst)

### Documentation
**location**: `<path>`

### Architecture
**location**: `<path>`

### API Contract
**location**: `<path>`

### Technical Roadmap
**location**: `<path>`

### Coding Conventions
**location**:
- `<stack>`: `<path>` (one row per stack; a single `<path>` is enough if this repo has only one)

### Implementation Plan
**location**: depends on the Task Management mechanism in use —
- file-based Task Management (default): inside the story's own folder, e.g. `docs/stories/STORY-NNN/implementation_plan.md`
- app/service-based Task Management: attached to the story object in that system
<confirm which applies with lead-developer during Setup>

### Repo-level ADRs
**location**: `<path>` (one series per stack if this repo has more than one)
