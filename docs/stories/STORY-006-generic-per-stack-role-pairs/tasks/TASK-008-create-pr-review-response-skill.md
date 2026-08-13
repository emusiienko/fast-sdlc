---
status: Done
assignee: orchestrator
story: STORY-006
---

# TASK-008: Create pr-review-response skill

## What to implement
Shared skill for handling review comments on an existing PR — mechanics only (read every comment via `gh`, classify as actionable/informational/out-of-scope, fix, commit, push, reply). Originally scoped to also cover opening a PR, but that turned out too mechanically trivial (push, `gh pr create`, done) to need a shared skill — cut, keeping only the part with real, non-obvious process value (comment classification, escalation, reply discipline).

## Scope
`fast-sdlc/fast-sdlc/skills/pr-review-response/SKILL.md`

## Dependencies
None

## Acceptance Criteria
- [x] Skill created — read every comment, filter to what's addressed to this work, classify, fix, commit, push, reply to all
- [x] No role names or SDLC-specific terminology in the mechanics
- [ ] `fast-sdlc-coder` (TASK-002) references this skill instead of re-explaining PR-comment mechanics inline — deferred to TASK-002 itself, not yet started

## Intended ADR

---
## Implementation Report
<!-- filled by implementor -->
**Date**: 2026-08-13
**What was done**: Wrote `fast-sdlc/skills/pr-review-response/SKILL.md` through many small, deliberate rounds of correction:

- Dropped the originally-planned "Create" section entirely — opening a PR turned out too mechanically trivial (push, `gh pr create`, done) to justify a shared skill; kept only "Respond to Comments," the part with real, non-obvious process value
- Stripped all role names and SDLC-specific artifact vocabulary (ADR, Architecture, API Contract, Conventions, Story/Task) to keep it genuinely reusable
- Removed several instructions that turned out vacuous once examined: a "precondition" that was really SDLC-process logic, not this skill's concern; "not just the most recent" comment (the actual `gh` commands already return the full list — nothing to guard against); "mention it in whatever gets reported back" (vague — this skill doesn't know what report exists in any given caller, and the PR reply itself is already the durable record)
- Split the original single "escalate" bucket into two distinct, real categories: **Needs clarification** (the comment itself is ambiguous — ask) vs. **Needs a decision** (clear, but outside this work's own authority — flag, don't fix until resolved)
- Added **Debatable** as its own category — the skill doesn't assume every comment is correct; disagreement gets a rationale reply, not blind compliance
- Added a step to filter comments to only the ones addressed to this specific work, before classifying — relevant once a PR can carry comments meant for more than one area of work (multi-repo/multi-stack)
- Added Guidelines: keep replies short; sign every reply (all PR activity shares one GitHub account, so without a signature nobody can tell which agent responded); batch a no-code-change pass separately from a code-change pass when actionable items are a small minority; one commit for all actionable items, not one per comment; ask before generalizing a fix to other locations it wasn't explicitly requested for

**Deviations from plan**: Scope narrowed from the original task description (Create + Respond) down to Respond only — Create didn't survive being made properly agnostic, there was no real content left in it.

**Technical decisions made**: Confirmed via direct question that `gh` supports seeing the full comment history including human replies to the agent's own prior comments (via `in_reply_to_id` on review comments, and chronological ordering on general PR comments) — this is what makes the "reply, then check back later" loop implicit in this skill actually workable.

**Code locations**: `fast-sdlc/skills/pr-review-response/SKILL.md`

**Technical debt noted**: Wiring `fast-sdlc-coder` to reference this skill is TASK-002's job, not started yet.
