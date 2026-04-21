---
layout: default
title: Scaffold Repository Config Prompt
---

# Scaffold Repository Config Prompt — Example

This is an example `.prompt.md` file for bootstrapping the full GitHub Copilot configuration in a Defra repository. Run it once when starting a new project or adding Copilot support to an existing one.

It generates the complete `.github/` directory including instructions, agents, prompts, skills, `.copilotignore`, and MCP server configuration — all tailored to your project details.

> **Prefer an interactive setup?** Use the [Repo Setup agent]({{ "/pages/agents/repo-setup" | relative_url }}) instead. It interviews you about your project and generates the same files through a guided conversation.

## Example file contents

The content below goes into your `.github/prompts/scaffold-repo.prompt.md` file:

---

````markdown
---
description: Generate the full .github/ Copilot configuration for a Defra project — instructions, agents, prompts, skills, .copilotignore, and MCP config
---

# Scaffold Copilot Config

Generate the complete `.github/` directory structure for this repository, including root instructions, scoped instructions, agents, prompts, skills, `.copilotignore`, and MCP server configuration. Tailor every file to the project details below.

## Project details

Provide these values (edit them before running this prompt):

- **Service name**: [e.g. Waste exemptions service]
- **Service description**: [e.g. Allows waste operators to register exemptions from environmental permits]
- **Tech stack**: [Node.js + Hapi | .NET + ASP.NET Core Minimal API | both]
- **Frontend**: [Nunjucks + GOV.UK Frontend | API only (no frontend)]
- **Database**: [PostgreSQL | MongoDB | DynamoDB | none]
- **Hosting**: [AWS ECS | Azure AKS | Azure App Service | other]
- **Auth method**: [Azure AD | Magic link | Basic session | none]
- **Message broker**: [Azure Service Bus | AWS SQS | RabbitMQ | none]

## Files to generate

Create every file listed below. Skip sections that do not apply based on the tech stack.

### 1. Root instructions — `.github/copilot-instructions.md`

