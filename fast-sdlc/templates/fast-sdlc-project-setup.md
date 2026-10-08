<!--
Template — filled during Project Initiation.
Default location: <repo root>/fast-sdlc-project-setup.md, unless CLAUDE.md's "### Fast SDLC" section points elsewhere.

This file is the single source of truth for the project: the team and where
each member's own config lives, where task management happens, and where each
sdlc-defined artifact actually lives (Artifacts). Each role's own `agent.md`
(References only — a settled format/location decision with no home above,
nothing else) lives in its own folder — built separately, task by task.

Every default below is stated as a concrete value. Where a value depends on
this repo's own shape (its stacks, its source folders), it stays a placeholder
instead — those are named in each chapter.
-->

# Project Setup

---

## Team

<!--
**working folder(s)** is used only by the orchestrator, to decide where to spawn
each role's agent (inline or face-to-face) — the agent itself is never aware of
this field. It may list more than one path; when it does, the orchestrator picks
whichever is most relevant to the task at hand. orchestrator has no working
folder(s) of its own: it isn't spawned by anything, it's the skill applied over
the session the user already has open, so its root is the project root by
construction.

A role's folder holds that role's own `agent.md` and the formats it has
confirmed for the artifacts it owns.
-->

### orchestrator
**folder**: `<Configs Folder>/agents/orchestrator/`

### business-analyst
**folder**: `<Configs Folder>/agents/business-analyst/`
**working folder(s)**: `.` (project root — default, since Validated Idea/Requirements/Business Documentation are too varied in kind and location to reduce to one path)

### architect
**folder**: `<Configs Folder>/agents/architect/`
**working folder(s)**: `.` (project root — default, since Architecture/ADR Log/Architectural Design are too varied in kind and location to reduce to one path)

### lead-developer
**folder**: `<Configs Folder>/agents/lead-developer/` — single-stack default. If this repo has more than one stack, replace this section with one per stack instead, named `### lead-developer-<stack>`, each with its own **folder**: `<Configs Folder>/agents/lead-developer/<stack>/`
**working folder(s)**: defaults to this stack's Source Code location(s) (Artifacts). Per-stack sections each get their own **working folder(s)** too.

### coder
**folder**: `<Configs Folder>/agents/coder/` — single-stack default. If this repo has more than one stack, replace this section with one per stack instead, named `### coder-<stack>`, each with its own **folder**: `<Configs Folder>/agents/coder/<stack>/`
**working folder(s)**: same default as this stack's `lead-developer` working folder(s). Per-stack sections each get their own **working folder(s)** too.

### devops
**folder**: `<Configs Folder>/agents/devops/`
**working folder(s)**: defaults to the Infrastructure Code location(s) (Artifacts) — one entry per stack/environment if this repo has more than one

### qa-automation
**folder**: `<Configs Folder>/agents/qa-automation/`
**working folder(s)**: defaults to the Automated Test Suite location (Artifacts)

### manual-qa
**folder**: `<Configs Folder>/agents/manual-qa/`
**working folder(s)**: defaults to the Test Case Registry location(s) (Artifacts) — one entry per stack if this repo has more than one

---

## Configs Folder

`.sdlc/`

---

## Task Management

<!--
Two mechanisms are described below. Keep the one this project uses and delete
the other. Statuses, types and content rules are defined in `sdlc` > Task
Management and > Artifact Content Rules either way — what belongs here is only
how this project's mechanism renders them.
-->

### file-based (default)

**skill**: `file-based-task-management` | <name of an alternative Task Management skill>
**location**: `<Configs Folder>/task-management/`

### MCP-based

**mechanism**: MCP server `claude-code-project-management` — `http://localhost:8000/mcp/v1/` (HTTP streamable)
**project**: `<project name>` — id `<project id>`. All work is created under it.

Free-form content (stories, tasks, bugs, reports, escalations) is text. Its sections follow `sdlc` > Artifact Content Rules, unless a format for it is confirmed in Artifacts.

