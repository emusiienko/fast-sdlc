---
name: file-based-escalation
description: Implements the Escalation/Feedback mechanism (as defined in the sdlc skill) using files — one thread per escalation, tracked in an index. One possible Escalation/Feedback mechanism; selected per-project in fast-sdlc-project-setup.md.
---

# File-Based Escalation

Renders the Escalation/Feedback concept from `sdlc` (one role raising something to another role, or to the user, mid-pipeline — after it has already produced its output) as files. A project selects this skill in `fast-sdlc-project-setup.md` > Escalation/Feedback, which also names the operating folder this skill works in (default `docs/escalations/`).

Load this skill whenever you need to raise something to another role or to the user, or when you need to check for and respond to an escalation addressed to you.

---

## Structure

### Index (`<operating folder>/index.md`)

```markdown
| Escalation | Topic | Requester | Responder(s) | Status | Opened | Closed |
|---|---|---|---|---|---|---|
| [escalation-roadmap-fit-lead-developer-architect](escalation-roadmap-fit-lead-developer-architect.md) | Roadmap doesn't fit this repo | lead-developer | architect | open | 2026-08-20 | |
```

### Escalation file (`<operating folder>/escalation-<topic>-<requester>-<responders>.md`)

```markdown
## <date> — <from> → <to>
<the topic, the artifacts involved, and, if there is one, a suggested resolution>
```

Each new entry — from either side — is appended under its own dated heading. The file carries the whole conversation, oldest first.

---

## Flows

### Raise an escalation
1. Pick a short topic slug and identify the responder(s) who need to answer — another role, or `user`
2. Create `escalation-<topic>-<requester>-<responders>.md` in the operating folder with your opening entry
3. Add a row to `index.md` with status `open`
4. Note the escalation's existence in your own report, so the orchestrator can route a response

### Respond to an escalation
1. Open the escalation file addressed to your role
2. Read every prior entry in full
3. Append a new dated entry with your answer, a follow-up question, or a proposed resolution
4. Leave the escalation open if more discussion is needed — the same file carries the whole conversation

### Close an escalation
Only the requester who opened the escalation decides when it's resolved.
1. Append a closing entry
2. Update the row in `index.md` to `closed`, with the closing date
