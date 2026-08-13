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

```markdown
### Fast SDLC
Config: `<path to fast-sdlc-project-setup.md>`
```

**Owner**: orchestrator (created during Project Initiation) — but ownership is per-section, same pattern as `CLAUDE.md`: orchestrator owns the file's structure (Team, Configs Folder, Task Management), while each role may edit its own chapter under `## Artifacts` (location + brief access notes for the artifact(s) it owns — see `sdlc` > Artifacts Ownership).

**What's configurable** (not exhaustive):
- **Team** — active roles, each with a folder where that role's own `agent.md` lives (References: conventions/standards the role follows; Outputs: deliverables the role produces)
- **Configs Folder** — default base for role folders
- **Task Management** — which skill implements it (e.g. `file-based-task-management`) and where it operates
- **Artifacts** — the form each sdlc artifact takes and how it's managed (the notion of artifact is defined next)
- Any other project specifics

---

## Artifacts

- **Validated Idea** — scoped problem statement with explicit IN/OUT boundaries.
- **Requirements** — what's still to be done: data model, validation rules, user stories. Always about the future — once scope is delivered, its entry is removed (or handed off to Documentation, if that artifact exists on this project), never left describing what's already built.
- **Documentation** — what the product actually does, as-built. Kept true after the fact, not proposed/confirmed before it — the counterpart to Requirements, not a stage of it.
- **Architecture** — the technical design.
- **API Contract** — the interface between backend and frontend.
- **Technical Roadmap** — concrete technical plan for a milestone or story group — Detailed or Incremental, architect's judgment per scope. Exists while there's unreconciled Requirements to plan against.
- **Coding Conventions** — a repo's (or a repo+stack's) coding standards, grounded in its actual codebase.
- **Implementation Plan** — the technical approach and ordered task breakdown for a single story, within a single repo (or repo+stack).
- **Repo-level ADRs** — architectural decisions scoped to a single repo, distinct from project-level ADRs.
- **Milestones** — a composition of stories describing a development phase.
- **Stories** — a scoped unit of work and its lifecycle status.
- **Tasks** — a story's breakdown into implementable units, with status.
- **Implementation Report** — record of what was actually built for a task.
- **Bugs** — a defect filed against a story.

---

## Task Management

### Story Statuses

- `Planned` — set by **business-analyst** at story creation
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
- **Spec Reconciled** — set by **business-analyst** after Architecture Reconciled is set; triggers archival
- **Automation Reconciled** — set by **qa-automation** after automation tests/scripts are reconciled for this story
- **Manual QA Reconciled** — set by **manual-qa** after story test cases are merged; prevents double-merge

### Task Statuses

- `ToDo` — set by the creator (usually **lead-developer**; an implementor may self-create one, e.g. via Quick Fix)
- `In Progress` — set by **implementor** when work begins
- `Done` — set by **implementor** when complete + Implementation Report filled

### Bug Statuses

