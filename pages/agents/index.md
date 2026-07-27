---
layout: default
title: Agents — Examples for GitHub Copilot
---

# Agents — Examples for GitHub Copilot

Agents are specialised AI personas you can invoke in GitHub Copilot Chat. Each agent has a defined role, expertise, and workflow that shapes how Copilot responds.

## How agents work

When you select an agent in Copilot Chat (using `@agent-name`), Copilot adopts that persona for the conversation. The agent's instructions override the default behaviour, giving you focused, role-specific assistance.

Agent files live in `.github/agents/` and use the `.agent.md` extension.

## How to use these examples

1. Copy the `.agent.md` file into your repository's `.github/agents/` directory
2. Edit any project-specific details (service name, tech stack, team conventions)
3. Open Copilot Chat and select the agent from the agent picker

> **Keep agents current.** Defra standards evolve. Revisit agent files quarterly or when the [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) repository is updated.

## Available examples

| Agent | Purpose | Best for |
|-------|---------|----------|
| [Defra App Developer]({{ "/pages/agents/defra-app-developer" | relative_url }}) | Builds Defra-compliant applications | Writing new features, implementing user stories |
| [Code Reviewer]({{ "/pages/agents/code-reviewer" | relative_url }}) | Systematic code review | Reviewing PRs, checking standards compliance |
| [Tester]({{ "/pages/agents/tester" | relative_url }}) | BDD-focused testing and Playwright journey tests | Writing unit tests, acceptance tests, journey tests, coverage gaps |
| [Accessibility Advisor]({{ "/pages/agents/accessibility-advisor" | relative_url }}) | WCAG 2.2 AA and GOV.UK Design System compliance | Reviewing or fixing UI templates, forms, and components |
| [Orchestrator]({{ "/pages/agents/orchestrator" | relative_url }}) | Plans and delegates complex tasks to specialist agents | Large features, multi-phase work, multi-agent workflows |
| [Repo Setup]({{ "/pages/agents/repo-setup" | relative_url }}) | Interviews you about your project and generates the full Copilot config | Setting up a new repo, adding Copilot to an existing project |
| [LAP Inovation documentation]({{ "/pages/agents/LAP-Inovation-documentation.agent" | relative_url }}) | Produces factual, evidence-based system documentation (HLD, LLD, ADRs, Runbook) for the current codebase. No refactoring | Producing Documentation |
| [LAP Inovation implementation]({{ "/pages/agents/LAP-Inovation-implementation.agent" | relative_url }}) | Implements one approved migration slice per pull request, aligned to the intelligent migration programme, with tests, documentation, and rollback awareness | Implements Migration per pull request |
| [LAP Inovation Intelligent Migration]({{ "/pages/agents/LAP-Inovation-intelligent-migration.agent" | relative_url }}) | Designs and governs an end-to-end intelligent application migration programme, including team model, phased roadmap, risk controls, and success metrics. | Design and Governance usecases |
| [LAP Inovation Modernise to modular monolith]({{ "/pages/agents/LAP-Inovation-modernise-to-modular-monolith" | relative_url }}) | Proposes a target architecture and an incremental, reversible migration plan from the current monolith. | Target Architecture roposal |
| [LAP Inovation Testing]({{ "/pages/agents/LAP-Inovation-testing.agent" | relative_url }}) | Establishes a baseline test strategy and automated tests to protect existing behaviour; adds CI. | Test Strategy Usecases |
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
