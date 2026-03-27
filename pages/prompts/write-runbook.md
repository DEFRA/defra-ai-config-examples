---
layout: default
title: Write Runbook Prompt
---

# Write Runbook Prompt — Example

This is an example `.prompt.md` file for generating an operational runbook for a service. Run it when preparing for a first release, after a significant architecture change, or when onboarding a new on-call team.

## Example file contents

The content below goes into your `.github/prompts/write-runbook.prompt.md` file:

---

````markdown
---
description: Generate an operational runbook for this service
---

# Write Runbook

Generate an operational runbook for this service and save it to `docs/runbook.md`.

The runbook is for on-call engineers who may not be familiar with the codebase. Write it to be practical and specific — not generic cloud documentation. Every alert response must be a numbered checklist, not prose.

## Context to read first

Before generating the runbook, read:

- `README.md` — service overview and setup
- `src/config/` or equivalent — all environment variables and their purpose
- The health check route (usually `GET /health`) — understand what it checks
- Any existing runbook or incident response docs in `docs/`
- `docker-compose.yml` or infrastructure config — understand the dependencies

## Output

Save the runbook to `docs/runbook.md` with this structure:

```markdown
# [Service Name] — Runbook

## Overview

Brief description of what this service does, who uses it, and its criticality
(e.g. "This is a live-facing service used by waste operators to register exemptions.
Downtime affects users' ability to submit regulatory data to Defra.").

## Architecture

- **Runtime**: [e.g. Node.js 22 on Docker / .NET 9 on Azure App Service]
- **Key dependencies**: [e.g. PostgreSQL, Redis, GOV.UK Notify, upstream permit API]
- **Entry point**: [URL(s) in each environment]

## Environments

| Environment | URL | Purpose |
|-------------|-----|---------|
| Development | ... | Local testing |
| Staging | ... | Pre-production verification |
| Production | ... | Live service |

## Health check

`GET /health` — returns `200 OK` with `{ "status": "ok" }` when healthy.

A failing health check indicates: [describe the most likely cause — e.g. database unreachable].

## Key environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `DATABASE_URL` | Yes | Connection string for the primary database |
| `SESSION_SECRET` | Yes | Secret used to sign session cookies |

## Alerts and responses

### Service is returning 5xx errors

1. Check application logs — look for error messages and stack traces
2. Check the health endpoint (`GET /health`) — note what it returns
3. Check database connectivity — is the database reachable and accepting connections?
4. Check downstream API availability — are external services responding?
5. If none of the above — restart the service container and monitor for recurrence

### High response times (p95 > 2s)

1. Check for slow database queries in the application logs
2. Check whether a downstream dependency is responding slowly
3. Check container resource utilisation — CPU and memory pressure
4. Check for unusually high traffic volume

### Health check is failing

1. Check which component is failing — the health check should identify it
2. Check the logs immediately before the failure started
3. If database: check connectivity and connection pool exhaustion
4. If memory: check for a memory leak — restart and monitor

## Deployment

[Describe how to deploy a new version — automated pipeline link, manual steps if any.]

## Rollback

[Describe how to roll back to the previous version, including commands.]

## Contacts

| Role | Contact |
|------|---------|
| On-call | [Slack channel] |
| Product owner | [Name] |
| Service owner | [Name] |
```

## Rules for the runbook

- Use imperative steps — "Check the logs" not "The logs should be checked"
- Include real commands and URLs where you can infer them from the codebase
- Mark placeholders clearly with `[...]` for the team to complete
- Keep alert responses as numbered steps — easy to follow under pressure
- Do not include secrets, passwords, or internal system credentials
````

---

[Back to prompts index]({{ "/pages/prompts" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
