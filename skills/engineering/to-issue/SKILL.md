---
name: to-issue
description: Publish the design already discussed in this conversation as a GitHub issue — decides single-repo vs cross-repo scope from the Repository Map, and embeds the branch name so implement never invents one. Use when the user wants the discussed change filed as an issue, or as the first stage of /ship.
---

# To Issue

Run at the workspace root, where root `AGENTS.md`'s Repository Map and Cross-Repository Workflow are in scope.

## 1. Synthesize, don't interview

Work from what the conversation and codebase exploration already settled — problem, solution, seams. Do not re-ask questions already answered. Only ask the user something genuinely unresolved (see Edge cases); never guess at ownership or scope.

## 2. Decide scope

Match the discussed change against root `AGENTS.md`'s Repository Map:

- **Exactly one owning repo** → single-repo scope.
- **More than one owning repo, or a change to a cross-repository contract/architecture boundary/shared platform decision** → cross-repo scope, per root `AGENTS.md`'s Cross-Repository Workflow.
- **Ownership doesn't resolve cleanly** → ask the user which repo(s) own it. Don't guess.

## 3. Decide the branch slug

Pick one short kebab-case slug describing the change (e.g. `add-rate-limit-header`). This slug is shared across every issue this run creates — it's what keeps sibling sub-issues coherent, since each still gets its own per-repo issue number. The full branch/worktree name assembled later is `agent/<issue-number>-<slug>`, decided here so `implement` never invents one.

## 4. Publish

Pick the issue type (Feature Spec, Bug Report, or Chore) that matches the change's nature. Exact section-by-section bodies to reproduce (`gh issue create --body` skips the template UI, so its structure must be typed out by hand), the Branch field format, the Issue Type PATCH command, and the sub-issue linking command are all in [references/publishing.md](references/publishing.md) — read it now.

**Single-repo**: create the one issue in the owning repo, set its Issue Type, then edit it to add the `## Branch` field once the issue number is known.

**Cross-repo**: create the Initiative issue in `XDenovo/platform` first (Initiative template, Issue Type `Initiative`), then one self-contained implementation issue per owning repo (that repo's matching template + its own `## Branch` field). Finish by linking every one of those sub-issues to the Initiative — the Initiative is only complete once each repo's sub-issue is attached, per root `AGENTS.md`'s Cross-Repository Workflow.

## 5. Report back

Reply with every issue URL created (the Initiative first, then its sub-issues, if cross-repo) and the decided branch slug.

## Edge cases

The conversation never settled on seams/scope precisely → synthesize your best understanding, state the assumption plainly in the issue body, and proceed. Don't block publishing on a full interview.
