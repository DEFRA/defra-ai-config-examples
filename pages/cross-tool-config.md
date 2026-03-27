---
layout: default
title: Using these examples with other AI tools
---

# Using these examples with other AI tools

The agents, instructions, prompts, and skills on this site are written for GitHub Copilot's configuration format, but the **content** — the rules, standards, and patterns — works with any AI coding tool. The rules themselves come from [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab), which remain the single source of truth regardless of which tool you use. This page shows how to adapt the examples for other tools.

## How the content maps across tools

Every AI coding tool has its own configuration format, but they all solve the same problem: giving the AI context about your project. The table below shows where each tool expects to find that context.

### GitHub Copilot

| Concept | Configuration |
|---------|--------------|
| **Root instructions** | `.github/copilot-instructions.md` |
| **Scoped instructions** | `.github/instructions/**/*.instructions.md` with `applyTo` frontmatter |
| **Agents** | `.github/agents/*.agent.md` |
| **Prompts** | `.github/prompts/*.prompt.md` |
| **Skills** | `.github/skills/{name}/SKILL.md` ([Agent Skills open standard](https://agentskills.io/specification){:target="_blank"}) (opens in new tab) |

### Claude Code

| Concept | Configuration |
|---------|--------------|
| **Root instructions** | `CLAUDE.md` in repo root |
| **Scoped instructions** | `CLAUDE.md` in subdirectories, or `.claude/rules/*.md` with `paths` frontmatter |
| **Agents** | `.claude/agents/` for custom subagents |
| **Prompts** | `.claude/commands/*.md` (custom slash commands) |
| **Skills** | Not directly supported — embed skill content in `CLAUDE.md` or `.claude/rules/` files |

### Cursor

| Concept | Configuration |
|---------|--------------|
| **Root instructions** | `.cursor/rules/*.mdc` with `alwaysApply: true` |
| **Scoped instructions** | `.cursor/rules/*.mdc` with `globs` frontmatter |
| **Agents** | Not supported — use rules instead |
| **Prompts** | Not supported — paste into chat |
| **Skills** | Not supported — merge skill content into `.cursor/rules/*.mdc` files |

### Windsurf

| Concept | Configuration |
|---------|--------------|
| **Root instructions** | `.windsurf/rules/*.md` with Always On trigger (legacy: `.windsurfrules`) |
| **Scoped instructions** | `.windsurf/rules/*.md` with Glob trigger |
| **Agents** | Supports `AGENTS.md` |
| **Prompts** | Not supported — paste into chat |
| **Skills** | Not supported — merge skill content into `.windsurf/rules/` files |

## Adapting the examples

### Claude Code

Claude Code reads project context from `CLAUDE.md` files and `.claude/` configuration. To use the Defra examples:

1. Create a `CLAUDE.md` in your repo root with the content from [copilot-instructions.md]({{ "/pages/instructions/copilot-instructions" | relative_url }})
2. Append the relevant backend instructions ([Node.js]({{ "/pages/instructions/node-backend" | relative_url }}) or [C#]({{ "/pages/instructions/csharp-backend" | relative_url }}))
3. Append the [frontend instructions]({{ "/pages/instructions/frontend" | relative_url }}) if your service has a UI
4. Replace `applyTo` frontmatter with `paths` frontmatter in any `.claude/rules/*.md` files

For scoped rules, you have two options:

**Option A — subdirectory `CLAUDE.md` files** (simpler):

```
your-repo/
├── CLAUDE.md                    # Root instructions (always active)
├── src/
│   └── CLAUDE.md                # Backend-specific rules
└── views/
    └── CLAUDE.md                # Frontend-specific rules
```

**Option B — `.claude/rules/` with `paths` frontmatter** (more precise):

```
your-repo/
├── CLAUDE.md                    # Root instructions
└── .claude/
    └── rules/
        ├── node-backend.md      # Scoped to JS/MJS files
        └── frontend.md          # Scoped to Nunjucks/SCSS files
```

Each rule file uses `paths` frontmatter to target specific file patterns:

```yaml
---
paths:
  - "**/*.js"
  - "**/*.mjs"
---
```

**Agent content**: Claude Code supports custom subagents via `.claude/agents/`. Create agent definitions here to replicate the behaviour of the [Defra App Developer agent]({{ "/pages/agents/defra-app-developer" | relative_url }}).

**Prompt content**: Create `.claude/commands/*.md` — each file becomes a reusable command invoked as `/project:command-name`. Use `$ARGUMENTS` as a placeholder for dynamic input.

### Cursor

Cursor uses rule files in `.cursor/rules/`. To use the Defra examples:

1. Create `.cursor/rules/` in your repository root
2. Copy the root instructions into a `.mdc` file with `alwaysApply: true`
3. Copy scoped instructions into separate files with `globs` frontmatter
4. Update the frontmatter from `applyTo` to `globs`:

**Copilot format:**
```yaml
---
applyTo: "**/*.js,**/*.mjs"
---
```

**Cursor format:**
```yaml
---
description: Node.js backend rules
globs: "**/*.js,**/*.mjs"
alwaysApply: false
---
```

Cursor supports four rule types controlled by frontmatter:

| Rule type | Frontmatter | Behaviour |
|-----------|-------------|-----------|
| **Always** | `alwaysApply: true` | Always included in context (root instructions) |
| **Auto Attached** | `globs: "pattern"` | Included when matching files are referenced |
| **Agent Requested** | `description: "..."` only | AI reads description and decides whether to include |
| **Manual** | No description, no globs | Only included when explicitly referenced with `@rulename` |

**Agent and prompt content**: Cursor does not have separate agent or prompt files. Merge agent rules into your rule files and paste prompt templates into the chat when needed.

### Windsurf

Windsurf uses a rules directory at `.windsurf/rules/`. To use the Defra examples:

1. Create `.windsurf/rules/` in your repository root
2. Create an always-on rule file with root instructions
3. Create scoped rule files with Glob triggers for backend and frontend rules
4. Set the appropriate trigger type in each file's frontmatter

**Windsurf rule frontmatter:**

```yaml
---
trigger: glob
globs: ["**/*.js", "**/*.mjs"]
description: Node.js backend rules
---
```

Windsurf supports four trigger types:

| Trigger | Behaviour |
|---------|-----------|
| `always_on` | Always included in context (root instructions) |
| `glob` | Included when matching files are referenced (scoped instructions) |
| `model_decision` | AI reads description and decides whether to include |
| `manual` | Only included when explicitly referenced by the user |

**Legacy**: `.windsurfrules` in the repo root still works as a single-file alternative. Use it if your team does not need scoped rules.

**Agent content**: Windsurf supports `AGENTS.md` for defining agent personas.

**Prompt content**: Windsurf does not have a prompt file system. Paste prompt templates into the chat when needed.

### Skills across tools

GitHub Copilot is currently the only tool that supports the [Agent Skills open standard](https://agentskills.io/specification){:target="_blank"} (opens in new tab). For other tools, convert each skill's content into their native rule format:

| Tool | How to adapt skills |
|------|--------------------|
| **Claude Code** | Copy the skill's markdown body into a `.claude/rules/{skill-name}.md` file. Use `paths` frontmatter if the skill targets specific file types. |
| **Cursor** | Copy the skill's markdown body into a `.cursor/rules/{skill-name}.mdc` file. Set `alwaysApply: false` and add a `description` so Cursor auto-selects it when relevant. |
| **Windsurf** | Copy the skill's markdown body into a `.windsurf/rules/{skill-name}.md` file. Use `trigger: model_decision` so the AI decides when to include it. |

The skill content (Defra standards, GOV.UK accessibility rules, PII handling) transfers directly — only the file location and frontmatter format change. See the [Skills examples]({{ "/pages/skills" | relative_url }}) for the source content.

## What stays the same across tools

The configuration format changes between tools but the underlying rules — Defra coding standards, GOV.UK Design System, WCAG 2.2 AA, OWASP practices, SonarCloud quality gates, 90% test coverage, and no PII in logs — are identical everywhere.

## Multi-tool setup

If your team uses multiple AI tools, maintain canonical rules in `.github/copilot-instructions.md` and copy the same content into `CLAUDE.md`, `.cursor/rules/`, or `.windsurf/rules/` as needed. Use the [scaffold prompt]({{ "/pages/prompts/scaffold-repo" | relative_url }}) to generate the initial config, then update the canonical source when rules change.

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
