# GitHub Copilot Example Setup for Defra

Practical setup guidance for configuring GitHub Copilot in Defra projects. This site provides ready-to-use agents, skills, instructions, and reusable prompts that help Copilot **enforce** [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the single source of truth for how Defra teams write code. These configuration files do not replace or override those standards; they encode them so Copilot produces compliant code from the start.

> **New to AI in software development?** Read the [Defra AI in the SDLC playbook](https://defra.github.io/defra-ai-sdlc/){:target="_blank"} (opens in new tab) first. It covers the [four pillars](https://defra.github.io/defra-ai-sdlc/pages/getting-started/the-four-pillars){:target="_blank"} (opens in new tab) of working effectively with AI — particularly **Rules and Instructions** and **Prompts** — which this site implements with ready-to-use configuration files. For tool-specific privacy and compliance guidance, see the [AI Tool Guidance site](https://defra.github.io/ai-sdlc-tool-guidance/){:target="_blank"} (opens in new tab).

## Why this matters

AI coding tools work best when given clear, project-specific context. A well-configured GitHub Copilot will:

- Follow your team's coding standards and conventions automatically
- Produce code that passes your quality gates first time
- Understand your architecture and make consistent decisions
- Save time on repetitive tasks like writing tests, ADRs, and documentation

## What is in this guide

### [Getting Started](pages/getting-started.md)

The main setup guide — explains GitHub Copilot's configuration system and how to set up your repository with instructions, agents, prompts, and skills. Includes a quick start with the three essential files to get you running.

### [Agents](pages/agents/index.md)

Specialised AI personas you invoke in Copilot Chat. Each agent has a defined role, expertise, and workflow.

- [Defra App Developer](pages/agents/defra-app-developer.md) — builds Defra-compliant applications following all software development standards
- [Code Reviewer](pages/agents/code-reviewer.md) — systematic code review using Defra quality criteria
- [Tester](pages/agents/tester.md) — BDD-focused testing with coverage enforcement and Playwright journey tests
- [Accessibility Advisor](pages/agents/accessibility-advisor.md) — WCAG 2.2 AA and GOV.UK Design System compliance for UI code
- [Orchestrator](pages/agents/orchestrator.md) — plans complex features and delegates phases to specialist agents
- [Repo Setup](pages/agents/repo-setup.md) — interviews you about your project and generates the full Copilot configuration

### [Instructions](pages/instructions/index.md)

Project-specific rules and standards that Copilot follows automatically.

- [Root instructions](pages/instructions/copilot-instructions.md) — project overview, branching, commits, quality gates
- [Node.js backend](pages/instructions/node-backend.md) — Hapi, vanilla JavaScript, logging, error handling
- [C# backend](pages/instructions/csharp-backend.md) — ASP.NET Core Minimal APIs, Serilog, FluentValidation, xUnit
- [Python backend](pages/instructions/python-backend.md) — FastAPI, Pydantic Settings, async MongoDB, pytest
- [Frontend](pages/instructions/frontend.md) — WCAG 2.2 AA, GOV.UK Design System, Nunjucks
- [Documentation](pages/instructions/docs.md) — READMEs, ADRs, JSDoc, inline comment discipline
- [Testing](pages/instructions/testing.md) — Jest, BDD naming, AAA pattern, coverage targets, mock discipline
- [Real-world example](pages/instructions/real-world-example.md) — complete production instructions for a Node.js/Hapi service

### [Prompts](pages/prompts/index.md)

Reusable prompt templates for common tasks.

- [Write ADR](pages/prompts/write-adr.md) — generate architecture decision records
- [Write Tests](pages/prompts/write-tests.md) — generate test suites for existing code
- [Security Review](pages/prompts/security-review.md) — review code against OWASP and Defra security standards
- [Scaffold Repo](pages/prompts/scaffold-repo.md) — generate the full `.github/` config, skills, `.copilotignore`, and MCP setup for a new project
- [Generate PR Description](pages/prompts/generate-pr-description.md) — write a structured PR description from staged changes
- [Write Runbook](pages/prompts/write-runbook.md) — generate an operational runbook for a service
- [Refactor to Standards](pages/prompts/refactor-to-standards.md) — audit code against Defra standards and produce a prioritised refactor plan

### [Skills](pages/skills/index.md)

Reusable capability packages that agents discover and load automatically based on what you are working on.

- [Defra Standards](pages/skills/defra-standards.md) — Node.js and .NET coding standards, approved packages, and CDP platform conventions
- [GOV.UK Accessibility](pages/skills/govuk-accessibility.md) — WCAG 2.2 AA guidance, GOV.UK Frontend macros, and common failure patterns
- [Security and PII](pages/skills/security-and-pii.md) — UK data classifications, PII handling, parameterised queries, and OWASP guidance

### [MCP Setup](pages/mcp-setup.md)

How to configure MCP servers to extend what Copilot agents can do — connecting them to GitHub, Azure DevOps, Playwright, and other tools.

### [.copilotignore and Advanced Configuration](pages/copilotignore.md)

Content exclusion, model pinning, enabling Agent Skills, and Plan Mode.

### [Cross-Tool Config](pages/cross-tool-config.md)

How to adapt the examples for other AI tools — Claude Code, Cursor, Windsurf, and others. The rules and standards are the same; only the file format changes.

## How to use the examples

1. Browse the [Getting Started](pages/getting-started.md) guide to understand the configuration structure
2. Navigate to the example files that match your project's needs
3. Copy the raw markdown content into the matching directory in your repository
4. Edit the examples to fit your specific project context

## Quick start — minimum viable setup

Copy four files into your repo's `.github/` directory and you are up and running. See the [Getting Started guide](pages/getting-started.md) for the full setup table, what to edit in each file, and how to add more configuration as your team's needs grow.

## Related resources

- [Defra AI in the SDLC playbook](https://defra.github.io/defra-ai-sdlc/){:target="_blank"} (opens in new tab) — foundational guidance on AI in software development
- [AI Tool Guidance](https://defra.github.io/ai-sdlc-tool-guidance/){:target="_blank"} (opens in new tab) — privacy, compliance, and data handling assessments for AI tools
- [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the guardrails encoded in these examples
- [GitHub Copilot customisation docs](https://docs.github.com/en/copilot/customizing-copilot){:target="_blank"} (opens in new tab) — official GitHub documentation

## Contact us

The Defra AI Capabilities and Enablement team maintains this site. Contact us:

- **Microsoft Teams**: Defra AI Community
- **Email**: AICapabilitiesEnablement@defra.gov.uk

## Licence

All content is available under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/){:target="_blank"} (opens in new tab), except where otherwise stated.
