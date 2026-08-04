# fast-sdlc
Agentic Framework for softwar development

## Installation

The `fast-sdlc/` folder is a Claude Code plugin (`.claude-plugin/`, `agents/`, `skills/`). No build or deploy step is required.

### Quick test (no install)

Load the plugin for a single session without registering it anywhere:

```bash
claude --plugin-dir /path/to/fast-sdlc/fast-sdlc
```

### Project-scoped install

Enable the plugin for one specific project only:

```bash
# from inside the target project
/plugin marketplace add /path/to/fast-sdlc/fast-sdlc --scope project
/plugin install fast-sdlc@fast-sdlc
```

### Global (user-level) install

Enable the plugin across all projects:

```bash
/plugin marketplace add /path/to/fast-sdlc/fast-sdlc
/plugin install fast-sdlc@fast-sdlc
```

Before a global install, remove or rename any `~/.claude/agents/*.md` files that share a base name with an agent in this plugin (e.g. `architect`) — a global agent file overrides a same-named plugin agent. Plugin skills are automatically namespaced (`fast-sdlc:skill-name`) and don't have this issue.

### After editing plugin files

```bash
/reload-plugins
```
