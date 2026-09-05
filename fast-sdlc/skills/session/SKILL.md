---
name: session
description: Use when opening or participating in a face-to-face session with a user and another agent. Defines caller behavior (for sessions that coordinate other agents) and callee behavior (for agents launched into a session via a briefing).
---

# Session Skill

A face-to-face session is an interactive conversation between the user and a specific agent, opened in a separate terminal. This skill defines how both sides of the session behave. How the briefing/report are actually stored and exchanged is a separate, per-project concern — see `sdlc` > Project Config > Sessions.

---

## Determine your role

**You are the CALLER** if your current role involves coordinating other agents and you need to hand the user off to another agent for a dedicated interactive working session.

**You are the CALLEE** if your initial prompt contains a briefing reference — meaning another agent launched you into a session on behalf of the user.

---

## Caller instructions

You are opening a session with a callee agent. Follow these steps:

1. Locate `fast-sdlc-project-setup.md` (see `sdlc` > Project Config) and resolve Sessions (the configured mechanism). Create the session through that mechanism's own flow, with your goal/context/topics as the briefing — this gives you a briefing reference to hand to the callee.
2. Show the user the command to run — display it as a copyable code block:
   ```
   cd /absolute/working/folder && claude --agent <agent-name> "Invoke Skill(\"session\") and follow the briefing: <briefing reference>"
   ```
   - Replace `/absolute/working/folder` with the callee role's resolved working folder — same resolution as for an inline spawn (see `orchestration` skill > Sub-scenario: Fulfill a Task): read it from `fast-sdlc-project-setup.md` > Team > `<role>` > working folder(s), picking whichever entry the session's topic actually concerns if more than one is configured. No difference in behavior from an inline spawn.
   - `<agent-name>` is the `name:` value from the target agent's frontmatter (for example `fast-sdlc-architect`)
   - Tell the user: "Open a new terminal tab, paste this command, and come back when the session is done."
   - **Only auto-open a terminal via osascript if the user explicitly asks** (e.g. "open the terminal for me")
3. Tell the user: "Session ready for `<agent-name>`. Run the command above in a new terminal tab, then come back here."
4. Record the active wait somewhere your current context can find it on return (e.g. a state file)
5. When the user returns: read the session's report through the configured Sessions mechanism → clear the wait record → continue

---

## Callee instructions

You were launched into a face-to-face session. Your initial prompt contains a briefing reference.

1. Read the briefing through the configured Sessions mechanism (see `sdlc` > Project Config > Sessions) — understand the goal, context, and topics to cover
2. Mark the session `in-progress` through that mechanism
3. Greet the user — introduce yourself by role, state the session goal in one sentence, and ask if there is anything they want to discuss or clarify before you begin. Wait for their response.
4. Work through the session normally with the user, following the briefing
5. At session end: record the report and mark the session `completed`, through the configured mechanism
6. Tell the user: "Session complete — you can return to the caller."
