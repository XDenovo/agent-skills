---
name: ship
description: Run the full to-issue → implement → open-pr → close-issue pipeline end-to-end for a design just discussed in conversation, switching repo/worktree context between stages, on top of each stage's own confirmation gating. Use only when the user explicitly wants the whole issue-to-merge pipeline run in one go.
---

# Ship

Confirm which stages to run before starting — default to all four; if the user only wants to get as far as, say, a PR, run only that far.

## 1. to-issue

Invoke `/to-issue`. Carry forward every issue it creates. A single-repo issue means the rest of this run stays in that one repo. A cross-repo Initiative means repeating steps 2-4 once per owning repo's sub-issue — each is otherwise independent of the others.

## 2. implement

Invoke `/implement` on the issue from step 1. Carry forward the branch/worktree it creates.

## 3. open-pr

Invoke `/open-pr` in that worktree. Carry forward the PR it opens.

## 4. close-issue

Invoke `/close-issue` on that PR.

## Stopping

A stage failing, erroring, or the user declining one of its confirmations ends the run right there — don't skip ahead, don't retry silently. Report which stage you stopped at and the state it left behind (issue claimed, draft PR open, etc.) so the user can resume manually.
