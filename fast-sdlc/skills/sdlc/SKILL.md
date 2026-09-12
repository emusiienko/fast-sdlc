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

**Owner**: orchestrator (created during Project Initiation) — but ownership is per-section, same pattern as `CLAUDE.md`: orchestrator owns the file's structure (Team, Configs Folder, Task Management, Sessions, Pull Requests, Escalation/Feedback), while each role may edit its own chapter under `## Artifacts` (location + brief access notes for the artifact(s) it owns — see `sdlc` > Artifacts Ownership).

**What's configurable** (not exhaustive):
- **Team** — active roles, each with a folder where that role's own `agent.md` lives (References — a settled format/location decision that has no home elsewhere in this config, nothing else; see the rule below). A repo-bound role (lead-developer, coder) gets its own section and folder per stack if the repo has more than one — never one shared folder across stacks. Every role except orchestrator also has one or more **working folder(s)** — used only by the orchestrator, to decide where to spawn that role (inline or face-to-face); the role itself is never aware of this field. Defaults to that role's own generated-artifact location(s) where one exists (Source Code, Infrastructure Code, Automated Test Suite, Test Case Registry), or project root for roles whose artifacts are too varied to reduce to one path (business-analyst, architect).
- **Configs Folder** — default base for role folders
- **Task Management** — which skill implements it (e.g. `file-based-task-management`) and where it operates
- **Sessions** — how face-to-face session coordination is actually mechanized (a skill, an MCP server, etc.) — default: `file-based-session-management` skill
- **Pull Requests** — branching strategy for Acceptance/Reconcile PRs (target development directly, or a feature branch) — default: target development
- **Escalation/Feedback** — how one role raises something to another role, or to the user, mid-pipeline, once it has already produced its output — default: `file-based-escalation` skill
- **Artifacts** — the form each sdlc artifact takes and how it's managed (the notion of artifact is defined next)
- Any other project specifics

**`agent.md` is not memory.** It holds only a settled format/location decision that has no home in Artifacts — nothing else. Never write conventions, standards, patterns, or general notes there, even ones you find useful or ones missing from a canonical artifact. A gap in a canonical artifact (Coding Conventions, Technical Documentation, etc.) gets flagged to that artifact's owner — never filled in your own `agent.md` instead.

**`fast-sdlc-project-setup.md` and `CLAUDE.md` aren't for task-bound information.** Don't write temporary artifacts or anything bound to the task at hand into either file — a location/format chapter that's already been confirmed stays as it is. Any modification to either file must be a decision explicitly stated in the current task's own description; otherwise, don't touch them — flag a discrepancy instead of correcting it yourself.

---

## Artifacts

- **Validated Idea** — scoped problem statement with explicit IN/OUT boundaries.
- **Requirements** — what's still to be done: data model, validation rules, user stories. Once scope is delivered, hand its content off to Business Documentation, if that artifact exists on this project.
- **Business Documentation** — what the product actually does, as-built, from the business/behavioral view. Kept true after the fact, not proposed/confirmed before it — the counterpart to Requirements, not a stage of it.
- **Technical Documentation** — what the system actually is, as-built. A folder: SAD (its master document), Solution Overview, ADR Log, NFR, Data Models, API Contracts, and (when topology warrants it) Deployment/Infrastructure View. The counterpart to Architectural Design, not a stage of it.
- **Architectural Design** — architect's Phase 1 (Scope Planning) output for a milestone or story group: Solution Overview, NFR, and the Technical Implementation Plan. Exists while there's unreconciled Requirements to plan against.
- **Story Implementation Plan** — architect's cross-repo detail for a single story, during its execution window: technical approach, key decisions, and implementation steps for other roles to turn into their own Tasks. Produced only when a story needs it.
- **Coding Conventions** — a repo's (or a repo+stack's) coding standards, grounded in its actual codebase.
- **Implementation Plan** — the technical approach and ordered task breakdown for a single story, within a single repo (or repo+stack).
- **Story Implementation Deviation Report** — lead-developer's filtered account of what deviated from plan for a story, within a single repo (or repo+stack), sorted into the sections each downstream role actually needs (Architecture, Data Models, API Contracts, Requirements) — read instead of raw Implementation Reports.
- **Repo-level ADRs** — architectural decisions scoped to a single repo, distinct from project-level ADRs.
- **Source Code** — the actual implementation for a repo (or repo+stack).
- **Code Map** — coder's own account of where things actually live in the code, module/function-level — more detailed than lead-developer's folder-level Repository Structure, kept current at Reconcile.
- **Automated Test Suite** — qa-automation's black-box test code, verifying the system from the outside across repos/services.
- **Infrastructure Code** — IaC, CI/CD pipelines, deployment scripts, and environment configuration.
- **Test Case Registry** — manual-qa's index of test cases and their individual TC files.
- **Milestones** — a composition of stories describing a development phase.
- **Stories** — a scoped unit of work and its lifecycle status.
- **Tasks** — a story's breakdown into implementable units, with status.
- **Implementation Report** — record of what was actually built for a task.
- **Bugs** — a defect filed against a story.
- **Pull Requests** — a reviewable unit of change, opened by the orchestrator at Acceptance and again at Reconcile, bundling whichever roles' work is ready at that point. Not one-per-story — a story's work may span several.

