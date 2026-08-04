# Orchestrator Conventions

## 1. Project structure mirrors the plugin structure

The plugin lives in its own top-level folder, `fast-sdlc/`, sibling to `docs/` — no separate packaging step, no build. Layout:

```
fast-sdlc/               (repo root)
├── docs/
├── README.md
└── fast-sdlc/           (plugin root)
    ├── .claude-plugin/
    │   ├── plugin.json
    │   └── marketplace.json
    ├── agents/
    └── skills/
```

## 2. Installation is documented in README.md, not scripted

No deploy script — installing/enabling the plugin is a handful of native Claude Code commands (`/plugin marketplace add`, `/plugin install`, `/reload-plugins`, or `claude --plugin-dir` for ad-hoc testing). These steps live in `README.md` (global install and project-scoped install), not in a shell script.

## 3. Naming: prefix agents, not skills

- **Skills**: no manual prefix. The plugin system auto-namespaces skill invocation as `plugin-name:skill-name` at dispatch time — this is separate from the skill's own `name` field, so no collision risk with global `~/.claude/skills/`.
- **Agents**: manually prefix with `fast-sdlc-` (hyphen, e.g. `fast-sdlc-architect`) — not `fast-sdlc:architect`. The `name` field in an agent's frontmatter cannot contain `:` (reserved for the plugin-scoping mechanism), and a global `~/.claude/agents/<name>.md` with the same base name overrides a same-named plugin agent. The prefix is the only protection until global definitions are retired.

## 4. SDLC is adapted, not followed literally

This project doesn't have the roles the `sdlc` skill assumes (no architect, no implementors, no QA). The `sdlc` skill is kept as reference only — so the orchestrator understands the process being designed/refined here — not as this project's own literal workflow.

- **Stories/tasks**: the lifecycle stays (story → tasks → Pending Approval → Accepted), but the orchestrator manages it personally instead of spawning agents. The orchestrator creates stories and tasks, and assigns each task to itself or to the user (for tasks that require the user to test something).
- **User-assigned tasks**: the user reports completion verbally; the orchestrator updates the task's status and Implementation Report on the user's behalf.
- **`docs/stories/index.md`**: no QA/Reconcile columns (`Auto ✓`, `QA ✓`, `Arch ✓`, `Spec ✓`, `Auto Rec ✓`, `QA Rec ✓`) — just `ID | Title | Type | Priority | Status`.
- **No Reconcile phase**: there's no architect/spec-writer to reconcile docs against.
- **`Accepted` stays a hard gate**: dropping Reconcile does not loosen this — it still requires the user's explicit confirmation, same as before.
