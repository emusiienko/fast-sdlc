---
status: Planned
type: chore
priority: TBD
date: 2026-08-06
---

# STORY-002: Merge product-strategist and spec-writer into business-analyst

## Desired Outcome

One role, `business-analyst`, replaces `product-strategist` and `spec-writer` — Discovery and Spec become two sequential modes of the same agent instead of two separate roles/agents. Reinforces the earlier decision to remove API Contract from spec-writer's scope: a business-facing role never touches technical interface design.

## Context

`product-strategist` (Discovery, optional gatekeeper) and `spec-writer` (Spec, required) are both business/product-facing, non-technical roles, sequential in the pipeline. Discovery is already `Required: No` in practice — the gate is often skipped anyway. Merging removes a handoff without dropping the validate-then-specify discipline, which is preserved as two internal modes of one agent (the same pattern `architect` already uses: Project Setup / Feature Design / Reconcile modes).

## Acceptance Criteria

- [ ] `business-analyst` agent created (Discovery Mode + Spec Mode), replacing the two source agents' content
- [ ] `fast-sdlc-product-strategist` and `fast-sdlc-spec-writer` agent files retired
- [ ] `sdlc` skill updated: Roles, Mapping between roles and agents, Pipeline (phases 1+2 merged), Artifacts Ownership, Common Workflows (Setup, Reconcile)
- [ ] `orchestration` skill updated: every reference to product-strategist/spec-writer
- [ ] `session` skill updated: Callee list
- [ ] No leftover reference to `product-strategist` or `spec-writer` as separate roles anywhere in the plugin

## Edge Cases & Error Scenarios

- [ ] Face-to-face session default: both source roles default to face-to-face today — decide the merged role's default (likely: still face-to-face, since both source roles were)
