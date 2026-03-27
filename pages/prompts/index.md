---
layout: default
title: Prompts — Examples for GitHub Copilot
---

# Prompts — Examples for GitHub Copilot

Prompts are reusable templates for common tasks. Unlike instructions (always active) or agents (personas), prompts are one-shot — run them when you need a specific output.

## How prompts work

Prompt files live in `.github/prompts/` and use the `.prompt.md` extension. When you open Copilot Chat, you can select a prompt from the prompt picker to run it.

Prompts can include:
- A description of the expected output
- The file format and save location
- Variables (placeholders) that the user fills in
- References to instruction files for standards context

## How to use these examples

1. Copy the `.prompt.md` file into your repository's `.github/prompts/` directory
2. Open Copilot Chat and select the prompt from the picker
3. Fill in any variables when prompted
4. Review the generated output before saving

## Available examples

| Prompt | Purpose | Output |
|--------|---------|--------|
| [Write ADR]({{ "/pages/prompts/write-adr" | relative_url }}) | Generate architecture decision records | Markdown ADR document |
| [Write Tests]({{ "/pages/prompts/write-tests" | relative_url }}) | Generate test suites for existing code | Jest test file |
| [Security Review]({{ "/pages/prompts/security-review" | relative_url }}) | Review code against OWASP and Defra security standards | Review findings report |
| [Scaffold Repo]({{ "/pages/prompts/scaffold-repo" | relative_url }}) | Generate the full Copilot config, skills, `.copilotignore`, and MCP setup for a Defra project | Complete directory structure |
| [Generate PR Description]({{ "/pages/prompts/generate-pr-description" | relative_url }}) | Write a structured PR description from staged changes | Pull request description |
| [Write Runbook]({{ "/pages/prompts/write-runbook" | relative_url }}) | Generate an operational runbook for a service | `docs/runbook.md` |
| [Refactor to Standards]({{ "/pages/prompts/refactor-to-standards" | relative_url }}) | Audit code against Defra standards and produce a prioritised refactor plan | Standards audit report |

## Writing your own prompts

Follow this structure:

```markdown
---
description: One-line summary of what this prompt produces
---

# Prompt Title

[Clear description of what to generate]

## Context
[What the user needs to provide or select]

## Output format
[Expected structure, file type, save location]

## Standards
[Reference to instruction files or external standards]
```

**Tips:**
- Start with a clear description of the expected output
- Tell Copilot where to save the file
- Reference instruction files for standards rather than restating rules
- Keep prompts focused on one task

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
