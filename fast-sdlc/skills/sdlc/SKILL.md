---
name: sdlc
description: Use when starting work as any SDLC agent — defines the full software development lifecycle, story/task statuses, artifacts ownership, common workflows, and artifact content rules shared across all roles.
---

# SDLC

This file defines the shared software development lifecycle. Every agent operating in this SDLC is governed by what's here.

---

## Glossary

- **Role** — a named position in the pipeline (e.g. architect). Conceptual — always referred to by name, never by the agent implementing it. Resolved to an agent via Roles > Mapping between roles and agents.
- **Artifact** — a named unit of information the SDLC produces/consumes (e.g. Requirements). Not tied to a file or format — its form and access method are resolved per-project (see Project Config).
- **Task Management** — how work is tracked through the pipeline (stories, tasks, statuses) on a given project. Resolved per-project (see Project Config).

---

## Project Config

This SDLC is fully configurable. Project-specific configuration lives in `fast-sdlc-project-setup.md`.

**Finding it**: check the target project's `CLAUDE.md` for a `### Fast SDLC` section pointing to the config file. If that section is absent, default to `fast-sdlc-project-setup.md` at the repo root.

**What's configurable** (not exhaustive):
- **Team** — active roles, each with a folder where that role's own `agent.md` lives (References: conventions/standards the role follows; Outputs: deliverables the role produces)
- **Configs Folder** — default base for role folders
- **Task Management** — how tasks are tracked and managed on this project
- **Artifacts** — the form each sdlc artifact takes and how it's managed (the notion of artifact is defined next)
- Any other project specifics

---

## Artifacts

- **Validated Idea** — scoped problem statement with explicit IN/OUT boundaries.
- **Requirements** — engineering-ready spec: data model, validation rules, user stories.
- **Architecture** — the technical design.
- **API Contract** — the interface between backend and frontend.
- **Technical Roadmap** (optional) — longer-horizon technical plan.
- **Milestones** — a composition of stories describing a development phase.
- **Stories** — a scoped unit of work and its lifecycle status.
- **Tasks** — a story's breakdown into implementable units, with status.
- **Implementation Report** — record of what was actually built for a task.
- **Bugs** — a defect filed against a story.

---

## Task Management

### Story Statuses

- `Planned` — set by **spec-writer** at story creation
- `In Progress` — set by **implementor** when first task starts
- `Pending Approval` — set by **orchestrator** when all tasks are `Done`
- `Accepted` — set by **user** after review
- `Approved` — reserved for future use

### Story Status Flags

Independent of the statuses above. How each flag is actually represented (a table column, a field in an external tool, ...) depends on how Task Management is resolved for this project (see Project Config).

**Pre-`Accepted`** (each optional independently):
- **Automated Tests Passed** — set by **qa-automation** after automated tests pass; skipped for devops-only stories. On fail: story returned to `In Progress` with structured feedback.
- **Manual QA Passed** — set by **manual-qa** after manual verification passes. On fail: bugs filed, flag left unset.

The user reviews these flags before setting `Accepted`. Stories can reach `Accepted` without them if no QA roles are configured.

**Post-`Accepted`**:
- **Architecture Reconciled** — set by **architect** during Reconcile; architect always runs first
- **Spec Reconciled** — set by **spec-writer** after Architecture Reconciled is set; triggers archival
- **Automation Reconciled** — set by **qa-automation** after automation tests/scripts are reconciled for this story
- **Manual QA Reconciled** — set by **manual-qa** after story test cases are merged; prevents double-merge

### Task Statuses

- `ToDo` — set by **architect** at task creation
- `In Progress` — set by **implementor** when work begins
- `Done` — set by **implementor** when complete + Implementation Report filled

---

## Roles

Each phase of the pipeline is owned by a named role. Agents identify themselves by role name and understand their position in the sequence.

- **product-strategist** — Phase 1, Discovery: gatekeeper between "someone had a thought" and "we're going to build this." Validates ideas BEFORE any technical work begins. Does not write code, suggest architectures, or evaluate technical feasibility — only ensures the right problem is being solved.

- **spec-writer** — Phase 2, Spec: bridges product vision and engineering execution. Produces specs that are precise, traceable, unambiguous, and scoped to exactly what's needed. Does not design architecture or write code.

