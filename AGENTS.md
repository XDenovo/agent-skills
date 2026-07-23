# XDenovo Agent Skills

## Repository Role

- This public repository is the canonical source and distribution repository for reusable XDenovo agent skills.
- It contains two skill catalogs with different audiences:
  - `engineering` skills support XDenovo team members while developing and operating XDenovo repositories.
  - `product-use` skills teach external users' agents how to use published XDenovo products, including public MCP capabilities.
- Future Codex and Claude Code plugins may package selected skills with MCP connection configuration and presentation assets.
- Application source, MCP server implementations, deployment manifests, and repository-specific instructions belong in their owning repositories.

## Catalog Layout

```text
skills/
  engineering/
    <skill-name>/
      SKILL.md
  product-use/
    <skill-name>/
      SKILL.md
```

- Treat `skills/` as the canonical skill source. Do not maintain independent copies for individual agent clients.
- Keep every skill name unique across both catalogs; categories do not namespace installed skills.
- Do not place published catalog skills under `.agents/skills/`. That location is for skills that should activate while working on this repository, not for the repository's distributable catalog.
- Use `metadata.internal: true` only for deliberately hidden or unfinished skills. The `engineering` category describes its audience and does not by itself make a skill internal.

## Engineering Skills

- Design these skills to be installed into an XDenovo development repository with the `skills` CLI and executed in that target repository.
- Require the agent to read the target repository's `AGENTS.md`, manifests, scripts, and CI before acting. The target repository remains authoritative for its commands and ownership boundaries.
- Keep a rule in the target repository's `AGENTS.md` when it applies to only that repository. Use an engineering skill for a reusable workflow or expertise shared across repositories.
- The repository is public even when a skill is intended for the XDenovo team. Do not include non-public architecture details, credentials, production data, private endpoints, or secret-bearing examples.
- Defer deployment authorization and destructive-operation safeguards to the target repository while retaining any stricter safety requirements defined by the skill.

## Product-Use Skills

- Write for external users and their agents. Do not assume access to an XDenovo source checkout, internal network, or private documentation.
- Depend only on published product behavior, public documentation, user-provided authentication, and supported client capabilities.
- Connect MCP workflows through the public Gateway at `https://api.xdenovoai.com/`. Do not expose private Compute MCP Service endpoints or internal credentials.
- Keep tool names, schemas, examples, and authentication guidance aligned with the released public contract. Do not document planned behavior as if it were available.
- Use synthetic inputs and outputs in examples; never include customer, research, or production data.

## Skill Authoring

- Each skill is a directory containing `SKILL.md` and may include `scripts/`, `references/`, and `assets/`.
- Use a lowercase kebab-case directory name of at most 64 characters and make the frontmatter `name` match it exactly.
- Write a concise `description` that states what the skill does, when it should trigger, and any important boundary that prevents false activation.
- Use `compatibility` only when the skill has material client, system-package, network, or runtime requirements.
- Treat `allowed-tools` as compatibility metadata, not as an enforceable security boundary.
- Keep `SKILL.md` focused on the workflow. Move detailed material into directly linked reference files and avoid reference chains.
- Resolve files relative to the skill root. Do not use workspace-relative paths, developer-specific absolute paths, or dependencies outside the skill directory.
- Prefer instructions for judgment-heavy workflows and scripts for deterministic operations. Scripts must declare their dependencies, validate inputs, produce actionable errors, and clean up temporary resources.
- Record third-party licenses and attribution for bundled code, templates, data, or assets.

## Plugin Packaging

- Plugins are distribution adapters, not the canonical source of reusable skill content and not a replacement for MCP service repositories.
- Customer-facing plugins should package selected `product-use` skills with the configuration needed to reach the public Gateway. Do not include `engineering` skills unless an explicitly separate internal plugin and catalog are approved.
- Keep each plugin self-contained because clients install plugins into caches. Plugin files must not reach outside the plugin root through relative paths.
- A cross-client plugin may share its `skills/`, assets, and other compatible payload while keeping platform manifests and catalogs explicit:
  - Codex manifest: `plugins/<plugin>/.codex-plugin/plugin.json`
  - Claude Code manifest: `plugins/<plugin>/.claude-plugin/plugin.json`
  - Codex catalog: `.agents/plugins/marketplace.json`
  - Claude Code catalog: `.claude-plugin/marketplace.json`
- Keep authentication material out of manifests and MCP configuration. Obtain credentials through the supported runtime authentication flow.
- Validate Codex and Claude Code packages independently. Compatibility on one client does not establish compatibility on the other.
- When the first plugin is implemented, define one packaging process that selects canonical skills, produces a self-contained plugin, and detects drift between source and packaged content.

## Development and Validation

- Derive commands from committed tooling and CI. Do not invent a validator, package manager, or release command that the repository has not configured.
- For documentation-only changes, review affected links and run `git diff --check`.
- Do not add placeholder CI that passes without validating a real skill or plugin artifact.
- `TODO: When the first skill lands, pin the skill validation tooling and document exact frontmatter, reference, and local discovery checks, including a non-installing skills CLI listing check.`
- `TODO: When a skill first includes scripts, document its language-specific formatting, linting, test, and dependency checks.`
- `TODO: When the first plugin lands, document exact Codex and Claude Code manifest, marketplace, packaging, and local-install smoke tests.`
- `TODO: When public skills are ready for discovery, add and validate skills.sh.json groupings for Engineering and Product Use.`
- `TODO: Before the first plugin release, define versioning, tag, changelog, and packaged-skill drift policies.`

## Git and GitHub Workflow

- Use a repository Issue as the implementation specification for a new skill, material skill revision, plugin, or publishing change.
- Use a Platform Initiative only when the change affects a cross-repository contract, architecture boundary, or coordinated release.
- Preserve unrelated work, stage only intended paths, and report checks that were actually run.
- Follow Conventional Commits.

## Canonical References

| Topic | Source |
|---|---|
| Portable skill format | [Agent Skills specification](https://agentskills.io/specification) |
| Repository discovery and installation | [skills CLI](https://github.com/vercel-labs/skills) |
| skills.sh repository-page grouping | [skills.sh customization](https://skills.sh/docs/customize) |
| Codex skill authoring | [Build skills](https://learn.chatgpt.com/docs/build-skills) |
| Codex plugin packaging | [Build plugins](https://learn.chatgpt.com/docs/build-plugins) |
| Claude Code plugin packaging | [Create plugins](https://code.claude.com/docs/en/plugins) |
| Claude Code plugin marketplaces | [Create and distribute a plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) |
| Platform architecture and public Gateway boundary | [XDenovo Platform architecture](https://github.com/XDenovo/platform/blob/main/docs/architecture.md) |
