---
status: Done
assignee: orchestrator
story: STORY-001
---

# TASK-010: Update fast-sdlc-devops agent

## What to implement
Update the agent to resolve task-file and conventions-doc locations from `fast-sdlc-project-setup.md` instead of assuming fixed `docs/` paths.

## Scope
`fast-sdlc/fast-sdlc/agents/fast-sdlc-devops.md`

## Dependencies
TASK-003

## Acceptance Criteria
- [x] No remaining hardcoded path this agent should instead resolve from config (verified via grep, no matches)

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-15
**What was done**: Rewrote `fast-sdlc-devops.md` to resolve every path dynamically, then extended the pass (per direct user requests) into a broader cleanup of its DevOps Strategy artifact:

- **BEFORE STARTING / OWNERSHIP**: added the same `fast-sdlc-project-setup.md` resolution pattern as `coder`/`qa-automation`/`manual-qa` — own location, Architecture/Technical Roadmap/Implementation Plan locations (read-only), Task Management location — replacing every hardcoded `docs/architecture.md`, `docs/devops-strategy.md`, `docs/stories/...`, `docs/technical_roadmap.md` reference
- **OWNERSHIP mirrors `coder`'s pattern**: Stories via Quick Fix only, Tasks Implementation-Report-only unless self-created, Bugs status `fixed` only
- **Description frontmatter fixed**: was missing Mode 5 (Bug Fix) entirely — said "four modes" while defining five; corrected to five and added the missing bullet
- **Templates externalized** to `templates/devops/` (mirroring architect/lead-developer's pattern): `devops-strategy-format.md`, `claude-md-infrastructure-format.md`, `readme-deployment-format.md` — referenced via `${CLAUDE_PLUGIN_ROOT}` instead of embedded inline
- **CI/CD folded into the DevOps Strategy** (explicit user request): the former standalone CI/CD Pipelines template is now a section inside the Strategy itself — one comprehensive document, not two
- **SETUP QUESTIONNAIRE removed** (explicit user request): its 9 questions are now explicit named sections/fields in the Strategy template (Project Profile, Cloud & Repository) — Setup mode now asks "whatever the template needs that isn't already answered by Architecture," the same pattern every other role's Setup mode already uses, instead of maintaining a separate, drift-prone parallel list
- **Concrete file/folder names removed from the agent's own instructions** (explicit user request) — `deploy.sh`, `infra/`, etc. only appear as the template's own suggested defaults now, never asserted as fixed truth in the agent's operating instructions (Outputs, Process steps, SELF-VERIFICATION all genericized to "deployment entry point" / "IaC files" / "per the Strategy's ... section")
- Fixed a pre-existing step-numbering gap in MODE 1 (jumped from step 4 to step 6)

**Deviations from plan**: Grew substantially past "dynamic paths," per explicit follow-up requests in the same session — same pattern as the last several tasks in this story.

**Technical decisions made**: DevOps Strategy is now the single comprehensive artifact for every DevOps decision (cloud/repo/compliance profile, environments, branch strategy, IaC, CI/CD, secrets, deployment, rollback) — no separate CI/CD template, no separate questionnaire.

**Code locations**: `fast-sdlc/agents/fast-sdlc-devops.md`, `fast-sdlc/templates/devops/devops-strategy-format.md`, `fast-sdlc/templates/devops/claude-md-infrastructure-format.md`, `fast-sdlc/templates/devops/readme-deployment-format.md`

**Technical debt noted**: None remaining from this pass.