#### Useful tools
These return more information in one call; prefer them to several single-item reads.
- `get_project_work_state` — the project with its Stories
- `get_story_aggregate`, `get_story_detail` — a Story with its related data
- `get_task_detail` — a Task with its related data

#### Statuses
- **Story**: `planned` · `in-progress` · `pending-approval` · `approved` · `accepted` · `done`
- **Task**: `todo` · `in-progress` · `done`
- **Bug** (a Task): a Bug that is `open` is stored as `todo`; its other statuses are `in-progress` · `fixed` · `resolved` · `wont-fix`. `fixed`, `resolved` and `wont-fix` apply to Bugs only

#### Types
Task: `task`. Bug: `bug`.

#### Roles
Role fields (assignee, creator, author, addressee) use the role names from `sdlc`. The user is `operator`, so an escalation to the user is `to=operator`. The allowed role list is managed in the server's project settings and grows with the team.

#### Stories
- Labels: `type` = `feature` | `fix` | `chore`; `priority` = `P0` | `P1` | `P2`
- Stories are not archived: `done` is the final status. A Story is deleted only when the user asks.

#### Tasks
- A Bug is a Task of type `bug`, created under the Story it was found in. Label: `severity` = `critical` | `major` | `minor` | `trivial`. Bug statuses are Task statuses.
- A dependency is a Task link of type `depends-on`, from the dependent Task to the Task it depends on. Record each dependency both in the Task content and as a link. A Task is ready to run when every Task it depends on is `done`.
- An Implementation Report is a comment on the Task, labeled `kind=implementation-report`, posted before the Task is set `done`.

---

## Sessions

<!-- Keep the mechanism this project uses and delete the other. -->

### file-based (default)

**mechanism**: `file-based-session-management` | <name of an alternative skill>
**location**: `<Configs Folder>/sessions/`

### MCP-based

**mechanism**: MCP server `claude-code-project-management` (see Task Management)

- The session id is the briefing reference.
- The briefing's Topics go into the session `context`, after its Context. The report's open questions go into the result summary.
- Status values are the server's own: confirm the exact strings from the first session created, and record them here.

---

## Pull Requests

**strategy**: `to-development` (default) | `to-feature-branch` | custom
- `to-development`: Acceptance and Reconcile Pull Requests target the project's development branch directly (see DevOps Strategy > Branch Strategy)
- `to-feature-branch`: the user has already created an overarching feature branch for this scope; orchestrator creates one branch per phase (implementation, Reconcile) off it, and both Pull Requests target that feature branch instead of development
- custom: `<describe>`

---

## Escalation/Feedback

<!-- Keep the mechanism this project uses and delete the other. -->

### file-based (default)

**mechanism**: `file-based-escalation` | <name of an alternative skill>
**location**: `<Configs Folder>/escalations/`

### MCP-based

**mechanism**: comments on `claude-code-project-management` (see Task Management)

- An escalation is a comment thread on the requester's own Task: the root comment is labeled `kind=escalation`, `to=<role>`, `status=open`; replies point to the root comment. It goes on the Story when it is to business-analyst and affects requirements described in that Story.
- To close one, the requester replies, then sets the root comment's label `status=closed`. Updating labels replaces the whole list, so repeat the others.
- To find escalations addressed to a role, filter the Task's comments by `to=<role>`; open ones have `status=open`. Nothing notifies the addressee; the orchestrator checks the Task's comments after the agent reports that it has finished its work, and routes the open escalations.

---

## Artifacts

<!--
One ### chapter per sdlc-defined artifact, free form. An artifact isn't just a
path — say however a reading agent actually needs to access it: which file is
current vs. history, a folder read via a specific skill, a wiki/external
system and how to query it, etc.

Each default below holds unless the owning role and the user agree otherwise
during Setup.
-->