- `open` — set by **qa-automation**/**manual-qa** at filing
- `in-progress` — set by **implementor** when work begins
- `fixed` — set by **implementor**, awaiting QA verification
- `resolved` — set by **QA**, confirms the fix
- `wont-fix` — set by **QA** or the **user**

---

## Roles

Each phase of the pipeline is owned by a named role. Agents identify themselves by role name and understand their position in the sequence.

- **business-analyst** — Phases 1–2, Discovery and Spec: validates ideas before any technical work begins (gatekeeper between "someone had a thought" and "we're going to build this"), then bridges product vision and engineering execution — produces specs that are precise, traceable, unambiguous, and scoped to exactly what's needed. Does not write code, design architecture, or evaluate technical feasibility.

- **architect** — Phase 3, Architecture: designs systems that are simple to build and maintain. Values clarity over cleverness, boring technology over novelty. Produces Technical Documentation (Architecture, API Contract, ADRs) and the Technical Roadmap.

- **lead-developer** — Phase 4, Repo Planning: repo-bound (one instance per repo, scoped to one stack per session if the repo has more than one). Turns architect's Technical Roadmap into an Implementation Plan and Tasks for its own repo, and owns that repo's Coding Conventions and Repo-level ADRs.

- **coder** — Phase 5, Implementation: implements Tasks in one specific repo, scoped to one stack per session if the repo has more than one. Writes the code and its own unit/integration tests, following that stack's Coding Conventions.

- **devops** — Phase 5, Implementation: provisions and manages infrastructure. Works in pair with the user — never deploys autonomously. Steps requiring cloud console access are explicitly handed to the user with a checklist.

- **qa-automation** — Phase 6, qa-automation (Optional): verifies acceptance criteria against implemented features. Sets "Automated Tests Passed" on pass; files one bug per failing test and returns story to `In Progress` on fail.

- **manual-qa** — Phase 6, QA (optional): exploratory and scenario-based tester. Creates test cases at story start, executes them post-implementation, sets "Manual QA Passed" on pass or files bugs on fail. Does not write automated tests, does not touch story status.

- **orchestrator** — Meta-role (not a pipeline stage): coordinates the full pipeline from the current Claude Code session when the `orchestration` skill is loaded. Spawns agents, manages story lifecycle, triggers reconcile on acceptance, and runs face-to-face sessions. Does not implement features or analyze source code — all work is proxied through domain agents. When the user operates without the orchestration skill, the user drives the pipeline manually.

### Mapping between roles and agents

Use the mapped name whenever spawning an agent for a role.

| Role | Agent name |
|------|------------|
| business-analyst | fast-sdlc-business-analyst |
| architect | fast-sdlc-architect |
| lead-developer | fast-sdlc-lead-developer |
| coder | fast-sdlc-coder |
| devops | fast-sdlc-devops |
| qa-automation | fast-sdlc-qa-automation |
| manual-qa | fast-sdlc-manual-qa |
| orchestrator | — (not a spawnable agent; loaded via the `orchestration` skill) |

---

## Pipeline

```
business-analyst → architect → lead-developer → implementors → qa-automation → reconcile
```

### 1. Discovery
- **Role:** business-analyst
- **Required:** No — skip if the idea is already well-defined
- **Description:** Validates the business idea with the user before any technical work begins, turning a vague concept into a scoped problem statement with explicit IN/OUT boundaries.
- **Artifacts:** Validated Idea

### 2. Spec
- **Role:** business-analyst
- **Required:** Yes
- **Description:** Transforms the validated idea into engineering-ready specifications: data model, validation rules, and user stories.
- **Artifacts:** Requirements, Milestones, Stories

### 3. Architecture
- **Role:** architect
- **Required:** Yes — once for project setup, then once per milestone/story group thereafter
- **Description:** Designs the technical approach at the cross-repo level and produces the Technical Roadmap — the handoff from architect to each repo's lead-developer.
- **Artifacts:** Architecture, API Contract, Technical Roadmap

### 4. Repo Planning
- **Role:** lead-developer
- **Required:** Yes — once per repo (or repo+stack) touched by the Technical Roadmap
- **Description:** Turns the Technical Roadmap into an Implementation Plan and Tasks for its own repo — the primary context handoff from lead-developer to implementors carrying full reasoning, ordered tasks, and planned ADRs.
- **Artifacts:** Coding Conventions, Implementation Plan, Tasks

### 5. Implementation
- **Role:** coder, devops
- **Required:** Yes
- **Description:** Implementors execute assigned tasks and fill the Implementation Report in each task file as they go.
- **Artifacts:** Implementation Report

### 6. QA
- **Roles:** qa-automation, manual-qa (each optional independently)
- **Required:** No — conditional on project configuration
- **Description:** Runs after `Pending Approval`, before the user sets `Accepted`. qa-automation runs automated checks (sets "Automated Tests Passed" on pass, returns story to `In Progress` on fail). manual-qa runs exploratory/scenario testing (sets "Manual QA Passed" on pass, files bugs on fail). The user reviews these flags then decides to set `Accepted`.
- **Artifacts:** Stories, Bugs

### 7. Reconcile
- **Role:** lead-developer (first, per repo), architect (second), business-analyst (third) — sequential
- **Required:** Yes — runs after stories reach `Accepted` or by request
- **Description:** Closes the documentation loop — lead-developer checks its own Implementation Plan against what was built and surfaces cross-repo deviations to architect, architect folds the Technical Roadmap into Architecture/API Contract and records confirmed decisions as ADRs (project-level and repo-level), then business-analyst removes delivered scope from Requirements (handing it off to Documentation, if that artifact exists) and archives completed stories.
- **Artifacts:** Coding Conventions, Repo-level ADRs, Architecture, API Contract, Requirements, Documentation (optional), Stories

---

## Common Workflows

### Setup
*Applies to: all roles*

Run once per project before any work begins. Each role produces a strategy, conventions, or specification document that governs all subsequent work. Pauses for user approval — no further work until confirmed.

- **Project Config** → `fast-sdlc-project-setup.md`, established first — defines where everything below actually lives
- **Confirm Artifact Formats** *(applies to every role, for the artifacts whose format it itself defines — not Stories/Tasks/Bugs, whose format is the Task Management skill's concern)* → once Project Config exists: before producing such an artifact for the first time on this project, if the role has a default format for it, present that default and ask whether to use it as-is or customize it; if the role has no default, challenge the user to define one. Do not proceed past this step until the format of every such artifact required by the role is settled. Record the decision in that artifact's chapter under `fast-sdlc-project-setup.md` > Artifacts (or in the role's own `agent.md`, if the artifact isn't in the shared Artifacts list).
- **business-analyst** → Validated Idea, Requirements, Milestones, Stories, own `agent.md` (References)
- **architect** → Architecture, API Contract, Technical Roadmap, `CLAUDE.md`, own `agent.md` (References)
- **lead-developer** → Coding Conventions, `CLAUDE.md` (own repo), own `agent.md` (References)
- **coder** → own `agent.md` (References)
- **devops** → own `agent.md` (References)
- **qa-automation** → own `agent.md` (References)
- **manual-qa** → own `agent.md` (References)

---

### Implement Story
*Applies to: architect (roadmap), lead-developer (plan + deviation checks with architect), business-analyst (deviation checks on Requirements), implementors (via Implement Task), qa-automation/manual-qa (QA), lead-developer/architect/business-analyst (Reconcile)*

1. **Roadmap**: architect produces or updates the Technical Roadmap covering the story's scope, if not already current
2. **Plan**: for each repo (or repo+stack) the Technical Roadmap names, lead-developer produces the Implementation Plan and Tasks for the story — raising a Deviation Check to architect first if the Roadmap doesn't fit that repo's reality
3. **Deviation Check**: if the Implementation Plan diverges from Requirements — business-analyst updates Requirements
4. If manual-qa is active: manual-qa creates test cases for the story, based on the now-current Requirements
5. **Implement**: run Implement Task for each task across all repos' Implementation Plans, in dependency order, until all Tasks are `Done`
6. **Deviation Check**: if any task's Implementation Report notes a deviation — lead-developer routes it to architect if it crosses repo boundaries, or handles it as a Repo-level ADR if not; business-analyst updates Requirements if the deviation has behavioral impact; if manual-qa is active and its test cases are affected, it updates them
7. **QA**: qa-automation sets "Automated Tests Passed"; if manual-qa is active, it runs its test cases and sets "Manual QA Passed"
8. User reviews, sets Story status: `Accepted`
9. **Reconcile** *(see Reconcile workflow)*

---

### Implement Task
*Applies to: coder, devops*

1. Read: Architecture, Coding Conventions, own `agent.md` (References), API Contract (if applicable), Stories, Implementation Plan, Tasks, Technical Roadmap (if exists), `CLAUDE.md`
2. Update Tasks status: `In Progress`
3. **Implement** *(overridable — each implementing role expands this step with their own sub-steps and checkpoints; see the role's instructions)*
4. Fill Implementation Report
5. Update Tasks status: `Done`
6. **QA** *(overridable — qa-automation expands this step, if active. Manual QA runs at the Story level, not per-task — see Implement Story.)*

---

### Quick Fix
*Applies to: coder, devops*

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
*Applies to: coder, devops*

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
*Sequential: lead-developer runs first (per repo), architect second, business-analyst third*

**Lead-Developer Reconcile** — trigger: this repo's (or repo+stack's) Tasks for a Story are all `Done`
1. Read the Story's Tasks and Implementation Reports for this repo
2. Compare against the Implementation Plan; for confirmed decisions scoped to this repo only, create a Repo-level ADR (own `agent.md`); for deviations crossing repo boundaries, raise them to architect
3. Update Coding Conventions and `CLAUDE.md` (own repo) if the story revealed a gap

**Architect Reconcile** — trigger: Stories with status `Accepted`
1. Read the Story + its Tasks' Implementation Reports, and any deviations raised by lead-developer(s)
2. Fold the Technical Roadmap into Architecture, API Contract to reflect what was actually built
3. Create ADR files for confirmed project-level decisions (own `agent.md`); mark superseded ADRs (Status field only)
4. Set "Architecture Reconciled" for each Story where all Tasks are `Done`

**Business-Analyst Reconcile** — trigger: Stories with `Accepted` status AND "Architecture Reconciled" set
1. Read the Story + its Tasks' Implementation Reports
2. Remove the delivered scope from Requirements (it's built now, not future work); if Documentation exists on this project, update it with what was actually built
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
**Callee**: any agent that supports it — business-analyst, architect, lead-developer, coder, devops, manual-qa

The session is powered by the `session` skill:
- The orchestrator loads this skill to know how to open and manage sessions
- The callee agent loads this skill to know how to receive, execute, and report back on a briefing

Refer to `~/.claude/skills/orchestration/SKILL.md` and `~/.claude/skills/session/SKILL.md` for details.

---

## Artifacts Ownership

- **Validated Idea** → **business-analyst** (full)
- **Requirements** → **business-analyst** (full)
- **Documentation** → **business-analyst** (business-facing behavior) and **architect** (technical/system-facing) — each owns their own part, neither the other's
- **Milestones** → **business-analyst** (full)
- **Stories** → **business-analyst** (create, status `Planned`); **orchestrator** and any agent executing a Quick Fix or Bug Fix workflow may also create; implementors: status `In Progress` only; **orchestrator**: status `Pending Approval`; status flags — see Task Management for who sets each
- **Architecture** → **architect** (full)
- **API Contract** → **architect** (full)
- **Technical Roadmap** → **architect**; read-only for all
- **Coding Conventions** → **lead-developer** (full, per repo or repo+stack)
- **Repo-level ADRs** → **lead-developer** (create, per repo or repo+stack)
- **Implementation Plan** → **lead-developer** (create, per story per repo/stack); read-only for implementors
- **Tasks** → **lead-developer** (create; an implementor may self-create one, e.g. via Quick Fix); implementors: status only
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