- **architect** — Phase 3, Architecture: designs systems that are simple to build and maintain. Values clarity over cleverness, boring technology over novelty. Produces technical plans and task files — does not write implementation code or IaC.

- **backend-developer** — Phase 4, Implementation: implements backend code only — never frontend. Writes code that QA can test without reading internals. Does not decide test strategy (that's in conventions) and does not write tests themselves.

- **devops** — Phase 4, Implementation: provisions and manages infrastructure. Works in pair with the user — never deploys autonomously. Steps requiring cloud console access are explicitly handed to the user with a checklist.

- **frontend-developer** — Phase 4, Implementation: implements React/JavaScript UI components, pages, and server-side rendering with Next.js. Consumes the API contract but never modifies it. Chooses the right rendering strategy (SSR, SSG, ISR, client-side) per requirement. Does not touch backend code, IaC, or architecture files.

- **qa-automation** — Phase 5, qa-automation (Optional): verifies acceptance criteria against implemented features. Sets "Automated Tests Passed" on pass; files one bug per failing test and returns story to `In Progress` on fail.

- **manual-qa** — Phase 5, QA (optional): exploratory and scenario-based tester. Creates test cases at story start, executes them post-implementation, sets "Manual QA Passed" on pass or files bugs on fail. Does not write automated tests, does not touch story status.

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
- **Artifacts:** Validated Idea

### 2. Spec
- **Role:** spec-writer
- **Required:** Yes
- **Description:** Transforms the validated idea into engineering-ready specifications: data model, validation rules, and user stories.
- **Artifacts:** Requirements, Milestones, Stories

### 3. Architecture
- **Role:** architect
- **Required:** Yes — once for project setup, then once per story before implementation
- **Description:** Designs the technical approach and produces an implementation plan per story — the primary context handoff from architect to implementors carrying full reasoning, ordered tasks, and planned ADRs.
- **Artifacts:** Architecture, API Contract, Technical Roadmap (optional), Tasks

### 4. Implementation
- **Role:** backend-developer, devops, frontend-developer
- **Required:** Yes
- **Description:** Implementors execute assigned tasks and fill the Implementation Report in each task file as they go.
- **Artifacts:** Implementation Report

### 5. QA
- **Roles:** qa-automation, manual-qa (each optional independently)
- **Required:** No — conditional on project configuration
- **Description:** Runs after `Pending Approval`, before the user sets `Accepted`. qa-automation runs automated checks (sets "Automated Tests Passed" on pass, returns story to `In Progress` on fail). manual-qa runs exploratory/scenario testing (sets "Manual QA Passed" on pass, files bugs on fail). The user reviews these flags then decides to set `Accepted`.
- **Artifacts:** Stories, Bugs

### 6. Reconcile
- **Role:** architect (first), spec-writer (second) — always sequential
- **Required:** Yes — runs after stories reach `Accepted` or by request
- **Description:** Closes the documentation loop — architect updates architecture docs and ADRs to match what was actually built, then spec-writer updates requirements and archives completed stories.
- **Artifacts:** Architecture, API Contract, Requirements, Stories

---

## Common Workflows

### Setup
*Applies to: all roles*

Run once per project before any work begins. Each role produces a strategy, conventions, or specification document that governs all subsequent work. Pauses for user approval — no further work until confirmed.

- **Project Config** → `fast-sdlc-project-setup.md`, established first — defines where everything below actually lives
- **product-strategist** → Validated Idea, own `agent.md` (References)
- **spec-writer** → Requirements, Milestones, Stories, own `agent.md` (References)
- **architect** → Architecture, API Contract, Technical Roadmap (optional), `CLAUDE.md`, own `agent.md` (References)
- **backend-developer** → own `agent.md` (References)
- **devops** → own `agent.md` (References)
- **frontend-developer** → own `agent.md` (References)
- **qa-automation** → own `agent.md` (References)
- **manual-qa** → own `agent.md` (References)

---

### Implement Task
*Applies to: backend-developer, devops, frontend-developer*

1. Read: Architecture, own `agent.md` (References), API Contract (if applicable), Stories, Tasks, Technical Roadmap (if exists), `CLAUDE.md`
2. Update Tasks status: `In Progress`
3. **Implement** *(overridable — each implementing role expands this step with their own sub-steps and checkpoints; see the role's instructions)*
4. Fill Implementation Report
5. Update Tasks status: `Done`
6. **QA** *(overridable — active QA roles expand this step; see qa-automation and manual-qa instructions. If no QA roles are configured, skip this step.)*

---

### Quick Fix
*Applies to: backend-developer, devops, frontend-developer*

For small bugs or misconfigurations that don't warrant a full story. All criteria must hold: ≤ 3 files affected, clear defect (not a feature), no architectural or infrastructure changes.

1. Get next STORY-NNN ID from Stories
2. Create a Story (type: fix)
3. Create a Task (self-assigned)
4. Apply the fix
5. Fill Implementation Report
6. Set Story status: `Pending Approval`
7. **QA** *(overridable — active QA roles expand this step; see qa-automation and manual-qa instructions. If no QA roles are configured, skip this step.)*

---

### Bug Fix
*Applies to: backend-developer, devops, frontend-developer (and future implementor roles)*

1. Read the Bug — description, steps to reproduce, severity, assignee
2. **Domain check — before writing any code**: verify the root cause is in your domain. If NOT: add investigation note to the Bug, use `AskUserQuestion` to ask the user who to reassign to, update assignee on the Bug, stop.
3. **Fix** *(overridable — each implementing role defines their own sub-steps; see role instructions)*
4. Set Bug status: `fixed`

After Bug Fix: QA re-runs Check Story — Execute to verify. If all pass, "Manual QA Passed" is set and the story proceeds to Accepted. If bugs remain, the loop repeats.

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

**Architect Reconcile** — trigger: Stories with status `Accepted`
1. Read the Story + its Tasks' Implementation Reports
2. Update Architecture, API Contract to reflect what was actually built
3. Create ADR files for confirmed decisions (own `agent.md`); mark superseded ADRs (Status field only)
4. Set "Architecture Reconciled" for each Story where all Tasks are `Done`

**Spec-Writer Reconcile** — trigger: Stories with `Accepted` status AND "Architecture Reconciled" set
1. Read the Story + its Tasks' Implementation Reports
2. Update Requirements to reflect reality
3. Set "Spec Reconciled"
4. Archive Stories with "Spec Reconciled" set; offer to delete story folders

**QA Reconcile** — optional; applies to qa-automation and manual-qa if active on the project
Runs in parallel with or after arch/spec reconcile. Detail defined in each QA role's own instructions. At minimum:
- Review test cases against what was actually built; update or retire cases that no longer match reality
- qa-automation: update automation scripts if behavior changed
- manual-qa: merge story-level test cases into its own test case registry (own `agent.md` Outputs); update TC files

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

## Artifacts Ownership

- **Validated Idea** → **product-strategist** (full)
- **Requirements** → **spec-writer** (full)
- **Milestones** → **spec-writer** (full)
- **Stories** → **spec-writer** (create, status `Planned`); **orchestrator** and any agent executing a Quick Fix or Bug Fix workflow may also create; implementors: status `In Progress` only; **orchestrator**: status `Pending Approval`; status flags — see Task Management for who sets each
- **Architecture** → **architect** (full)
- **API Contract** → **architect** (full)
- **Technical Roadmap** (optional) → **architect**; read-only for all
- **Tasks** → **architect** (create); implementors: status only
- **Implementation Report** → **implementor** (full, per task)
- **Bugs** → **manual-qa** and **qa-automation** (each creates their own)

---

## Artifact Content Rules

> Content requirements for the free-form parts of each artifact — not technical/bookkeeping fields (id, status, assignee, ...), which are handled by Task Management.

### Stories
Must state:
- Desired Outcome
- Context
- Acceptance Criteria
- Edge Cases & Error Scenarios

### Tasks
Must state:
- What to implement — at module/service level: no file paths, no function names
- Scope — modules/services affected
- Dependencies — other tasks, or "None"
- Acceptance Criteria

### Implementation Report
Filled by the implementor when a Task reaches `Done`. Must state:
- What was done
- Deviations from plan
- Technical decisions made
- Code locations
- Technical debt noted

### Bugs
Must state:
- Description
- Steps to Reproduce
- Expected Result
- Actual Result