### Validated Business Idea
**location**: `<Configs Folder>/planning/validated-idea/` (`latest.md` + `changelog.md`, per the versioning mechanism below)
**versioning**: `file-based-versioning` (default) | <name of an alternative versioning skill> | none
**format**: `<Configs Folder>/agents/business-analyst/validated-idea-format.md` (a single document; default — accept as-is or customize with business-analyst)

### Requirements
**location**: `<Configs Folder>/planning/requirements/`
**format**: `<Configs Folder>/agents/business-analyst/` — a multi-file format, so the folder is named rather than one file (proposal, not a confirmed default — walk the user through it and confirm/adjust with business-analyst)

### Business Documentation
**location**: `documentation/business/` (repo root — the business-facing half of the as-built documentation; its technical counterpart is Technical Documentation below)
**format**: `<Configs Folder>/agents/business-analyst/` — a multi-file format, so the folder is named rather than one file (proposal, not a confirmed default — walk the user through it and confirm/adjust with business-analyst)

### Technical Documentation
**location**: `documentation/technical/` (repo root — a folder; the technical/system-facing counterpart of Business Documentation)

### Architectural Design
**location**: `<Configs Folder>/planning/architectural-design/` — one subfolder per scope (a milestone or story group), named after that scope

### Story Implementation Plan
**location**: alongside the story, per the Task Management mechanism in use —
- file-based Task Management (default): inside the story's own folder, e.g. `<Configs Folder>/task-management/STORY-NNN-<slug>/story-implementation-plan.md`
- MCP-based Task Management: attached to the story object in that server

Produced per story, only when needed.

### Coding Conventions
**location**: `<Configs Folder>/conventions/coding-conventions-<stack>.md` (drop the `-<stack>` suffix if this repo has only one stack) — a shared folder rather than the owning role's own config folder, so every role can read it directly

### Implementation Plan
**location**: depends on the Task Management mechanism in use, one per stack if this repo has more than one —
- file-based Task Management (default): inside the story's own folder, e.g. `<Configs Folder>/task-management/STORY-NNN-<slug>/implementation-plan-<stack>.md`
- MCP-based Task Management: attached to the story object in that server, one per stack
<confirm which applies with lead-developer during Setup>

### Story Implementation Deviation Report
**location**: depends on the Task Management mechanism in use, one per stack if this repo has more than one —
- file-based Task Management (default): inside the story's own folder, e.g. `<Configs Folder>/task-management/STORY-NNN-<slug>/deviation-report-<stack>.md` (drop the `-<stack>` suffix if this repo has only one)
- MCP-based Task Management: attached to the story object in that server, one per stack
<confirm which applies with lead-developer during Setup>

### Repo-level ADRs
**location**: `<path>` — inside the stack's own folder (one series per stack if this repo has more than one), since the series belongs to that stack's code, not to the config folder. Depends on this repo's layout, so it has no project-independent default

### Source Code
**location**:
- `<stack>`: `<path>` (one row per stack; a single `<path>` is enough if this repo has only one) — one or more folders; list all that apply for this stack (e.g. `src/`, `migrations/`) rather than assuming a single root

Depends on this repo's layout, so it has no project-independent default.

### Code Map
**location**: `<Configs Folder>/code-maps/<stack>.md` (use `code-map.md` if this repo has only one stack) — a single file per stack, persistent across stories (not per-story). `CLAUDE.md` > Key Docs points here; no length cap of its own

### Automated Test Suite
**location**: `<path>` (a single suite, tested black-box across repos/services — not split per stack) — one or more folders; list all that apply. A runnable artifact, so it lives beside the code it exercises rather than in the config folder; depends on this repo's layout, so it has no project-independent default

### Infrastructure Code
**location**:
- `<stack/environment>`: `<path>` (one row per stack/environment if this repo has more than one; a single `<path>` is enough otherwise) — one or more folders; list all that apply

Depends on this repo's layout, so it has no project-independent default.

### Test Case Registry
**location**:
- `<stack>`: `<path>` (one row per stack; a single `<path>` is enough if this repo has only one)

Depends on this repo's layout, so it has no project-independent default.
