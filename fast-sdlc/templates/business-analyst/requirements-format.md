<!-- Proposed format for the Requirements artifact — not a confirmed default. Walk the user through it and confirm/adjust with business-analyst during Step 0 (Confirm Artifact Formats). -->

# Requirements Structure

Requirements is a folder, not a single file — a flat requirements.md doesn't scale once a project has more than a handful of features.

```
<Requirements location>/
  index.md
  data-model.md
  non-functional.md
  <feature-slug>.md            (flat — default)
  <domain>/<feature-slug>.md   (grouped — optional, for projects where features cluster into domains, e.g. admin/, support/)
```

Grouping into domain folders is optional and per-project — pick flat or grouped (or a mix) at Step 0. Either way, `index.md` is the single source of truth for where each feature's file actually lives; nothing else assumes flat or grouped.

"Feature" here is the same unit as Story Decomposition Rule 1 (`sdlc` > business-analyst) — a user scenario or business capability, never a screen.

---

## `index.md`

```markdown
## Feature List
| Feature | File | Description | Priority | Access |
|---------|------|-------------|----------|--------|
| ...     | `<feature-slug>.md` or `<domain>/<feature-slug>.md` | ... (note if a list within the feature must be paginated) | P0/P1/P2 | Public / Authorized users / Authorized users with <role/permission> |
```

## `<feature-slug>.md` (one per feature)

```markdown
# <Feature Name>

## Validation Rules
### Field-Level
| Entity | Field | Rule | Error Message |
|--------|-------|------|---------------|

### Business-Level
Numbered list of business rules that span multiple fields or entities.

## Error Handling
Paths that diverge from the happy path — what can go wrong from the user's perspective, and what the user experiences as a result. No HTTP status, no logging requirements — that's for architect/implementor to decide.

| Scenario | User-Facing Outcome |
|----------|---------------------|

## Usage Limits (optional — only where a business reason applies)
| Feature | Limit | Rationale |
|---------|-------|-----------|
```

## `data-model.md` (project-wide — entities are often shared across features)

```markdown
For each entity:
### EntityName
| Field | Type | Required | Constraints | Notes |
|-------|------|----------|-------------|-------|

Relationships: describe cardinality (1:1, 1:N, M:N) and foreign key behavior (CASCADE, RESTRICT, etc.)
Indexes: list all indexes with justification
```

## `non-functional.md` (project-wide)

```markdown
### Performance
- Page load targets
- Response time targets
- Concurrent user targets

### Security
- Data sensitivity classifications
- Encryption requirements (at rest, in transit)

### Accessibility
- WCAG 2.1 AA compliance required
- Specific component-level requirements

### Browser & Device Support
- List supported browsers and minimum versions
- Mobile responsiveness requirements
```
