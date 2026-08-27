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
**folder**: `<Configs Folder>/lead-developer/` — single-stack default. If this repo has more than one stack, replace this section with one per stack instead, named `### lead-developer-<stack>`, each with its own **folder**: `<Configs Folder>/lead-developer-<stack>/`

### coder
**folder**: `<Configs Folder>/coder/` — single-stack default. If this repo has more than one stack, replace this section with one per stack instead, named `### coder-<stack>`, each with its own **folder**: `<Configs Folder>/coder-<stack>/`

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

## Sessions

**mechanism**: `file-based-session-management` (default skill) | <name of an alternative skill, or an MCP server/other mechanism>
**location**: `docs/sessions/` (only applicable for file-based mechanisms)

---

## Pull Requests

**strategy**: `to-development` (default) | `to-feature-branch` | custom
- `to-development`: Acceptance and Reconcile Pull Requests target the project's development branch directly (see DevOps Strategy > Branch Strategy)
- `to-feature-branch`: the user has already created an overarching feature branch for this scope; orchestrator creates one branch per phase (implementation, Reconcile) off it, and both Pull Requests target that feature branch instead of development
- custom: `<describe>`

---

## Escalation/Feedback

**mechanism**: `file-based-escalation` (default) | <name of an alternative skill, or an MCP server/other mechanism>
**location**: `docs/escalations/` (only applicable for file-based mechanisms)

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

### Business Documentation
**location**: `<path>`
**format**: `<Configs Folder>/business-analyst/business-documentation-format.md` (proposal, not a confirmed default — walk the user through it and confirm/adjust with business-analyst)

### Technical Documentation
**location**: `<path>` (a folder)

### Architectural Design
**location**: `<path>` (a folder)

### Story Implementation Plan
**location**: `<path>` (per story, produced only when needed)

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
