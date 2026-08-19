---
layout: default
title: LAP Implementations Agents - Examples for GitHub Copilot
---

# LAP Implementations Agents - Examples for GitHub Copilot

Legacy Application Modernisation (LAP) agents are specialised AI personas you can invoke in GitHub Copilot Chat to support the safe, incremental modernisation of legacy applications. Each agent has a defined role, expertise, and workflow that shapes how Copilot responds.

## How agents work

When you select an agent in Copilot Chat (using `@agent-name`), Copilot adopts that persona for the conversation. The agent's instructions override the default behaviour, giving you focused, role-specific assistance.

Agent files live in `.github/agents/` and use the `.agent.md` extension.

## How to use these examples

1. Copy the `.agent.md` file into your repository's `.github/agents/` directory
2. Edit any project-specific details (service name, tech stack, team conventions)
3. Open Copilot Chat and select the agent from the agent picker

> **Keep agents current.** Defra standards evolve. Revisit agent files quarterly or when the [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) repository is updated.

## Available examples

| Agent | Purpose |
|-------|---------|
| [LAP Innovation Documentation]({{ "/pages/agents/lap-gitHub-copilot/lap-innovation-documentation" | relative_url }}) | Produces factual, evidence-based system documentation (HLD, LLD, ADRs, Runbook) for the current codebase. No refactoring. |

| [LAP Innovation Implementation]({{ "/pages/agents/lap-gitHub-copilot/lap-innovation-implementation" | relative_url }}) | Implements one approved migration slice per pull request, aligned to the intelligent migration programme, with tests, documentation, and rollback awareness. |

| [LAP Innovation Intelligent Migration]({{ "/pages/agents/lap-gitHub-copilot/lap-innovation-intelligent-migration" | relative_url }}) | Establish a repeatable, low-risk migration operating model that increases delivery success probability using AI-augmented teams. |

| [LAP Inovation Modernise to Modular Monolith]({{ "/pages/agents/lap-gitHub-copilot/lap-innovation-modernise-to-modular-monolith" | relative_url }}) | Design a realistic modernisation path that can be executed safely and incrementally. |

| [LAP Innovation Testing]({{ "/pages/agents/lap-gitHub-copilot/lap-innovation-testing" | relative_url }}) | Create a safety net that enables confident, incremental modernisation. |

## Writing your own agents

Follow this structure:

```markdown
---
description: One-line summary of what this agent does
tools: [edit, execute, read, search, web, todos, thinking]
---

# Agent Name

## Role
What this agent is and its expertise.

## Workflow
1. Step one
2. Step two
3. Step three

## Rules
- What the agent must always do
- What the agent must never do

## References
- Links to standards or instruction files this agent follows
```

**Tips:**
- Keep the description under 100 characters — it appears in the agent picker
- Prefer **tool sets** (`edit`, `execute`, `read`, `search`, `web`, `agent`) over individual tool names — each set bundles all the related built-in tools so the agent has full Agent-mode capability without a long brittle list
- Add `todos` for any multi-step agent — it gives the agent (and you) a visible progress checklist
- For read-only review agents, omit `edit` and `execute` to enforce the principle of least privilege
- VS Code [silently ignores unknown tool names](https://code.visualstudio.com/docs/copilot/customization/custom-agents#_custom-agent-file-structure){:target="_blank"} (opens in new tab), so MCP and extension tools can be listed safely
- Reference instruction files rather than restating rules
- Define clear boundaries — what the agent does and does not do

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
