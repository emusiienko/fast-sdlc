# fast-sdlc
Agentic Framework for software development

## Overview

Fast SDLC is a Claude Code plugin that turns a session into an orchestrator coordinating specialized AI agents. It can take any scope, from business requirements to tested code, through a defined software development lifecycle: each stage is handled by an agent with a single, well-defined role.

It is built for developers who want to delegate a whole delivery pipeline to AI agents while keeping control over what gets merged.

## Key Features

- **Orchestrator–subagent architecture**: one orchestrator session plans and tracks the work, and spawns a specialized subagent for each task.
- **End-to-end SDLC pipeline**: business analysis, architecture, repo-level planning, implementation, QA, and documentation reconcile.
- **Multiple workflows**: project initiation, new work planning, story execution, issue reporting, and research.
- **Multi-repository support**: architecture is designed across repos, and each repo (and stack) gets its own implementation plan and tasks.
- **Human-in-the-loop review**: the user reviews the plan, the pull requests, and the final acceptance before a story is considered done.

## How It Works

```mermaid
flowchart LR
    U([User]) <--> O{{Orchestrator}}
    O --> BA[business-analyst]
    O --> AR[architect]
    O --> LD[lead-developer]
    O --> IM[coder / devops]
    O --> QA[qa-automation / manual-qa]
    BA --> AR --> LD --> IM --> QA
    QA --> PR[/Pull request review/]
    PR --> U
```

The user talks only to the orchestrator. For each story it creates tasks and spawns the responsible subagent: requirements first, then architecture, then a per-repo implementation plan, then the code, then QA. Implementation is opened as a pull request for the user to review, and after acceptance the architecture and requirements documents are reconciled with what was actually built.

## Related Projects

- [claude-code-project-management-utility](https://github.com/emusiienko/claude-code-project-management-utility): a companion task management utility.

## Architecture Decisions

Decisions about the design of this framework are recorded in the [adr](adr/) folder.

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
