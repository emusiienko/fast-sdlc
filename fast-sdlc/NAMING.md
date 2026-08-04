# Naming Map

Reference for this plugin's identifiers — used to prevent collisions with global (`~/.claude/agents`, `~/.claude/skills`) definitions of the same base role, and as the lookup table when rewriting cross-references between agents/skills inside this plugin.

## Agents (prefixed)

| Original name | Plugin name |
|---|---|
| architect | fast-sdlc-architect |
| backend-developer | fast-sdlc-backend-developer |
| devops | fast-sdlc-devops |
| frontend-developer | fast-sdlc-frontend-developer |
| manual-qa | fast-sdlc-manual-qa |
| product-strategist | fast-sdlc-product-strategist |
| qa-automation | fast-sdlc-qa-automation |
| spec-writer | fast-sdlc-spec-writer |

## Skills (not prefixed — auto-namespaced by the plugin system as `fast-sdlc:<name>`)

- orchestration
- sdlc
- session
