---
name: file-based-versioning
description: Implements artifact versioning using files — a "latest" file, a changelog, and a capped set of archived prior versions. One possible versioning mechanism for an artifact; the artifact's chapter in fast-sdlc-project-setup.md names which mechanism it uses.
---

# File-Based Versioning

Renders artifact versioning as files: a "latest" file (the current version), a changelog (human-readable history of changes), and a capped set of archived prior versions.

---

## Structure

### Layout

```
<artifact folder>/
  latest.md
  changelog.md
  <artifact>-vN.md   (archived, max 3 kept)
```

### Changelog entry format

```markdown
## v{N} [YYYY-MM-DD]: <short change title>

<Human-readable description of what changed vs. the previous version.>

**Integration Points:** <existing capabilities affected, or "None">
**Breaking Changes:** <existing behaviors that change, or "None">
```

Consumers may extend this format with additional fields specific to their own use (e.g. a `**Stories:**` field to track downstream work generated from each entry).

---

## Flows

### Create initial version
1. Write `latest.md`
2. Create `changelog.md` — no entry yet, nothing to compare the initial version against

### Add a new version
1. Determine next version number — list existing `<artifact>-vN.md` archives; next N = highest existing N + 1, or 1 if none exist
2. Archive the current version — copy `latest.md` verbatim to `<artifact>-v{N}.md` (no edits)
3. Write the new `latest.md` with the fully updated content
4. Append a changelog entry (see format above)
5. Enforce max 3 archives — if archived versions now exceed 3, delete the oldest
