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

### product-strategist
**folder**: `<Configs Folder>/product-strategist/`

### spec-writer
**folder**: `<Configs Folder>/spec-writer/`

### architect
**folder**: `<Configs Folder>/architect/`

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

## Artifacts

<!--
One ### chapter per sdlc-defined artifact, free form. An artifact isn't just a
path — say however a reading agent actually needs to access it: which file is
current vs. history, a folder read via a specific skill, a wiki/external
system and how to query it, etc.
-->

### Validated Business Idea
**location**: `<path>`

### Requirements
**location**: `<path>`

### Architecture
**location**: `<path>`

### API Contract
**location**: `<path>`

### Technical Roadmap
**location**: `<path>`
