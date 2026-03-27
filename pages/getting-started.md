---
layout: default
title: GitHub Copilot — Setup Guide
---

# GitHub Copilot — Setup Guide

This guide explains how to configure GitHub Copilot for Defra projects using agents, skills, instructions, and reusable prompts.

> **Single source of truth:** [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) are the authority for how Defra teams write code. The configuration files on this site help Copilot **enforce** those standards — they do not replace or override them. When a standard changes, update your configuration files to match.

## Quick start — minimum viable setup

Copy these four files into your repo's `.github/` directory and you are up and running:

| Step | File to copy | Save to | What to edit |
|------|-------------|---------|--------------|
| 1 | [Root instructions]({{ "/pages/instructions/copilot-instructions" | relative_url }}) | copilot-instructions.md | Replace the project overview with your service name, tech stack, and paths |
| 2 | [Defra App Developer]({{ "/pages/agents/defra-app-developer" | relative_url }}) | agents/ | Update the tech stack section if you use .NET instead of Node.js |
| 3 | [Node.js]({{ "/pages/instructions/node-backend" | relative_url }}), [C#]({{ "/pages/instructions/csharp-backend" | relative_url }}), or [Frontend]({{ "/pages/instructions/frontend" | relative_url }}) | instructions/ | Pick the file matching your tech stack — activates automatically for matching files |
| 4 | [Code Reviewer]({{ "/pages/agents/code-reviewer" | relative_url }}) | agents/ | Ready to use — no edits needed |

That gives you Defra-compliant code generation, tech-stack-specific rules that fire automatically, a pre-commit checklist, and a 9-category review checklist before every PR. Add more files from the sections below as your team's needs grow.

## Set up everything at once

Instead of copying files one by one, generate the full configuration in a single step:

| Approach | How it works | Best for |
|----------|-------------|----------|
| [Scaffold Repo prompt]({{ "/pages/prompts/scaffold-repo" | relative_url }}) | Fill in your project details upfront, run the prompt, get all files generated in one pass | Teams who know their tech stack and want speed |
| [Repo Setup agent]({{ "/pages/agents/repo-setup" | relative_url }}) | The agent interviews you about your project then generates tailored config files | Teams who want guided help choosing the right configuration |

Both approaches generate the same output: root instructions, scoped instructions, agents, prompts, skills, `.copilotignore`, and MCP server config — all tailored to your project.

## How GitHub Copilot uses configuration files

GitHub Copilot reads configuration files from your repository's `.github/` directory. These files give Copilot project-specific context that improves the quality and relevance of its suggestions.

There are five types of configuration:

| Type | Location | Purpose |
|------|----------|---------|
| **Instructions** | `.github/copilot-instructions.md` and `.github/instructions/*.instructions.md` | Project-wide rules, standards, and conventions that always apply |
| **Agents** | `.github/agents/*.agent.md` | Specialised personas with specific expertise and workflows |
| **Prompts** | `.github/prompts/*.prompt.md` | Reusable prompt templates for common tasks |
| **Skills** | `.github/skills/{name}/SKILL.md` | Reusable capability packages that agents discover and load automatically |
| **Content exclusion** | `.copilotignore` in repo root | Files and directories that Copilot must not use as context |

## Recommended directory structure

```
your-repo/
├── .copilotignore                            # Files excluded from Copilot context
├── .github/
│   ├── copilot-instructions.md              # Root instructions (always active)
│   ├── agents/
│   │   ├── defra-app-developer.agent.md     # Defra-compliant development
│   │   ├── code-reviewer.agent.md           # Systematic code review
│   │   ├── tester.agent.md                  # BDD-focused testing
│   │   ├── accessibility-advisor.agent.md   # WCAG 2.2 AA compliance
│   │   └── orchestrator.agent.md            # Multi-agent workflow planning
│   ├── instructions/
│   │   ├── node-backend.instructions.md     # Node.js/Hapi backend rules
│   │   ├── csharp-backend.instructions.md   # C#/ASP.NET Core Minimal API rules
│   │   ├── frontend.instructions.md         # Accessibility, GDS patterns
│   │   ├── docs.instructions.md             # Documentation standards
│   │   └── testing.instructions.md          # Test structure, coverage, mocking
│   ├── prompts/
│   │   ├── write-adr.prompt.md              # Generate architecture decisions
│   │   ├── write-tests.prompt.md            # Generate test suites
│   │   ├── security-review.prompt.md        # Review code for vulnerabilities
│   │   ├── generate-pr-description.prompt.md # Write PR descriptions
│   │   ├── write-runbook.prompt.md          # Generate operational runbook
│   │   └── refactor-to-standards.prompt.md  # Audit and prioritise tech debt
│   └── skills/
│       ├── defra-standards/
│       │   └── SKILL.md                     # Defra coding standards and conventions
│       ├── govuk-accessibility/
│       │   └── SKILL.md                     # WCAG 2.2 AA and GOV.UK Frontend rules
│       └── security-and-pii/
│           └── SKILL.md                     # Security, PII, and OWASP guidance
└── .vscode/
    └── mcp.json                             # MCP server configuration
```

## Instructions

Instructions tell Copilot how your project works. The root file (`.github/copilot-instructions.md`) is always active — Copilot reads it for every interaction. Additional instruction files in `.github/instructions/` are scoped by glob pattern and activate only for matching files.

**Key principles for writing instructions:**
- One file per concern (backend, frontend, testing, docs)
- Use imperative phrasing — "Use Hapi for HTTP servers" not "Hapi should be used"
- Include one short example and one counterexample per rule
- Link to canonical sources rather than duplicating content

### Example instruction files

- [Root copilot-instructions.md]({{ "/pages/instructions/copilot-instructions" | relative_url }}) — branching, commits, quality gates, project overview
- [Node.js backend instructions]({{ "/pages/instructions/node-backend" | relative_url }}) — Hapi, vanilla JavaScript, logging, error handling
- [Frontend instructions]({{ "/pages/instructions/frontend" | relative_url }}) — WCAG 2.2 AA, GDS Design System, Nunjucks, no JS frameworks
- [C# backend instructions]({{ "/pages/instructions/csharp-backend" | relative_url }}) — ASP.NET Core Minimal APIs, Serilog, FluentValidation, xUnit
- [Documentation instructions]({{ "/pages/instructions/docs" | relative_url }}) — READMEs, ADRs, JSDoc, inline comment discipline
- [Testing instructions]({{ "/pages/instructions/testing" | relative_url }}) — Jest, BDD naming, AAA pattern, coverage targets, mock discipline
- [Real-world example]({{ "/pages/instructions/real-world-example" | relative_url }}) — complete instructions file for a Node.js/Hapi/Nunjucks GOV.UK service

## Agents

Agents are specialised AI personas you invoke in Copilot Chat. Each agent has a defined role, expertise area, and workflow. Use agents when you want Copilot to adopt a specific mindset — for example, reviewing code with security in mind, or writing tests using BDD patterns.

**Key principles for writing agents:**
- Give each agent a clear role and boundaries
- Reference instruction files rather than restating rules
- Define the agent's workflow as a numbered sequence
- State what the agent must not do

### Example agent files

- [Defra App Developer]({{ "/pages/agents/defra-app-developer" | relative_url }}) — builds Defra-compliant applications following all software development standards
- [Code Reviewer]({{ "/pages/agents/code-reviewer" | relative_url }}) — systematic review using Defra's quality criteria
- [Tester]({{ "/pages/agents/tester" | relative_url }}) — BDD-focused unit/integration testing and Playwright journey tests with coverage enforcement
- [Accessibility Advisor]({{ "/pages/agents/accessibility-advisor" | relative_url }}) — WCAG 2.2 AA and GOV.UK Design System compliance for UI code
- [Orchestrator]({{ "/pages/agents/orchestrator" | relative_url }}) — plans complex features and delegates phases to specialist agents
- [Repo Setup]({{ "/pages/agents/repo-setup" | relative_url }}) — interviews you about your project and generates the full Copilot configuration

## Prompts

Prompts are reusable templates for common tasks. Unlike instructions (which are always on) or agents (which set a persona), prompts are one-shot — you run them when you need a specific output, like generating an ADR or reviewing for security vulnerabilities.

**Key principles for writing prompts:**
- Start with a clear output description
- Include the expected file format and save location
- Reference relevant instruction files for standards
- Add variables (placeholders) for context the user provides

### Example prompt files

- [Write ADR]({{ "/pages/prompts/write-adr" | relative_url }}) — generate architecture decision records
- [Write Tests]({{ "/pages/prompts/write-tests" | relative_url }}) — generate test suites for existing code
- [Security Review]({{ "/pages/prompts/security-review" | relative_url }}) — review code against OWASP and Defra security standards
- [Scaffold Repo]({{ "/pages/prompts/scaffold-repo" | relative_url }}) — generate the full `.github/` config, skills, `.copilotignore`, and MCP setup for a new Defra project
- [Generate PR Description]({{ "/pages/prompts/generate-pr-description" | relative_url }}) — write a structured PR description from staged changes
- [Write Runbook]({{ "/pages/prompts/write-runbook" | relative_url }}) — generate an operational runbook for a service
- [Refactor to Standards]({{ "/pages/prompts/refactor-to-standards" | relative_url }}) — audit code against Defra standards and produce a prioritised refactor plan

## Skills

Agent Skills are reusable capability packages stored in `.github/skills/`. They differ from instruction files in a key way: Copilot discovers and loads them automatically based on the content of your messages, without you needing to name or invoke them.

Enable skills in VS Code by adding `"chat.agent.skills": true` to your user settings. Once enabled, Copilot reads the `description` frontmatter of each skill and loads it when the description matches what you are working on.

See [Agent Skills]({{ "/pages/skills" | relative_url }}) for a full explanation, the SKILL.md format, and examples.

### Example skill files

- [Defra standards]({{ "/pages/skills/defra-standards" | relative_url }}) — Node.js and .NET coding standards, approved packages, and CDP platform conventions
- [GOV.UK accessibility]({{ "/pages/skills/govuk-accessibility" | relative_url }}) — WCAG 2.2 AA guidance, GOV.UK Frontend macros, and common failure patterns
- [Security and PII]({{ "/pages/skills/security-and-pii" | relative_url }}) — UK data classifications, PII handling, parameterised queries, and OWASP guidance

## MCP Setup

MCP servers extend what Copilot agents can do by connecting them to external tools — GitHub issues, Azure DevOps, Playwright tests, and more. Configure them in `.vscode/mcp.json`. See [MCP Setup]({{ "/pages/mcp-setup" | relative_url }}) for configuration instructions and Defra's approved server list.

## Content exclusion

A `.copilotignore` file in your repo root tells Copilot which files it must not use as context. Use gitignore syntax. On government projects, exclude environment files containing real credentials, private keys, test fixtures with real personal data, and any OFFICIAL-SENSITIVE documents. See [.copilotignore and advanced configuration]({{ "/pages/copilotignore" | relative_url }}) for a recommended exclusion list and guidance on model pinning and Plan Mode.

## Using these examples with other AI tools

The rules and standards in these examples work with any AI coding tool — only the file format changes. See [Using these examples with other AI tools]({{ "/pages/cross-tool-config" | relative_url }}) for how to adapt them for Claude Code, Windsurf, and others.

## Getting started

1. Copy the `.github/` directory structure into your repository
2. Start with the root `copilot-instructions.md` — edit the project overview section for your service
3. Add the instruction files that match your tech stack (Node.js backend, frontend, or both)
4. Add agents that match your team's workflow
5. Add prompts for tasks your team does repeatedly
6. Test by opening Copilot Chat and asking it about your project — it should reference the standards

## Tips for effective configuration

- **Keep files short.** Copilot works best with concise, specific instructions. Aim for under 200 lines per file.
- **Use the SSOT pattern.** Define each rule in one place and reference it elsewhere. This prevents conflicting instructions.
- **Iterate.** Start with the basics and add detail as you discover what Copilot gets wrong.
- **Review output.** AI-generated code always needs human review. These configurations improve first-draft quality but do not replace code review.

## References

- [GitHub Copilot customisation docs](https://docs.github.com/en/copilot/customizing-copilot){:target="_blank"} (opens in new tab)
- [Capgemini GitHub Copilot template](https://github.com/Capgemini/template-github-copilot){:target="_blank"} (opens in new tab) — the community template that inspired this structure
- [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the guardrails encoded in the example files
- [Defra AI in the SDLC playbook](https://defra.github.io/defra-ai-sdlc/){:target="_blank"} (opens in new tab) — foundational AI guidance including the four pillars
- [agentskills.io](https://agentskills.io){:target="_blank"} (opens in new tab) — open standard specification for Agent Skills
