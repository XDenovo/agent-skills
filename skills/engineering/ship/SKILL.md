---
name: ship
description: Run the full to-issue → implement → open-pr → close-issue pipeline end-to-end for a design just discussed in conversation, switching repo/worktree context between stages, on top of each stage's own confirmation gating. Use only when the user explicitly wants the whole issue-to-merge pipeline run in one go.
---

# Ship

Confirm which stages to run before starting — default to all four; if the user only wants to get as far as, say, a PR, run only that far.

## 1. to-issue

Invoke `/to-issue`. Carry forward every issue it creates and the implementation waves it reports.
A single-repo issue means the rest of this run stays in that one repo.

For a cross-repo Initiative, run steps 2-4 according to those waves. Execute independent
sub-issues in the same wave in parallel when the available agent surface supports parallel work.
Do not start a later wave until the prerequisite issue has produced the contract, commit, release,
template, or other artifact named by `/to-issue`. If parallel execution is unavailable, say so and
process otherwise-independent work serially; do not misrepresent that execution constraint as an
implementation dependency.

## 2. implement

Invoke `/implement` on the issue from step 1. Carry forward the branch/worktree it creates.

## 3. open-pr

Invoke `/open-pr` in that worktree. Carry forward the PR it opens.

## 4. close-issue

Invoke `/close-issue` on that PR.

## Stopping

A stage failing, erroring, or the user declining one of its confirmations ends the run right there — don't skip ahead, don't retry silently. Report which stage you stopped at and the state it left behind (issue claimed, draft PR open, etc.) so the user can resume manually.
