---
name: session
description: Use when opening or participating in a face-to-face session with a user and another agent. Defines caller behavior (for sessions that coordinate other agents) and callee behavior (for agents launched into a session via a briefing file).
---

# Session Skill

A face-to-face session is an interactive conversation between the user and a specific agent, opened in a separate terminal. This skill defines how both sides of the session behave.

---

## Determine your role

**You are the CALLER** if your current role involves coordinating other agents and you need to hand the user off to another agent for a dedicated interactive working session.

**You are the CALLEE** if your initial prompt contains a briefing file path (e.g. `docs/sessions/SESSION-NNN-<slug>.md`) — meaning another agent launched you into a session on behalf of the user.

---

## Caller instructions

You are opening a session with a callee agent. Follow these steps:

1. Write the briefing file `docs/sessions/SESSION-NNN-<slug>.md` using the format below
2. Show the user the command to run — display it as a copyable code block:
   ```
   cd /absolute/project/root && claude --agent <agent-name> "Invoke Skill(\"session\") and follow the briefing in docs/sessions/SESSION-NNN-<slug>.md"
   ```
   - Replace `/absolute/project/root` with the stored `$PROJECT_ROOT` path
   - `<agent-name>` is the `name:` value from the target agent's frontmatter (e.g. `business-analyst`, `architect`, `lead-developer`)
   - Tell the user: "Open a new terminal tab, paste this command, and come back when the session is done."
   - **Only auto-open a terminal via osascript if the user explicitly asks** (e.g. "open the terminal for me")
3. Tell the user: "Session ready for `<agent-name>`. Run the command above in a new terminal tab, then come back here."
4. Record the active wait somewhere your current context can find it on return (e.g. a state file)
5. When the user returns: read the `## Report` section of the session file → clear the wait record → continue

---

## Callee instructions

You were launched into a face-to-face session. Your initial prompt contains a briefing file path.

1. Read the briefing file — understand the goal, context, and topics to cover
2. Update the file's `status` frontmatter to `in-progress`
3. Greet the user — introduce yourself by role, state the session goal in one sentence, and ask if there is anything they want to discuss or clarify before you begin. Wait for their response.
4. Work through the session normally with the user, following the briefing
4. At session end: append the `## Report` section to the same file and update `status` to `completed`
5. Tell the user: "Session complete — you can return to the caller."

---

## Document ownership

- `docs/sessions/SESSION-NNN-<slug>.md` — **caller** writes the `## Briefing` section; **callee** appends the `## Report` section at session end

### Session file format

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
