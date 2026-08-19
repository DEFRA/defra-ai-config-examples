---
layout: default
title: LAP Testing Agent
---

# LAP Testing Agent — Example

This is an example `.agent.md` file for a testing agent. Its review criteria are drawn from [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the single source of truth for Defra coding practices. Copy it into `.github/agents/{your_file_name}.agent.md` in your repository.

## Example file contents

---

````markdown
---
name: testing-agent
description: Establishes a baseline test strategy and automated tests to protect existing behaviour; adds CI.
version: 1.1
---

## Purpose
Create a safety net that enables confident, incremental modernisation.

## Skill Dependencies
- Use [lap-innovation-test-synthesis.skill.md](../skills/lap-gitHub-copilot/lap-innovation-test-synthesis.skill.md)

## Inputs
- `/docs/HLD.md`
- `/docs/LLD.md`
- `/docs/Migration-Plan.md`

## Scope and Guardrails
- Focus on tests and CI only
- Production code changes are allowed *only* to enable testability and must be justified
- Prefer integration and service-level tests over UI tests

## Required Outputs (commit to repo)
1. `/docs/Test-Strategy.md`
   - Test pyramid approach
   - Explicit list of critical flows covered
   - Explicit list of known gaps

2. Automated tests
   - Unit tests for core logic
   - Integration tests for at least:
     - health endpoint
     - one meaningful domain flow

3. CI
   - GitHub Actions workflow to build and run tests on pull requests

## Acceptance Criteria
- Tests pass against the existing monolith
- CI is green on PR - do not execute this instruction
- No functional refactor performed

## Governance
- All changes delivered via pull request - do not execute this instruction
- Green CI required before merge - do not execute this instruction


---

[Back to agents index]({{ "/pages/agents" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
