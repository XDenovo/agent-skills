---
name: to-issue
description: Publish the design already discussed in this conversation as a GitHub issue — decides single-repo vs cross-repo scope from the Repository Map, embeds the branch name so implement never invents one, and explains cross-repo implementation dependencies and parallel waves. Use when the user wants the discussed change filed as an issue, or as the first stage of /ship.
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

## 5. Plan cross-repo execution

For a cross-repo Initiative, derive the implementation dependency graph after every sub-issue is
created and linked. An edge needs a concrete reason, such as a contract, reusable workflow commit,
runtime release, generated template, or deployment input that one sub-issue must produce before
another can proceed. Different owning repositories do not automatically make work independent.

Group the sub-issues into ordered implementation waves, then write or update the Initiative's
`## Implementation order` section with the exact sub-issue URLs:

- put every currently unblocked, non-conflicting sub-issue in the same parallel wave and explain
  why they can proceed independently;
- place dependent sub-issues in later waves, naming the prerequisite issue and the exact artifact
  or decision they require;
- use a mixed plan when appropriate: parallelize within a wave and serialize only across real
  dependency edges.

Prefer the maximum safe parallelism supported by the dependency graph. If nothing can run in
parallel, give the complete serial order and a concrete reason for every transition.

## 6. Verify Project status

For every Issue created in step 4, poll the XDenovo Platform Project with
`gh project item-list 1 --owner XDenovo --limit 1000 --format json` and match the exact Issue URL
in `.items[].content.url`. Check at most 12 times with 5 seconds between attempts; automation is
asynchronous, so absence or another status on an early attempt is not yet a failure.

Every created Issue must appear with `Status` exactly `Todo`. Do not repair the Project item
manually: if any Issue does not reach `Todo` within the bounded wait, stop this stage and report
the Issue URL plus the last observed status so the automation failure remains visible.

## 7. Report back

Reply with every issue URL created (the Initiative first, then its sub-issues, if cross-repo) and
the decided branch slug. For a cross-repo Initiative, also explain the implementation relationship
in the same waves recorded on the Initiative: identify parallel sub-issues, serial dependencies,
and the reason for each dependency. Never return only an unsequenced list of issue links.

## Edge cases

The conversation never settled on seams/scope precisely → synthesize your best understanding, state the assumption plainly in the issue body, and proceed. Don't block publishing on a full interview.