---

## Task Management

### Story Statuses

- `Planned` — set by **business-analyst** at story creation
- `In Progress` — set by **implementor** when first task starts
- `Pending Approval` — set by **orchestrator** when all implementation Tasks are `Done`
- `Approved` — set by **manual-qa** itself, when its own Task reaches a full pass **and no bugs remain open** against this story — QA's own approval, distinct from the user's final `Accepted`. While any bug is open, the status stays `Pending Approval` regardless of what else passed. Not used if manual-qa isn't active on this project — the story is Accept-eligible directly from `Pending Approval` in that case.
- `Accepted` — set by **user** after review — always explicit, never automatic
- `Done` — set by **orchestrator** once every Reconcile Task for the story is `Done`

A story is ready for its next stage whenever every relevant Task is `Done` — there is no separate flag to track.

### Task Statuses

- `ToDo` — set by whoever creates the task
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

- **architect** — Phase 3, Architecture: designs systems that are simple to build and maintain. Values clarity over cleverness, boring technology over novelty. Produces Technical Documentation and Architectural Design.

- **lead-developer** — Phase 4, Repo Planning: repo-bound (one instance per repo, scoped to one stack per session if the repo has more than one). Turns architect's Architectural Design into an Implementation Plan and Tasks for its own repo, and owns that repo's Coding Conventions and Repo-level ADRs.

- **coder** — Phase 5, Implementation: implements Tasks in one specific repo, scoped to one stack per session if the repo has more than one. Writes the code and its own unit/integration tests, following that stack's Coding Conventions.

- **devops** — Phase 5, Implementation: provisions and manages infrastructure. Works in pair with the user — never deploys autonomously. Steps requiring cloud console access are explicitly handed to the user with a checklist.

- **qa-automation** — Phase 6, QA (Optional): writes black-box tests for a story from its API Contracts and acceptance criteria, then verifies them against implemented features, ahead of manual-qa. Files a Bug per finding.

- **manual-qa** — Phase 6, QA (optional): exploratory and scenario-based tester — the final check on work implementors and qa-automation already consider complete. Creates test cases at story start and executes them once qa-automation (if active) is done. Approves the story on a full pass with no open bugs; files bugs on fail.

- **orchestrator** — Meta-role (not a pipeline stage): coordinates the full pipeline from the current Claude Code session when the `orchestration` skill is loaded. Spawns agents, manages story lifecycle, creates QA/Reconcile Tasks, opens Pull Requests at Acceptance and Reconcile, and runs face-to-face sessions. Does not implement features or analyze source code — all work is proxied through domain agents. When the user operates without the orchestration skill, the user drives the pipeline manually.

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
business-analyst → architect → lead-developer → implementors → qa-automation/manual-qa → acceptance → reconcile
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
- **Artifacts:** Requirements, Stories

### 3. Architecture
- **Role:** architect
- **Required:** Yes — once for project setup, then once per milestone/story group thereafter
- **Description:** Designs the technical approach at the cross-repo level and produces Architectural Design — the handoff from architect to each repo's lead-developer.
- **Artifacts:** Technical Documentation, Architectural Design

### 4. Repo Planning
- **Role:** lead-developer
- **Required:** Yes — once per repo (or repo+stack) touched by the Technical Implementation Plan
- **Description:** Turns Architectural Design into an Implementation Plan and Tasks for its own repo — the primary context handoff from lead-developer to implementors carrying full reasoning, ordered tasks, and planned ADRs.
- **Artifacts:** Coding Conventions, Implementation Plan, Tasks

