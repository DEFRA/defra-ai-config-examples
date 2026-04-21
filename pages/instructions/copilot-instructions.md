---
layout: default
title: Root Instructions
---

# Root Instructions — Example

This is an example `.github/copilot-instructions.md` file — the root instructions that are always active for every Copilot interaction in your repository. The rules are drawn from [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the single source of truth for Defra coding practices. Edit the project overview section for your service.

## Example file contents

The content below goes into your `.github/copilot-instructions.md` file:

---

````markdown
# Project Instructions

## Project overview

<!-- Edit this section for your service -->

This is a [service name] for Defra, built with Node.js and Hapi. It [brief description of what the service does]. Define if [microservices design, frontend/backend split, or other architectural patterns apply].

- **Runtime**: Node.js (Active LTS)
- **Language**: Vanilla JavaScript
- **Framework**: Hapi
- **Frontend templating**: Nunjucks with GOV.UK Frontend
- **Database**: [PostgreSQL / MongoDB / none]
- **Hosting**: Containerised (Docker) running on [AWS ECS / Azure AKS / other]
- **Authentication**: [Microsoft Entra ID (internal services) / GOV.UK One Login (public-facing GDS services) / none]

<!-- Authentication guidance:
  - Internal staff-facing tools → Microsoft Entra ID (Azure AD)
  - Public-facing GDS services → GOV.UK One Login (https://www.sign-in.service.gov.uk/) -->

## Project paths quick reference

<!-- Edit these paths to match your service layout -->

```
src/
├── config/           # Server and environment configuration
├── plugins/          # Hapi plugins (auth, CSRF, headers, views)
├── routes/           # Route handlers — one file per route group
├── services/         # Business logic — called by route handlers
├── models/           # Database models
├── utils/            # Shared utilities (validation helpers, formatters)
├── views/
│   ├── layouts/      # Nunjucks base layouts (e.g. main.njk)
│   ├── partials/     # Reusable template fragments
│   ├── macros/       # GOV.UK component macros
│   ├── errors/       # Error page templates (404, 500)
│   └── pages/        # Page-level templates
├── public/           # Static assets (CSS, images)
└── __tests__/        # Test files mirroring src/ structure
```

## Naming conventions

- **Directories and template files**: `kebab-case` (e.g. `my-component.njk`)
- **JavaScript files**: `camelCase` (e.g. `applicationService.js`)
- **Routes (URL paths)**: lowercase with hyphens (e.g. `/submit-application`)
- **Environment variables**: `UPPER_SNAKE_CASE` (e.g. `DATABASE_HOST`)
- **CSS classes**: follow GOV.UK conventions (BEM where extending)
- **Config keys**: `lowerCamelCase` in the config module; read via `import { config } from '../config/index.js'`

## Branching and version control

- Use Git with the Defra GitHub organisation
- Main branch is always shippable — it must build, pass all tests, and be deployable at any time
- All work is done on branches, never directly on main. Follow trunk-based development practices with short-lived feature branches and pull requests
- Branch naming: `<type>/<brief-description>`
  - Types: `feature/`, `fix/`, `docs/`, `refactor/`, `test/`, `chore/`
- Commit messages use conventional format: `type: short description`
  - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
- Squash and merge when closing PRs
- Tag releases with semver before deployment

## Quality gates

All code must pass these checks before merging:

- Linter passes (`npx neostandard`) — do not extend or modify the ruleset, do not set `noStyle: true`
- All tests pass (`npm test`)
- Unit test coverage ≥90% (no decrease from baseline or SonarCloud baseline)
- SonarQube / SonarCloud quality gate passes
- At least one approving code review from another developer
- No unresolved security vulnerabilities in dependencies

<!-- STANDARDS NOTE: These instructions reflect Defra software development standards (https://github.com/DEFRA/software-development-standards). Standards evolve — review this file periodically or after any Defra standards update notification. -->

## Code standards

- Follow [Defra common coding standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/common_coding_standards.md)
- Follow [12-factor app](https://12factor.net/) principles — config from environment, stateless processes, logs as event streams, explicit dependency declaration
- Only use approved MCP servers (see [Defra MCP guidance](https://defra.github.io/defra-ai-sdlc/pages/appendix/defra-mcp-guidance/)) — do not enable community or self-built MCP servers
- No commented-out code
- No magic numbers — use named constants
- Prefer small, focused functions with descriptive names
- Use environment variables for all configuration — never hard-code secrets or connection strings
- Keep controllers thin: validate input, call a service, render a view

## Allowed dependencies

Full approved/discouraged dependency lists live in scoped instruction files (e.g. `node-backend.instructions.md`). Critical packages — do not introduce in any file:

- `express`, `fastify`, `koa` (use Hapi)
- `@hapi/joi` (use standalone `joi`)
- `mongoose` (use native `mongodb` driver)
- `moment` (use `Intl.DateTimeFormat` or `date-fns`)
- `lodash` (use native methods)
- `request`, `axios` (use native `fetch` or `undici`)
- `jquery` (progressive enhancement only)
- `eslint`, `prettier`, `standard` (use `neostandard` per [Defra JS standard](https://defra.github.io/software-development-standards/standards/javascript_standards/))
- `typescript` (requires approved exception)

New dependencies must be widely used, actively maintained, and compatible with the current Node.js LTS version.

## Security

- Follow OWASP Secure Coding Practices
- Never commit secrets to source control
- Never log PII (names, addresses, emails, phone numbers, NI numbers, bank details, API keys, tokens)
- Validate and sanitise all user input using `joi`
- Use parameterised queries for database access
- Avoid `eval`, dynamic `Function()`, or executing user-supplied data
- Validate and normalise file paths — reject path traversal attempts

## Documentation

- Write JSDoc comments for all exported functions
- Keep the README up to date with setup and run instructions
- Document breaking changes in PR descriptions

## How Copilot should respond

When generating or editing code for this project:

- Follow conventions already in the codebase — check existing patterns first
- Prefer modifying existing files over creating new ones when the change fits naturally
- Provide minimal diffs touching only the necessary files; do not refactor unrelated code
- Always include or update tests for changed behaviour
- For any form or reusable UI pattern, propose or extend a Nunjucks macro using GOV.UK components
- Keep solutions DRY: before adding new utilities, search for similar code in `src/utils/` or existing routes
- If a request conflicts with these instructions, or would use a discouraged library, skip tests, hardcode a secret, or break a quality gate — flag it explicitly and do not proceed silently
- If a standards violation cannot be avoided (e.g. integration constraint), document the deviation in a code comment and raise it for review

## Licence

All code is published under the [Open Government Licence v3](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) unless an exception is approved.
````

---

[Back to instructions index]({{ "/pages/instructions" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
