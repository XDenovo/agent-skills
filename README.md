# XDenovo Agent Skills

Reusable agent skills for XDenovo engineering and for agents that use published XDenovo products.

## Skill Catalogs

This repository contains two catalogs with different audiences:

| Catalog | Audience | Purpose |
|---|---|---|
| `engineering` | XDenovo team members | Reusable development, testing, release, deployment, and operational workflows installed into XDenovo repositories. |
| `product-use` | Users of published XDenovo products | Workflows that teach agents how to use public XDenovo MCP capabilities and other supported product scenarios. |

Engineering skills are intended to be installed by XDenovo developers into the repository where they are working. They complement that repository's own instructions and do not replace its `AGENTS.md`, manifests, scripts, or CI configuration.

Product-use skills are written for external users and their agents. They depend only on public product behavior, documentation, authentication, and endpoints; they do not require access to XDenovo source repositories or internal infrastructure.

## Repository Layout

```text
skills/
  engineering/
    <skill-name>/
      SKILL.md
  product-use/
    <skill-name>/
      SKILL.md
```

Each skill follows the [Agent Skills specification](https://agentskills.io/specification) and keeps its instructions, scripts, references, and assets within its own directory.

## Install Skills

List the skills published by this repository:

```bash
npx skills add XDenovo/agent-skills --list
```

Install one skill into the current project:

```bash
npx skills add XDenovo/agent-skills --skill <skill-name>
```

Choose a specific supported agent when needed:

```bash
npx skills add XDenovo/agent-skills --skill <skill-name> --agent codex
npx skills add XDenovo/agent-skills --skill <skill-name> --agent claude-code
```

See the [skills CLI documentation](https://github.com/vercel-labs/skills) for global installation, non-interactive installation, and additional supported agents.

## Product Plugins

When XDenovo's public MCP products are released, selected `product-use` skills may also be distributed as Codex and Claude Code plugins. These plugins will combine user-facing workflows with the configuration needed to connect through the public XDenovo Gateway at `https://api.xdenovoai.com/`.

Plugins are a distribution layer for skills and MCP connection configuration. The Gateway and Compute MCP Service implementations remain in their owning XDenovo repositories.

Engineering skills are not included in customer-facing plugins by default. Any future team-only plugin will be packaged and distributed separately.

## Contributing

Open an Issue before adding a new skill, materially changing an existing workflow, or introducing a plugin. Read [`AGENTS.md`](AGENTS.md) for catalog boundaries, authoring requirements, validation expectations, and XDenovo platform constraints.
