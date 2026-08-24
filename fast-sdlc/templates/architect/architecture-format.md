<!-- Default format for the Architecture artifact — accept as-is or customize with architect during Step 0 (Confirm Artifact Formats). Created or updated only at Reconcile — never written directly in Project Setup or Plan Next Scope mode. -->

# Architecture

## Tech Stack
Every system-level technology choice (language/runtime per service, cloud provider, managed services), with rationale.

## Services
Every deployable service in the system, grouped by bounded context. No code paths — internal code structure is lead-developer's domain.

- **Bounded Context: [Name]** — [one-line purpose]
  - [Service name] ([repo/platform, if applicable]) — [one-line purpose]

## Service Relations
How services depend on each other, including within the same bounded context.

- `[Service A] → [Service B]: [reason]`

## Data Flow
How a request moves through the system, across services. Numbered steps or nested bullets.

## Database
Per data store: provider/technology, and its Physical Data Model — either inline here directly, or a pointer to where it actually lives, per the location confirmed for that store (see DATA MODELING > "New storage platform"). Never both — the confirmed location is the single source of truth for that store's schema.

## Authentication Flow
Numbered steps describing the login/session/refresh cycle across services. Per-endpoint auth requirements live in API Contract, not here.

## Error Handling Strategy
Cross-service error policy: error envelope shape, retry/circuit-breaking semantics, user-facing message policy. Per-endpoint error responses live in API Contract, not here.

## Logging/Observability
Log structure, correlation IDs across services, where logs/traces go.

## Deployment Topology
System-level shape: which services exist, public vs internal exposure, shared infrastructure (DB, queues, gateway). Not literal env vars or IaC — that's devops's/lead-developer's domain.
