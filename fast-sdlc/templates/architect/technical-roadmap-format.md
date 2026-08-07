<!-- Default format for the Technical Roadmap artifact — accept as-is or customize with architect during Step 0 (Confirm Artifact Formats). -->

# Technical Roadmap

Concrete technical plan for a milestone or story group — not an implementation plan for a specific repo (that's lead-developer's job once this is agreed). Two forms — architect chooses per scope, based on how substantial the change is, not automatically by mode. If unsure which fits, ask the user.

## Detailed form

Used when the architecture changes substantially, or is being created from scratch (always the case at Project Setup, since there's no existing Technical Documentation to diff against yet).

Contains the expected Architecture (see architecture-format.md) and the expected API Contract (see api-contract-format.md) for the full scope, plus:

```
## Steps
1. STORY-001: <what, and why it comes first>
2. STORY-002 (depends on STORY-001): <what, and why it comes next>
```

## Incremental form

Used when the change is small relative to the existing system. Covers only what's new or changed, not a restatement of the whole system. Uses the same section names as Architecture (Tech Stack, Services, Service Relations, Data Flow, Database, Authentication Flow, Error Handling Strategy, Logging/Observability, Deployment Topology) — include only the ones that actually change for this scope, so Reconcile is a direct per-section merge into Architecture, not a re-interpretation.

```
# Technical Roadmap: <Milestone or story group>

## Context
Why this scope exists and how it fits into the existing system.

## <Architecture section name(s) that change — e.g. "Database", "Service Relations">
Only what's new or changed in this section, for this scope.

## API Contract Deltas
New or changed endpoints. Full detail (method, path, request/response schema, auth) — these become the live API Contract at Reconcile.

## Dependencies
Ordering constraints within this scope, on prior scope, or across repos/platforms — e.g. `STORY-001 (repo-a) → STORY-003 (repo-b): auth foundation required before protected routes`.

## Platforms Involved
Which repos/platforms this scope touches — tells lead-developer(s) who needs to plan implementation.

## Planned ADRs
Decisions likely to need an ADR once implementation confirms them. Actual ADR files are created in Reconcile — these are intentions only.

| Decision Area | Trigger Condition |
|---|---|
| Choose session strategy for OAuth flow | If third-party provider is added |
```
