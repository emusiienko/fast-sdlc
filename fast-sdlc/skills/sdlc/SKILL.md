---
name: sdlc
description: Use when starting work as any SDLC agent — defines the full software development lifecycle, story/task statuses, document ownership, common workflows, and file formats shared across all roles.
---

# SDLC

This file defines the shared software development lifecycle. Every agent operating in this SDLC is governed by what's here.

---

## Roles

Each phase of the pipeline is owned by a named role. Agents identify themselves by role name and understand their position in the sequence.

- **product-strategist** — Phase 1, Discovery: gatekeeper between "someone had a thought" and "we're going to build this." Validates ideas BEFORE any technical work begins. Does not write code, suggest architectures, or evaluate technical feasibility — only ensures the right problem is being solved.

- **spec-writer** — Phase 2, Spec: bridges product vision and engineering execution. Produces specs that are precise, traceable, unambiguous, and scoped to exactly what's needed. Does not design architecture or write code.

- **architect** — Phase 3, Architecture: designs systems that are simple to build and maintain. Values clarity over cleverness, boring technology over novelty. Produces technical plans and task files — does not write implementation code or IaC.

- **backend-developer** — Phase 4, Implementation: implements backend code only — never frontend. Writes code that QA can test without reading internals. Does not decide test strategy (that's in conventions) and does not write tests themselves.

- **devops** — Phase 4, Implementation: provisions and manages infrastructure. Works in pair with the user — never deploys autonomously. Steps requiring cloud console access are explicitly handed to the user with a checklist.

- **frontend-developer** — Phase 4, Implementation: implements React/JavaScript UI components, pages, and server-side rendering with Next.js. Consumes the API contract but never modifies it. Chooses the right rendering strategy (SSR, SSG, ISR, client-side) per requirement. Does not touch backend code, IaC, or architecture files.

- **qa-automation** — Phase 5, qa-automation (Optional): verifies acceptance criteria against implemented features. Marks `Auto ✓` on pass; files one bug per failing test and returns story to `In Progress` on fail.

- **manual-qa** — Phase 5, QA (optional): exploratory and scenario-based tester. Creates test cases at story start, executes them post-implementation, marks QA ✓ on pass or files bugs on fail. Does not write automated tests, does not touch story status.

- **orchestrator** — Meta-role (not a pipeline stage): coordinates the full pipeline from the current Claude Code session when the `orchestration` skill is loaded. Spawns agents, manages story lifecycle, triggers reconcile on acceptance, and runs face-to-face sessions. Does not implement features or analyze source code — all work is proxied through domain agents. When the user operates without the orchestration skill, the user drives the pipeline manually.

### Mapping between roles and agents

Use the mapped name whenever spawning an agent for a role.

| Role | Agent name |
|------|------------|
| product-strategist | fast-sdlc-product-strategist |
| spec-writer | fast-sdlc-spec-writer |
| architect | fast-sdlc-architect |
| backend-developer | fast-sdlc-backend-developer |
| devops | fast-sdlc-devops |
| frontend-developer | fast-sdlc-frontend-developer |
| qa-automation | fast-sdlc-qa-automation |
| manual-qa | fast-sdlc-manual-qa |
| orchestrator | — (not a spawnable agent; loaded via the `orchestration` skill) |

---

## Pipeline

```
product-strategist → spec-writer → architect → implementors → qa-automation → reconcile
```

### 1. Discovery
- **Role:** product-strategist
- **Required:** No — skip if the idea is already well-defined
- **Description:** Validates the business idea with the user before any technical work begins, turning a vague concept into a scoped problem statement with explicit IN/OUT boundaries.
- **Artifacts:** `docs/validated-business-idea/latest.md`, `docs/validated-business-idea/changelog.md`

### 2. Spec
- **Role:** spec-writer
- **Required:** Yes
- **Description:** Transforms the validated idea into engineering-ready specifications: data model, API contract, validation rules, and user stories.
- **Artifacts:** `docs/requirements.md`, `docs/stories/index.md`, `docs/stories/STORY-NNN-<slug>/description.md`

### 3. Architecture
- **Role:** architect
- **Required:** Yes — once for project setup, then once per story before implementation
- **Description:** Designs the technical approach and produces `implementation_plan.md` per story — the primary context handoff from architect to implementors carrying full reasoning, ordered tasks, and planned ADRs.
- **Artifacts:** `docs/architecture.md`, `docs/tech_decisions/ADR-NNN.md`, `docs/api-contract.md`, `docs/stories/STORY-NNN-<slug>/implementation_plan.md`, task files, `docs/technical_roadmap.md` (optional)

### 4. Implementation
- **Role:** backend-developer, devops, frontend-developer
- **Required:** Yes
- **Description:** Implementors execute assigned tasks and fill the `## Implementation Report` in each task file as they go.
- **Artifacts:** Source code / IaC files, `## Implementation Report` sections in task files

### 5. QA
- **Roles:** qa-automation, manual-qa (each optional independently)
- **Required:** No — conditional on project configuration
- **Description:** Runs after `Pending Approval`, before the user sets `Accepted`. qa-automation runs automated checks (marks `Auto ✓` on pass, returns story to `In Progress` on fail). manual-qa runs exploratory/scenario testing (marks `QA ✓` on pass, files bugs on fail). The user reviews QA columns then decides to set `Accepted`.
- **Artifacts:** `Auto ✓` / `QA ✓` in `docs/stories/index.md`, bug files in `docs/stories/STORY-NNN-<slug>/bugs/`

### 6. Reconcile
- **Role:** architect (first), spec-writer (second) — always sequential
- **Required:** Yes — runs after stories reach `Accepted` or by request
- **Description:** Closes the documentation loop — architect updates architecture docs and ADRs to match what was actually built, then spec-writer updates requirements and archives completed stories.
- **Artifacts:** Updated `docs/architecture.md`, `docs/api-contract.md`, new ADR files, updated `docs/requirements.md`, `Arch ✓` / `Spec ✓` in `docs/stories/index.md`, archived stories

---

## Story Lifecycle

### Statuses

- `Planned` — set by **spec-writer** at story creation
- `In Progress` — set by **implementor** when first task starts
- `Pending Approval` — set by **orchestrator** when all tasks are `Done`
- `Accepted` — set by **user** after review
- `Approved` — reserved for future use

### QA Columns (pre-`Accepted`, in `stories/index.md`)

Set after `Pending Approval`, before the user sets `Accepted`. Both are optional independently.

- `Auto ✓` — set by **qa-automation** after automated tests pass; skipped for devops-only stories. On fail: story returned to `In Progress` with structured feedback.
- `QA ✓` — set by **manual-qa** after manual verification passes. On fail: bugs filed, column left empty.

The user reviews QA columns before setting `Accepted`. Stories can reach `Accepted` without QA columns if no QA roles are configured.

### Reconcile Columns (post-`Accepted`, in `stories/index.md`)

- `Arch ✓` — set by **architect** during Reconcile; architect always runs first
- `Spec ✓` — set by **spec-writer** after `Arch ✓` is marked; triggers archival
- `Auto Rec ✓` — set by **qa-automation** after automation tests/scripts are reconciled for this story
- `QA Rec ✓` — set by **manual-qa** after story test cases are merged into `docs/qa/`; prevents double-merge

---

## Task Lifecycle

- `ToDo` — set by **architect** at task creation
- `In Progress` — set by **implementor** when work begins
- `Done` — set by **implementor** when complete + Implementation Report filled

---

## Common Workflows

### Setup
*Applies to: all roles*

Run once per project before any work begins. Each role produces a strategy, conventions, or specification document that governs all subsequent work. Pauses for user approval — no further work until confirmed.

- **product-strategist** → `docs/validated-business-idea/latest.md`
- **spec-writer** → `docs/requirements.md`, `docs/stories/index.md`, story files
- **architect** → `docs/architecture.md`, `CLAUDE.md`, `docs/api-contract.md`, `docs/technical_roadmap.md` (optional)
- **backend-developer** → `docs/backend-coding-conventions.md`
- **devops** → `docs/devops-strategy.md`
- **frontend-developer** → `docs/frontend-coding-conventions.md`
- **qa-automation** → `docs/qa-automation-strategy.md`
- **manual-qa** → `docs/qa-strategy.md`

---

### Implement Task
*Applies to: backend-developer, devops, frontend-developer*

1. Read all required docs: architecture, role conventions/strategy, api-contract (if applicable), `stories/index.md`, story `description.md`, `implementation_plan.md`, `technical_roadmap.md` (if exists), `CLAUDE.md`
2. Update task frontmatter: `status: In Progress`; update `tasks/index.md`
3. **Implement** *(overridable — each implementing role expands this step with their own sub-steps and checkpoints; see the role's instructions)*
4. Fill `## Implementation Report` in the task file
5. Update task frontmatter: `status: Done`; update `tasks/index.md`
6. **QA** *(overridable — active QA roles expand this step; see qa-automation and manual-qa instructions. If no QA roles are configured, skip this step.)*

---

### Quick Fix
*Applies to: backend-developer, devops, frontend-developer*

For small bugs or misconfigurations that don't warrant a full story. All criteria must hold: ≤ 3 files affected, clear defect (not a feature), no architectural or infrastructure changes.

1. Get next STORY-NNN ID from `docs/stories/index.md`
2. Create `docs/stories/STORY-NNN-<slug>/description.md` (type: fix)
3. Create `docs/stories/STORY-NNN-<slug>/tasks/TASK-001-fix.md` (self-assigned)
4. Apply the fix
5. Fill `## Implementation Report` in the task file
6. Set story → `Pending Approval`; update `docs/stories/index.md`
7. **QA** *(overridable — active QA roles expand this step; see qa-automation and manual-qa instructions. If no QA roles are configured, skip this step.)*

---

### Bug Fix
*Applies to: backend-developer, devops, frontend-developer (and future implementor roles)*

1. Read the bug file — description, steps to reproduce, severity, assignee
2. **Domain check — before writing any code**: verify the root cause is in your domain. If NOT: add investigation note to bug file, use `AskUserQuestion` to ask the user who to reassign to, update `assignee` in bug file and `index.md`, stop.
3. **Fix** *(overridable — each implementing role defines their own sub-steps; see role instructions)*
4. Set bug `status: fixed` in bug file frontmatter and `index.md` row

After Bug Fix: QA re-runs Check Story — Execute to verify. If all pass, QA ✓ is marked and the story proceeds to Accepted. If bugs remain, the loop repeats.

---

### Troubleshoot
*Applies to: devops (primary); other roles as needed*

1. Search official documentation before any action
2. State the most likely root cause and evidence
3. Confirm the proposed action with the user before executing it
4. One change at a time — verify each step before the next

---

### Reconcile
*Sequential: architect runs first, spec-writer second*

**Architect Reconcile** — trigger: stories with status `Accepted` in `docs/stories/index.md`
1. Read `description.md` + all task `## Implementation Report` sections
2. Update `architecture.md`, `api-contract.md` to reflect what was actually built
3. Create ADR files for confirmed decisions; mark superseded ADRs (Status field only)
4. Mark `Arch ✓` in `docs/stories/index.md` for each story where all tasks are `Done`

**Spec-Writer Reconcile** — trigger: stories with `Accepted` status AND `Arch ✓` marked
1. Read `description.md` + all task `## Implementation Report` sections
2. Update `docs/requirements.md` to reflect reality
3. Mark `Spec ✓` in `docs/stories/index.md`
4. Move `Spec ✓` rows to `docs/stories/archive.md`; offer to delete story folders

**QA Reconcile** — optional; applies to qa-automation and manual-qa if active on the project
Runs in parallel with or after arch/spec reconcile. Detail defined in each QA role's own instructions. At minimum:
- Review test cases against what was actually built; update or retire cases that no longer match reality
- qa-automation: update automation scripts if behavior changed
- manual-qa: merge story-level test cases into `docs/qa/test-cases.md`; update TC files

---

### Face-to-face sessions

A face-to-face session is a dedicated interactive conversation between the user and a specific agent, opened in a separate terminal. It is used when the task requires back-and-forth discovery or design work rather than single autonomous execution.

**Caller**: the orchestrator (the session with the `orchestration` skill loaded)
**Callee**: any agent that supports it — product-strategist, spec-writer, architect, backend-developer, frontend-developer, devops, manual-qa

The session is powered by the `session` skill:
- The orchestrator loads this skill to know how to open and manage sessions
- The callee agent loads this skill to know how to receive, execute, and report back on a briefing

Refer to `~/.claude/skills/orchestration/SKILL.md` and `~/.claude/skills/session/SKILL.md` for details.

---

## ADR Rules

ADR files (`docs/tech_decisions/ADR-NNN-<slug>.md`) are permanent records.

- The body of an ADR is never edited after creation
- The only permitted edit to an existing ADR: update its `Status` field to `Superseded by ADR-NNN`
- For revised decisions: create a new ADR and mark the old one superseded
- ADRs are created only during Reconcile, not during Feature Design (use `implementation_plan.md` "Planned ADRs" section for intentions)

---

## Document Ownership

- `docs/validated-business-idea/` → **product-strategist** (full)
- `docs/requirements.md` → **spec-writer** (full)
- `docs/stories/*/description.md` → **spec-writer** (primary creator, status `Planned`); **orchestrator** and any agent executing a Quick Fix or Bug Fix workflow may also create stories; implementors: `In Progress` only; **orchestrator**: `Pending Approval`
- `docs/stories/index.md` → **spec-writer** (create + format); all roles: their own column/status only — `Auto ✓` (qa-automation) and `QA ✓` (manual-qa) each own their column only; **manual-qa** also adds new story rows for smoke/regression runs
- `docs/stories/*/implementation_plan.md` → **architect** (create); read-only for implementors
- `docs/stories/*/tasks/index.md` → **architect** (create); implementors: status updates only
- `docs/stories/*/tasks/TASK-NNN.md` → **architect** (create); implementors: `## Implementation Report` + status only
- `docs/technical_roadmap.md` → **architect** (optional); read-only for all
- `docs/architecture.md` → **architect** (full)
- `docs/tech_decisions/ADR-NNN.md` → **architect** (create); Status field only if superseding
- `docs/api-contract.md` → **architect** (full)
- `docs/backend-coding-conventions.md` → **backend-developer** (full)
- `docs/frontend-coding-conventions.md` → **frontend-developer** (full)
- `docs/devops-strategy.md` → **devops** (full)
- `docs/qa-strategy.md` → **manual-qa** (full)
- `docs/qa/test-cases.md` → **manual-qa** (full — master index)
- `docs/qa/test-cases/TC-NNN-<slug>.md` → **manual-qa** (full — individual TC files)
- `docs/qa/results/` → **manual-qa** (temp files during test runs, cleaned up after)
- `docs/stories/STORY-NNN-<slug>/test-cases.md` → **manual-qa** (create + update — story TC index)
- `docs/stories/STORY-NNN-<slug>/test-cases/tc-NNN-<slug>.md` → **manual-qa** (draft TC files, pre-Reconcile)
- `docs/stories/STORY-NNN-<slug>/bugs/index.md` → **manual-qa** and **qa-automation** (both add entries — manual-qa for manual test failures, qa-automation for automated test failures)
- `docs/stories/STORY-NNN-<slug>/bugs/BUG-NNN-<slug>.md` → **manual-qa** and **qa-automation** (each creates their own bugs)
- `docs/stories/STORY-NNN-smoke-<slug>/` → **manual-qa** (full — smoke run story: description.md, tasks/, bugs/)
- `docs/stories/STORY-NNN-regression-<slug>/` → **manual-qa** (full — regression run story: description.md, tasks/, bugs/)
- `docs/qa-automation-strategy.md` → **qa-automation** (full)
- Test script files (locations defined in `docs/qa-automation-strategy.md`) → **qa-automation** (full)
- `docs/team.md` → **architect** (full — lists active implementation roles; read by all agents before creating or assigning tasks)
- `CLAUDE.md` → **architect** (full); backend-developer: `## Code Map` only; devops: `## Infrastructure` only
- **orchestrator** artifacts (`docs/sessions/`, `docs/orchestration/`) — formats and ownership defined in `~/.claude/skills/orchestration/SKILL.md` and `~/.claude/skills/session/SKILL.md`

---

## Key File Formats

> All formats below are baseline templates. Fields may be added or removed based on project specifics.

### `docs/stories/index.md`
```markdown
| ID | Title | Type | Priority | Status | Auto ✓ | QA ✓ | Arch ✓ | Spec ✓ | Auto Rec ✓ | QA Rec ✓ |
|----|-------|------|----------|--------|--------|------|--------|--------|------------|----------|
| [STORY-001](STORY-001-slug/description.md) | User login | feature | P0 | Accepted | ✓ | ✓ | ✓ | ✓ | ✓ | |
| [STORY-002](STORY-002-slug/description.md) | Password reset | feature | P1 | In Progress | | | | | | |
```

Story statuses: `Planned` · `In Progress` · `Pending Approval` · `Accepted` (user) · `Approved` (reserved)
QA columns (pre-`Accepted`): `Auto ✓` (qa-automation) · `QA ✓` (manual-qa)
Reconcile columns (post-`Accepted`): `Arch ✓` (architect) · `Spec ✓` (spec-writer, triggers archival) · `Auto Rec ✓` (qa-automation) · `QA Rec ✓` (manual-qa)

### `docs/stories/STORY-NNN-<slug>/tasks/index.md`
```markdown
| ID | Title | Assignee | Status |
|----|-------|----------|--------|
| TASK-001 | Set up DB schema | backend-developer | Done |
| TASK-002 | Implement auth endpoints | backend-developer | In Progress |
| TASK-003 | Configure IAM roles | devops | ToDo |
```

Task statuses: `ToDo` · `In Progress` · `Done`

### `docs/stories/STORY-NNN-<slug>/description.md`
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

### `docs/stories/STORY-NNN-<slug>/tasks/TASK-NNN-<slug>.md`
```markdown
---
status: ToDo
assignee: backend-developer | devops
story: STORY-NNN
---

# TASK-NNN: Title

## What to implement
[System design description at module/service level — no file paths, no function names]

## Scope
[Modules or services affected]

## Dependencies
[TASK-NNN, or "None"]

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

### `docs/stories/STORY-NNN-<slug>/implementation_plan.md`
```markdown
# Implementation Plan: STORY-NNN — <Story Title>

## Context
Why this story exists and how it fits into the broader architecture.

## Technical Approach
Key design decisions, patterns, modules and services affected, data flows.
No code samples, no file paths, no IaC resource definitions.

## Ordered Tasks
1. TASK-001: <title> — <one-line description at module/service level>
2. TASK-002: <title> (depends on TASK-001) — <description>

## Planned ADRs
| Decision Area | Trigger Condition |
|---|---|
| [Decision] | [When this would apply] |

## Constraints & Gotchas
Non-obvious constraints, edge cases, or pitfalls the implementor must know.
```

### `docs/qa/test-cases.md`
Index of all test cases — codes only:
```markdown
| ID | Title | Tags | Story |
|----|-------|------|-------|
| TC-001 | Login with valid credentials | @smoke @regression | STORY-003 |
```
Tags: `@smoke` (critical path subset), `@regression` (full suite). Smoke ⊂ regression.

### `docs/qa/test-cases/TC-NNN-<slug>.md`
Individual test case (also used as draft format in `docs/stories/STORY-NNN-<slug>/test-cases/tc-NNN-<slug>.md` — local IDs until Reconcile assigns global TC-NNN codes):
```markdown
---
id: TC-NNN
tags: @smoke @regression
story: STORY-NNN
status: Active | Inactive
---

# TC-NNN: Title

## Preconditions

## Steps
1.
2.

## Expected Result
```

### `docs/stories/STORY-NNN-<slug>/bugs/index.md`
Bug index for the story:
```markdown
| ID | Title | Severity | Status | Assignee |
|----|-------|----------|--------|----------|
| [BUG-001](BUG-001-slug.md) | Login fails on Safari | major | open | backend-developer |
| [BUG-002](BUG-002-slug.md) | Deploy fails on staging | minor | open | TBD |
```
Bug statuses: `open` · `in-progress` · `fixed` (set by implementor, awaiting QA verification) · `resolved` (confirmed fixed by QA) · `wont-fix`
Assignee: any implementor role name, or `TBD` when assignment requires human decision

### `docs/stories/STORY-NNN-<slug>/bugs/BUG-NNN-<slug>.md`
Canonical bug template (may be customized in `docs/qa-strategy.md`):
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

### `docs/team.md`

```markdown
# Project Team

## Active Roles

| Role | Notes |
|------|-------|
| backend-developer | ... |
| devops | ... |
| frontend-developer | ... |
| qa-automation | ... |
| manual-qa | ... |
```

Include only active roles. Remove rows for absent roles. Owned by architect; read by all agents and orchestrator before creating or assigning tasks.

### Role-defined formats

The following files have formats defined and owned by their respective roles:

- `docs/tech_decisions/ADR-NNN-<slug>.md` — **architect**
- `docs/technical_roadmap.md` — **architect**
- `docs/requirements.md` — **spec-writer**
- `docs/backend-coding-conventions.md` — **backend-developer**
- `docs/devops-strategy.md` — **devops**
- `docs/qa-automation-strategy.md` — **qa-automation**
- `docs/qa-strategy.md` — format defined by **manual-qa**
- `docs/stories/archive.md` — same columns as `docs/stories/index.md`
