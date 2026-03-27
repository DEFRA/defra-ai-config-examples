---
layout: default
title: Skills — Examples for GitHub Copilot
---

# Skills — Examples for GitHub Copilot

Agent Skills are reusable capability packages that agents discover and load automatically. Unlike Instructions (which are always injected) or Agents (which you invoke explicitly), a Skill activates itself when Copilot decides it is relevant to the current task — based on a short description you write in the skill file.

Skills follow the [Agent Skills open standard](https://agentskills.io/specification){:target="_blank"} (opens in new tab) and work across multiple AI coding assistants — not just GitHub Copilot.

## How skills differ from instructions and agents

| Feature | Instructions | Agents | Skills |
|---------|-------------|--------|--------|
| How it activates | Always on, or by `applyTo` glob | Explicitly invoked with `@agent-name` | Automatically, based on description relevance |
| Lives in | `.github/instructions/` | `.github/agents/` | `.github/skills/{name}/` |
| Can include sub-resources | No | No | Yes — scripts, reference files, examples |
| Primary purpose | Passive rules injected into context | A persona with a workflow | Domain expertise packaged for reuse |
| Open cross-tool standard | No | No | Yes — `agentskills.io` |

## How Copilot discovers skills

When you start a task in agent mode, Copilot scans `.github/skills/` for `SKILL.md` files and reads each skill's `description` field. If the description matches what you are working on, Copilot loads the skill's content into context — no manual invocation needed.

Write the `description` to match the keywords that will appear in your prompts. A specific description ("Apply Defra security and PII handling rules when reviewing route handlers or logging code") activates more reliably than a vague one ("Security stuff").

## How to enable skills in VS Code

Enable the **Chat: Use Agent Skills** setting in VS Code:

1. Open Settings (`Ctrl+,` on Windows/Linux, `Cmd+,` on macOS)
2. Search for `chat.agent.skills`
3. Enable the setting

You can also set this in your workspace `.vscode/settings.json`:

```json
{
  "chat.agent.skills": true
}
```

## SKILL.md format

Each skill lives in its own directory. The directory name must match the `name` field in the YAML frontmatter:

```
.github/skills/
└── defra-standards/
    ├── SKILL.md             # Required — frontmatter + guidance content
    └── references/          # Optional sub-resources (scripts, examples, docs)
        └── approved-packages.md
```

The `SKILL.md` file uses YAML frontmatter followed by markdown:

```markdown
---
name: defra-standards
description: Defra software development standards for Node.js and .NET government services. Use when writing, reviewing, or refactoring code in a Defra digital service.
license: OGL-UK-3.0
metadata:
  author: defra-digital
  version: "1.0"
---

# Defra Standards

## Rules
- One rule per line
- Include examples where helpful
```

### Frontmatter fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Max 64 characters. Lowercase letters, numbers, and hyphens. Must match the directory name. |
| `description` | Yes | Max 1024 characters. What the skill does and when to use it. Include keywords that match likely prompts. |
| `license` | No | Licence name. Use `OGL-UK-3.0` for Defra Open Government Licence content. |
| `metadata` | No | Author, version, or any other key-value pairs. |
| `compatibility` | No | Environment requirements — e.g. `Node.js 22+` or `.NET 9+`. |

**Keep `SKILL.md` under 500 lines.** Move detailed reference material to separate files in the skill directory and reference them from `SKILL.md`.

## Personal and global skills

Skills work at two scopes:

| Scope | Location | Shared with team? |
|-------|----------|-------------------|
| Project / repository | `.github/skills/{name}/SKILL.md` | Yes — committed to the repo |
| Personal | `~/.copilot/skills/{name}/SKILL.md` | No — local to your machine |

Personal skills are useful for coding preferences that apply across all your projects. Project skills are for standards and patterns specific to one service.

## Cross-tool compatibility

Skills follow the `agentskills.io` open standard and work with any compatible AI assistant:

| AI Coding Assistant | Project-level path |
|---------------------|--------------------|
| GitHub Copilot | `.github/skills/` |
| Claude Code | `.claude/skills/` |
| Cursor | `.cursor/skills/` |
| Codex CLI | `.codex/skills/` |
| Gemini CLI | `.gemini/skills/` |

## Available examples

| Skill | Activates when | Purpose |
|-------|---------------|---------|
| [Defra Standards]({{ "/pages/skills/defra-standards" | relative_url }}) | Working on a Defra Node.js or .NET service | CDP conventions, approved libraries, naming patterns, Docker base images |
| [GOV.UK Accessibility]({{ "/pages/skills/govuk-accessibility" | relative_url }}) | Working on Nunjucks templates, HTML, or SCSS | WCAG 2.2 AA guidance, GOV.UK Frontend patterns, common failure patterns |
| [Security and PII]({{ "/pages/skills/security-and-pii" | relative_url }}) | Writing or reviewing route handlers, service functions, or logging | PII categories, OWASP, parameterised queries, UK data classifications |

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
