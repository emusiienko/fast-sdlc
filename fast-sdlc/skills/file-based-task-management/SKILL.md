---
name: file-based-task-management
description: Implements Task Management (as defined in the sdlc skill) using files and folders — Stories, Tasks, Implementation Reports, and Bugs as markdown files. One possible Task Management mechanism; selected per-project in fast-sdlc-project-setup.md.
---

# File-Based Task Management

Renders the Task Management concept from `sdlc` (statuses, status flags, content rules) as files. A project selects this skill in `fast-sdlc-project-setup.md` > Task Management, which also names the operating folder this skill works in (default `docs/stories/` — referred to below as the operating folder).

Content requirements for each artifact (what must be said, not how) are defined in `sdlc` > Artifact Content Rules. This skill defines only the file/folder shape.

---

## Structure

### Layout

```
<operating folder>/
  index.md
  archive.md
  STORY-NNN-<slug>/
    description.md
    tasks/
      index.md
      TASK-NNN-<slug>.md
    bugs/
      index.md
      BUG-NNN-<slug>.md
```

### Stories index (`index.md`)

```markdown
| ID | Title | Type | Priority | Status | Auto ✓ | QA ✓ | Arch ✓ | Spec ✓ | Auto Rec ✓ | QA Rec ✓ |
|----|-------|------|----------|--------|--------|------|--------|--------|------------|----------|
| [STORY-001](STORY-001-slug/description.md) | User login | feature | P0 | Accepted | ✓ | ✓ | ✓ | ✓ | ✓ | |
| [STORY-002](STORY-002-slug/description.md) | Password reset | feature | P1 | In Progress | | | | | | |
```

Column order is fixed: `Auto ✓` (Automated Tests Passed) · `QA ✓` (Manual QA Passed) · `Arch ✓` (Architecture Reconciled) · `Spec ✓` (Spec Reconciled) · `Auto Rec ✓` (Automation Reconciled) · `QA Rec ✓` (Manual QA Reconciled) — as defined in `sdlc` > Task Management > Story Status Flags. Include only the columns for QA/Reconcile roles active on the project; drop the rest, in the same left-to-right order.

`archive.md` uses the same columns — rows move here once every applicable Reconcile flag is set.

### Story file (`description.md`)

```markdown
---
status: Planned
type: feature | fix | chore
priority: P0/P1/P2
date: YYYY-MM-DD
---

# STORY-NNN: Title

## Desired Outcome
## Context
## Acceptance Criteria
- [ ] ...
## Edge Cases & Error Scenarios
- [ ] ...
```

### Tasks index (`tasks/index.md`)

```markdown
| ID | Title | Assignee | Status |
|----|-------|----------|--------|
| TASK-001 | Set up DB schema | backend-developer | Done |
| TASK-002 | Implement auth endpoints | backend-developer | In Progress |
```

### Task file (`tasks/TASK-NNN-<slug>.md`)

```markdown
---
status: ToDo
assignee: backend-developer | devops
story: STORY-NNN
---

# TASK-NNN: Title

## What to implement
## Scope
## Dependencies
## Acceptance Criteria
- [ ] ...

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**:
**What was done**:
**Deviations from plan**:
**Technical decisions made**:
**Code locations**:
**Technical debt noted**:
```

### Bugs index (`bugs/index.md`)

```markdown
| ID | Title | Severity | Status | Assignee |
|----|-------|----------|--------|----------|
| [BUG-001](BUG-001-slug.md) | Login fails on Safari | major | open | backend-developer |
| [BUG-002](BUG-002-slug.md) | Deploy fails on staging | minor | open | TBD |
```

Assignee: any implementor role name, or `TBD` when assignment requires human decision.

### Bug file (`bugs/BUG-NNN-<slug>.md`)

```markdown
---
id: BUG-NNN
story: STORY-NNN
severity: critical | major | minor | trivial
status: open | in-progress | fixed | resolved | wont-fix
assignee: backend-developer | devops | frontend-developer | TBD
---

# BUG-NNN: Title

## Description
## Steps to Reproduce
1.
2.

## Expected Result
## Actual Result

## Additional Info
<!-- logs, screenshots, environment details -->
```

---

## Flows

### Create a Story
1. Get the next `STORY-NNN` ID — highest existing + 1, from the index
2. Create the story folder with `description.md` (frontmatter: `status: Planned`, `type`, `priority`, `date`; body per `sdlc` > Artifact Content Rules)
3. Add a row to the index: ID, Title, Type, Priority, `Status: Planned`

### Create a Task
1. Get the next `TASK-NNN` ID — highest existing + 1, within the story's own tasks index
2. Create the task file (frontmatter: `status: ToDo`, `assignee`, `story`; body per `sdlc` > Artifact Content Rules)
3. Add a row to the story's tasks index: ID, Title, Assignee, `Status: ToDo`

### Update Story Status
1. Update `status` in the story's `description.md` frontmatter
2. Update the `Status` column for that story's row in the index

### Update Task Status
1. Update `status` in the task file's frontmatter
2. Update the `Status` column for that task's row in the story's tasks index
3. On `Done`: fill Implementation Report in the same task file first

### Set a Story Status Flag
1. Set `✓` in the corresponding column for that story's row in the index — see Stories index above for the fixed column order

### Create a Bug
1. Get the next `BUG-NNN` ID — highest existing + 1, within the story's own bugs index
2. Create the bug file (frontmatter: `id`, `story`, `severity`, `status: open`, `assignee`; body per `sdlc` > Artifact Content Rules)
3. Add a row to the story's bugs index: ID, Title, Severity, `Status: open`, Assignee

### Update Bug Status
1. Update `status` in the bug file's frontmatter
2. Update the `Status` column for that bug's row in the story's bugs index

### Archive a Story
Trigger: "Spec Reconciled" is set (see `sdlc` > Reconcile).
1. Move the story's row from the index to `archive.md`
2. Offer to delete the story's folder
