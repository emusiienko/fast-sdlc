---
name: pr-review-response
description: Use when handling review comments on an existing pull/merge request. Defines how to process feedback via `gh` — reading, classifying, fixing, and replying.
---

# PR Review Response Skill

**Trigger**: given a specific PR reference. If none was given, stop and ask.

**Process**:
1. Read every comment on the PR via `gh`
2. Identify which comments are addressed to you specifically — a PR can carry comments meant for more than one area of work; only act on the ones that are yours
3. For each of those, classify it:
   - **Actionable** (a requested change you agree with) — fix it, and reply
   - **Debatable** (you disagree with the suggested change, or believe the code is already correct as-is) — reply with your rationale for keeping the current approach or proposing an alternative; don't apply a change you think is wrong just because it was requested
   - **Informational** (a question, an FYI) — reply, no code change
   - **Needs clarification** (the comment itself is ambiguous) — reply asking what's meant; don't guess
   - **Needs a decision** (the comment is clear, but resolving it requires a decision which is not provided or change outside this work's own scope/authority) — reply flagging that; don't fix it until the decsion is made and provided.
4. Apply fixes per whatever conventions already govern this codebase
5. Commit, push to the same branch — the PR updates automatically, no need to reopen anything
6. Reply to every actionable comment, referencing the commit that addressed it. Reply to informational comments too — silence on a comment reads as it being ignored.
7. Once every actionable comment is addressed, note that it's ready for re-review; don't assume the reviewer is watching for silent pushes.

---

## Guidelines

- Keep replies short and concise.
- Sign every reply with your own name. PR comments and pushes are all attributed to the same GitHub account regardless of who's acting — without a signature there's no way to tell who actually responded.
- If number of actionable items is much less than number of others take a round without changing the code, processing all items apart from actionable.
- If you decide to update the code, processed all actionable items in one single commit. 
- If a comment's fix could plausibly apply to other places beyond the one it was left on, and it's not clear whether it should — ask whether to apply it there too, rather than assuming either way.
