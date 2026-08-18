---
name: file-based-session-management
description: Implements Sessions (as defined in the session skill) using files — face-to-face session briefings and reports as markdown files. One possible Sessions mechanism; selected per-project in fast-sdlc-project-setup.md.
---

# File-Based Session Management

Renders a face-to-face session (see `session` skill for the CALLER/CALLEE protocol) as a file. A project selects this skill in `fast-sdlc-project-setup.md` > Sessions, which also names the operating folder this skill works in (default `docs/sessions/`).

This skill defines only the file shape and how to create/read/update it — the `session` skill defines what a caller and callee actually do with the briefing and report.

---

## Structure

### Session file (`<operating folder>/SESSION-NNN-<slug>.md`)

```markdown
---
session: SESSION-NNN
agent: <agent-name>
status: briefing | in-progress | completed
---

## Briefing
**Goal**: [what this session should accomplish]
**Context**: [The context of the conversation during which the callee has been launched. It may contain relevant paths and current state — but no file contents]
**Topics**: [specific things to cover or decide]

---

## Report
*(filled by the callee agent at session end)*

**Decisions made**: ...
**Open questions**: ...
**Summary**: ...
```

---

## Flows

### Create a session (caller)
1. Get the next `SESSION-NNN` ID — highest existing + 1, from the operating folder
2. Create `<operating folder>/SESSION-NNN-<slug>.md` with the `## Briefing` section filled in and `status: briefing`
3. Return the file's path — the caller hands this to the callee as the briefing reference

### Start a session (callee)
1. Read the file at the given briefing reference
2. Set `status: in-progress`

### Complete a session (callee)
1. Append the `## Report` section
2. Set `status: completed`

### Read a session's report (caller)
1. Read the `## Report` section from the file at the briefing reference