Include:
- Project overview with service name, description, and tech stack from above
- Project paths — match the actual directory layout of this repository
- Naming conventions (kebab-case directories, camelCase JS files, PascalCase C# files)
- Branching and version control (trunk-based, conventional commits, squash-and-merge)
- Quality gates (neostandard for JS, dotnet format for C#, ruff for Python, SonarCloud quality gate, 90% coverage)
- Allowed dependencies table — list the packages this project actually uses
- Discouraged dependencies — Express, lodash, moment, jquery, @hapi/joi
- Security rules — OWASP, no PII in logs, joi validation, parameterised queries
- How Copilot should respond — follow existing patterns, minimal diffs, include tests

### 2. Scoped instructions — `.github/instructions/`

**For Node.js projects**, create `node-backend.instructions.md`:
- `applyTo: "**/*.js,**/*.mjs"`
- Hapi framework conventions, @hapi/boom, joi, @hapi/crumb, @hapi/blankie
- Route handler → service → view pattern
- CSRF, CSP, security headers, cookie settings
- Winston logging, health endpoint, config module
- Jest + Supertest testing

**For .NET projects**, create `csharp-backend.instructions.md`:
- `applyTo: "**/*.cs,**/*.csproj"`
- Minimal API conventions, Serilog, FluentValidation
- xUnit v3 + FluentAssertions + NSubstitute testing
- Health endpoint, dependency injection, nullable reference types

**For frontend projects**, create `frontend.instructions.md`:
- `applyTo: "**/*.njk,**/*.html,**/*.css"`
- GOV.UK Design System, macro-first, no inline styles
- WCAG 2.2 AA, semantic HTML, error summary linking
- Nunjucks directory structure, progressive enhancement

**For all projects**, create `docs.instructions.md`:
- `applyTo: "**/*.md"`
- README structure (badges, prerequisites, running locally, deployment, licence)
- ADR format (context, decision, consequences)
- British English, plain language, sentences under 25 words
- JSDoc for exported functions, no inline comments for obvious code

**For all projects**, create `testing.instructions.md`:
- `applyTo: "**/*.test.js,**/*.test.ts,**/*.spec.js,**/*.spec.ts,**/*Tests.cs"`
- BDD naming convention — "should [expected behaviour] when [condition]"
- AAA pattern (Arrange, Act, Assert)
- Coverage targets — 90% global, 95% business logic, 100% security-critical paths
- Mock discipline — mock at boundaries only, no implementation-detail mocking
- One assertion per test, descriptive failure messages

### 3. Agents — `.github/agents/`

Create `defra-app-developer.agent.md`:
- Persona: senior Defra developer
- Tech stack section matching the project details above
- Full workflow with pre-commit checklist including SonarCloud quality gate
- Coding standards, security, logging, accessibility, documentation, containers
- Reference the tester agent for testing details

Create `code-reviewer.agent.md`:
- 9-category review checklist (PR hygiene, correctness, tests, security, performance, maintainability, architecture, documentation, accessibility)
- Severity levels (blocking, recommended, nit)
- SonarCloud quality gate and security hotspot checks

Create `tester.agent.md`:
- BDD testing approach with testing pyramid
- Test naming conventions with examples
- Minimum route test scenarios (happy path, validation, CSRF, auth, 404)
- Security testing checklist
- Coverage targets table (90% global, 95% business logic, 100% security)

Create `accessibility-advisor.agent.md`:
- WCAG 2.2 AA expert with GOV.UK Design System specialisation
- Audit workflow: scan templates, check macro usage, validate error handling, check colour contrast
- Output a prioritised findings table with severity, WCAG criterion, location, and fix
- Reference GOV.UK Frontend component macros — prefer macros over raw HTML
- Must not change business logic — accessibility fixes only

Create `orchestrator.agent.md`:
- Plans complex features and delegates phases to specialist agents
- Workflow: gather requirements, decompose into phases, assign each phase to the right agent, track progress
- Output a numbered phase plan with agent assignment, inputs, outputs, and done criteria
- Must not write code directly — delegates all code generation to specialist agents

### 4. Prompts — `.github/prompts/`

Create `write-adr.prompt.md`:
- Standard ADR template (context, decision, consequences, alternatives)
- Plain English, active voice, reference Defra standards

Create `write-tests.prompt.md`:
- Generate test suites following the project's test framework
- Coverage targets, mocking strategy, BDD naming

Create `security-review.prompt.md`:
- OWASP-based security review
- Defra-specific checks (PII, CSRF, CSP, secrets)

Create `generate-pr-description.prompt.md`:
- Structured PR description from staged or committed changes
- Sections: summary, what changed, how to test, checklist
- Link to related work items or ADRs

Create `write-runbook.prompt.md`:
- Operational runbook for the service
- Sections: service overview, architecture, deployment, monitoring, incident response, rollback
- Include health check URLs, log locations, and key contacts placeholders

Create `refactor-to-standards.prompt.md`:
- Audit the codebase against Defra software development standards
- Produce a prioritised refactoring plan with effort estimates
- Group findings by category (security, testing, code quality, documentation)

### 5. Skills — `.github/skills/`

Create `defra-standards/SKILL.md`:
- Frontmatter: name (≤64 chars), description (≤1024 chars, mentions "Defra", "Node.js", ".NET", "coding standards")
- Node.js and .NET coding standards, approved packages, rejected packages
- CDP platform conventions (health endpoints, config module, logging)
- Directory must match the `name` field in frontmatter

Create `govuk-accessibility/SKILL.md`:
- Frontmatter: name, description (mentions "WCAG", "GOV.UK", "accessibility")
- WCAG 2.2 AA success criteria relevant to server-rendered pages
- GOV.UK Frontend macro usage patterns and common failure modes
- Error summary linking, colour contrast, focus management

Create `security-and-pii/SKILL.md`:
- Frontmatter: name, description (mentions "security", "PII", "OWASP")
- UK government data classifications (OFFICIAL, OFFICIAL-SENSITIVE)
- Fields that constitute PII and logging rules
- Parameterised queries, input validation, CSRF, CSP headers

### 6. Content exclusion — `.copilotignore`

Create `.copilotignore` in the repository root with entries for: `.env*`, `**/secrets/`, `**/node_modules/`, `**/*.pem`, `**/*.key`, `**/*.pfx`, `**/dist/`, `**/build/`, `**/_site/`, `docker-compose.override.yml`, and any test fixtures with real personal data. Include a comment header explaining the file's purpose.

### 7. MCP server configuration — `.vscode/mcp.json`

Create `.vscode/mcp.json` with only Defra-approved MCP servers (GitHub, Azure DevOps, Playwright as applicable). Use `stdio` transport with `npx` commands and placeholder `args` referencing the project's organisation and repo. Note that MCP servers require VS Code restart after configuration.

## Rules

- Use British English throughout (organisation, colour, licence, behaviour)
- Follow GOV.UK style guide — plain English, active voice, sentences under 25 words
- Every instruction must be actionable and imperative ("Use Hapi" not "Hapi should be used")
- Include one example and one counterexample for key rules
- Keep each file under 200 lines; do not duplicate rules — cross-reference instead
````

---

[Back to prompts index]({{ "/pages/prompts" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