### 5. Implementation
- **Role:** coder, devops
- **Required:** Yes
- **Description:** Implementors execute assigned tasks and fill the Implementation Report in each task file as they go.
- **Artifacts:** Implementation Report

### 6. QA
- **Roles:** qa-automation, manual-qa (each optional independently)
- **Required:** No — conditional on project configuration
- **Description:** Runs after `Pending Approval`, before Acceptance. Orchestrator creates a Task for each active QA role once implementation Tasks are all `Done`; if both are active, manual-qa's Task depends on qa-automation's, so qa-automation always runs first. Either role's Task reaches `Done` once that round's check has run and been reported — reporting a failure (a Bug filed) is as valid a completion as a clean pass. Whenever a Bug either role filed is open, the orchestrator routes it through Bug Fix and, once it's marked `fixed`, creates a fresh Task for that same role to re-verify — repeating until none remain open. manual-qa alone sets the story `Approved`, once its latest Task is `Done` and no Bug is open against the story at all (including ones qa-automation found). If manual-qa isn't active, the story is Accept-eligible directly from `Pending Approval`.
- **Artifacts:** Tasks, Bugs

### 7. Acceptance
- **Role:** orchestrator (opens the Pull Request if needed, dispatches feedback), user (reviews, decides, merges)
- **Required:** Yes
- **Description:** Once the story is Accept-eligible (`Approved`, or `Pending Approval` complete if manual-qa isn't active), orchestrator opens the Pull Request(s) bundling the story's implementation work, if not already open. User reviews; any feedback left on the PR is routed by the orchestrator to the responsible role (via the `pr-review-response` skill) — looping until the user is satisfied. User then sets `Accepted` and merges the Pull Request.
- **Artifacts:** Pull Requests

### 8. Reconcile
- **Role:** architect, business-analyst, qa-automation/manual-qa (each if active) — architect and business-analyst sequential, QA roles in parallel
- **Required:** Yes — runs after the Acceptance Pull Request is merged, or by request
- **Description:** Orchestrator creates one Reconcile Task per applicable role, using each Task's `Dependencies` field to sequence them: business-analyst's depends on architect's; QA roles' have no dependency. What each role does within its own Task is defined in that role's own instructions. Any role whose work needs its own PR gets a new branch, named after the (now-merged) implementation branch plus a suffix describing the reconcile work (e.g. `<implementation-branch-name>-docs-reconcile`). Once every Reconcile Task for a story is `Done`, orchestrator opens the Pull Request(s) it needs — routing any feedback the same way as Acceptance — for the user to merge; story then reaches `Done`.
- **Artifacts:** Tasks, Pull Requests

---

## Common Workflows

### Setup
*Applies to: all roles*

Run once per project before any work begins. Each role produces a strategy, conventions, or specification document that governs all subsequent work. Pauses for user approval — no further work until confirmed.

- **Project Config** → `fast-sdlc-project-setup.md`, established first — defines where everything below actually lives
- **Confirm Artifact Formats** *(applies to every role, for each artifact it owns — not Milestones/Stories/Tasks/Bugs, whose setup is the Task Management skill's concern)* → once Project Config exists: before producing such an artifact for the first time on this project, confirm each of the following with the user, as its own message, one artifact at a time:
  - **Location** — where it actually lives; state the template's placeholder or a proposed path before asking. Confirm it even when it seems obvious, and never fold two distinct artifacts into one shared location without the user explicitly choosing that.
  - **Format** — if the role has a default, present it and ask whether to use it as-is or customize it; if it has no default, challenge the user to define one.
  - **Versioning** — where applicable, present the default mechanism and confirm, or ask.
  Do not proceed past this step until every owned artifact's location and format are settled. Record each decision in that artifact's chapter under `fast-sdlc-project-setup.md` > Artifacts (or in the role's own `agent.md`, if the artifact isn't in the shared Artifacts list).
- **business-analyst** → Validated Idea, Requirements, Stories, own `agent.md` (References)
- **architect** → Technical Documentation, Architectural Design, `CLAUDE.md`'s Key Docs chapter, own `agent.md` (References)
- **lead-developer** → Coding Conventions, `CLAUDE.md` (own repo), own `agent.md` (References)
- **coder** → own `agent.md` (References)
- **devops** → own `agent.md` (References)
- **qa-automation** → own `agent.md` (References)
- **manual-qa** → own `agent.md` (References)

---

### Implement Story
*Applies to: architect (roadmap, plan story implementation — if on the team), lead-developer (plan + deviation checks with architect), business-analyst (deviation checks on Requirements), implementors (via Implement Task), qa-automation/manual-qa (QA), orchestrator (Task creation, status, PRs), architect/business-analyst/qa-automation/manual-qa (Reconcile)*

1. **Scope Planning**: architect produces or updates Architectural Design covering the story's scope, if not already current
2. **Detail** *(if architect is on the team)*: architect updates API Contracts and Data Models in place for what this story touches, and creates a Story Implementation Plan (see MODE 3: Plan Story Implementation)
3. **Plan**: lead-developer reads Requirements, the story description, and — wherever they exist — architect's artifacts (Architectural Design, Technical Documentation, Story Implementation Plan); produces this stack's Implementation Plan (optional — see lead-developer's own Plan Implementation mode) and Tasks for the story — raising a Deviation Check to architect first if the plan doesn't fit that repo's reality
4. **Deviation Check**: if the Implementation Plan diverges from Requirements — business-analyst updates Requirements
5. If manual-qa is active: manual-qa creates test cases for the story, based on the now-current Requirements
6. **Implement**: run Implement Task for each task across all repos' Implementation Plans, in dependency order, until all Tasks are `Done`
7. **Deviation Check**: if any task's Implementation Report notes a deviation — lead-developer routes it to architect if it crosses repo boundaries, or handles it as a Repo-level ADR if not; business-analyst updates Requirements if the deviation has behavioral impact; if manual-qa is active and its test cases are affected, it updates them
8. **QA** *(see Pipeline > QA for the full mechanic)*: orchestrator sets `Pending Approval` and creates a Task per active QA role; manual-qa sets `Approved` once its own latest Task is `Done` with no Bug left open (immediately Accept-eligible from `Pending Approval` if manual-qa isn't active)
9. **Acceptance** *(see Pipeline > Acceptance)*: orchestrator opens the Pull Request(s); user reviews, sends any feedback through the orchestrator, then sets `Accepted` and merges
10. **Reconcile** *(see Reconcile workflow)* — story reaches `Done` once complete

---

### Implement Task
*Applies to: coder, devops*

1. Read: Technical Documentation, Coding Conventions, own `agent.md` (References), Stories, Implementation Plan, Tasks, Architectural Design (if exists), `CLAUDE.md`
2. Update Tasks status: `In Progress`
3. **Implement** *(overridable — each implementing role expands this step with their own sub-steps and checkpoints; see the role's instructions)*
4. Fill Implementation Report
5. Update Tasks status: `Done`
6. **QA** *(not per-task — both QA roles run at the Story level, once all its Tasks are `Done` — see Implement Story)*

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

After Bug Fix: the orchestrator creates a fresh Task for whichever QA role filed the Bug, to re-verify. Once no Bug remains open, manual-qa (if active) sets `Approved`. If bugs remain, the loop repeats.

---

### Troubleshoot
*Applies to: devops (primary); other roles as needed*

1. Search official documentation before any action
2. State the most likely root cause and evidence
3. Confirm the proposed action with the user before executing it
4. One change at a time — verify each step before the next

---

### Reconcile
*Trigger: the story's Acceptance Pull Request is merged, or by request*

1. Orchestrator creates one Reconcile Task per applicable role: lead-developer (`Dependencies: None`, one per stack), coder (`Dependencies: None`, one per stack — always, regardless of whether lead-developer is on the team), architect (`Dependencies:` every lead-developer Reconcile Task), business-analyst (`Dependencies:` architect's task)
2. Each role works its Task once its dependencies are `Done`, committing to a new branch named after the (now-merged) implementation branch plus a suffix describing its own reconcile work — high level, per role (full detail in each role's own instructions):
   - **lead-developer**: produces the Story Implementation Deviation Report for its stack, sorted into Architecture/Data Models/API Contracts/Requirements; records repo-level ADRs and Coding Conventions/`CLAUDE.md` updates where warranted
   - **coder**: updates its stack's Code Map to reflect what was actually built, independent of lead-developer's own Reconcile Task
   - **architect**: reads the Story Implementation Deviation Report(s); updates whichever of Technical Documentation, Data Models, API Contracts, and the ADR Log the report's sections affect
   - **business-analyst**: reads the Story Implementation Deviation Report(s)' Requirements sections; updates Business Documentation with what was built, if that artifact exists on this project; updates Requirements too if a discrepancy was found
3. Once every Reconcile Task for a story is `Done`, orchestrator opens the Pull Request(s) it needs, routing feedback the same way as Acceptance; once the user merges, orchestrator sets the story `Done` and archives it

qa-automation and manual-qa sit outside this automatic chain — orchestrator creates each of their Reconcile Tasks separately, by its own judgment (typically once the Story Implementation Deviation Report exists, and again later if it changes, e.g. after a bug-fix round). Each reads the report's relevant section on its own Task; see each role's own instructions.

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
- **Business Documentation** → **business-analyst** (full)
- **Technical Documentation** → **architect** (full)
- **Milestones** → **orchestrator** (create/update, on explicit user request); file shape and location governed by Task Management, same as Stories
- **Stories** → **business-analyst** (create, status `Planned`); **orchestrator** and any agent executing a Quick Fix or Bug Fix workflow may also create; implementors: status `In Progress` only; remaining statuses — see Task Management for who sets each
- **Architectural Design** → **architect**; read-only for all
- **Story Implementation Plan** → **architect** (create, per story, only when needed); read-only for all
- **Coding Conventions** → **lead-developer** (full, per repo or repo+stack)
- **Repo-level ADRs** → **lead-developer** (create, per repo or repo+stack)
- **Implementation Plan** → **lead-developer** (create, per story per repo/stack); read-only for implementors
- **Story Implementation Deviation Report** → **lead-developer** (create, per story per repo/stack); **coder** (create, same location, only when lead-developer isn't on the team); read-only for architect, business-analyst, qa-automation, manual-qa
- **Source Code** → **coder** (full, per repo or repo+stack); **lead-developer** (full, same location, only when writing code directly — see its MODE 5)
- **Code Map** → **coder** (full, per repo or repo+stack)
- **Automated Test Suite** → **qa-automation** (full)
- **Infrastructure Code** → **devops** (full, per repo/stack/environment)
- **Test Case Registry** → **manual-qa** (full, per repo or repo+stack)
- **Tasks** → **lead-developer** (create, for implementation); **orchestrator** (create, for QA and Reconcile — see Pipeline > QA and > Reconcile); any role may self-create one for its own work (e.g. Quick Fix); assignee: status + Implementation Report only
- **Implementation Report** → **implementor** (full, per task)
- **Bugs** → **manual-qa** and **qa-automation** (each creates their own)
- **Pull Requests** → **orchestrator** (opens, at Acceptance and at Reconcile); roles commit their own work onto the relevant branch; the **user** merges

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

### Escalations
Must state:
- Brief Topic (title)
- The role to who escalation goes (always 1 role)
- Problem — what's being raised, and the artifacts involved
- Suggested resolution, if there is one

When communicating with another role:
- Use the professional language of the artifacts involved in the escalation — to the maximum degree your own territory allows, without crossing into another role's
- Where your own territory's language runs out, fall back to common-sense language and abstractions — never cross into another role's territory to fill the gap
- When a term from your own domain is unavoidable to make your point, ideas, or problem clear, introduce it — paired with a plain-language explanation of what it means
- A generic term that merely overlaps another domain (e.g. "a dropdown") isn't jargon — fine to use
- No translation needed when the recipient already shares your vocabulary

---

## Writing Discipline

Applies to any free-form artifact — on top of whatever fields Artifact Content Rules above requires.

**Write it only if removing it would make the artifact incomplete or lose its sense** — a scenario step, an architectural service/component, a function being added or changed, an acceptance criterion, a constraint. That's the bar for what belongs.

Cut, don't add:
- Promotional or justifying language about a decision already made ("this ensures scalability", "this provides significant benefits") — state the decision and its constraints, not why it's good
- A rationale already implied by the decision itself, restated
- Anything else that fails the bar above

Genuinely non-obvious rationale — e.g. why a common alternative was rejected — isn't padding and stays.
