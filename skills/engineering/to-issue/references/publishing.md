# Publishing mechanics

Body templates to reproduce (org defaults, from `dot-github/.github/ISSUE_TEMPLATE/`, used whenever the target repo has no local override), the `## Branch` field, setting Issue Type, and linking sub-issues.

## Body templates

Fill every section; omit a heading only if the template marks it optional and there's nothing to say.

### Feature Spec

```markdown
## Goal

<What are we building, and why — plain language.>

## User scenario

<Given/When/Then, or a short user story.>

## Functional requirements

FR-001: System MUST ...
FR-002: [NEEDS CLARIFICATION: ...]  <!-- use this tag instead of guessing -->

## Out of scope

<What's explicitly excluded.>

## Edge cases

<Boundary conditions and error scenarios the implementation must handle.>

## Success criteria

<Measurable, technology-agnostic — the Definition of Done the closing PR verifies.>

## Context

<Links: docs/architecture.md section, related issues, prior art.>

## Branch

`agent/<issue-number>-<slug>`
```

### Bug Report

```markdown
## What happened?

## Expected behavior

## Steps to reproduce

## Environment

<Version, OS, deployment target (local / control-plane / gpu-node), etc.>

## Relevant logs

## Branch

`agent/<issue-number>-<slug>`
```

### Chore

```markdown
## What needs to change

## Why now

## Done when

<The condition that makes this closeable.>

## Branch

`agent/<issue-number>-<slug>`
```

### Initiative (cross-repo only, filed in `XDenovo/platform`)

```markdown
## Goal

<What are we achieving across repos, and why.>

## Repos involved

<owning repo, one per line>

## Success criteria

<Measurable, across the whole initiative.>
```

The Initiative issue has no `## Branch` field of its own — branches live on its per-repo sub-issues.

## Setting Issue Type

`gh issue create`/`gh issue edit` in this `gh` version have no `--type` flag, and `--body` skips the template UI that would otherwise set it. Set it directly via the REST API after creation:

```bash
gh api repos/<owner>/<repo>/issues/<number> -X PATCH -f type="Feature"   # or Bug, Task, Initiative
```

`Task` is the org's Issue Type for a Chore.

## Adding the Branch field

Two-step because the issue number doesn't exist until after creation:

```bash
gh issue create --repo <owner>/<repo> --title "<title>" --label <label> --body "<body without the Branch section>"
# → note the issue number from the returned URL
gh issue edit <number> --repo <owner>/<repo> --body "$(gh issue view <number> --repo <owner>/<repo> --json body -q .body)

## Branch

\`agent/<number>-<slug>\`"
```

## Linking a sub-issue to the Initiative

Requires the sub-issue's numeric database `id` (not its `number` — sub-issues can live in a different repo than the parent):

```bash
sub_id=$(gh api repos/<sub-owner>/<sub-repo>/issues/<sub-number> --jq .id)
gh api repos/XDenovo/platform/issues/<initiative-number>/sub_issues -F sub_issue_id="$sub_id"
```

Repeat once per owning repo's sub-issue.
