---
layout: default
title: LAP Delivery Standards Instructions
---

# LAP Delivery Standards Instructions — Example

This page shows the LAP delivery standards instruction file for GitHub Copilot.

## Example file contents

````markdown
---
description: "Defra LAP delivery standards that every PRD, architecture-requirements doc, and feature specification must embed and enforce: Defra/GDS software development standards, >=90% test coverage with acceptance-criteria-to-test mapping, WCAG 2.2 AA accessibility, secure-by-design (OWASP), observability, cloud well-architected, and the no-silent-loss traceability rules."
applyTo: "output/**,**/features/**,**/*PRD*.md,**/architecture-requirements.md"
---

# LAP Delivery Standards

These standards are mandatory for every artefact the LAP Innovation Agents produce and for every build that consumes those artefacts. They must be embedded from the outset — written into the PRD, the architecture-requirements document, and each feature specification — not bolted on later. Use British English throughout.

## 1. Software development standards

- Follow **Defra's software development standards** and the cross-government **GDS Service Standard** and **Technology Code of Practice**.
- Prefer open standards and well-supported, mainstream technologies. Avoid bespoke solutions where a proven platform capability exists.
- All infrastructure is defined as code (IaC) and all configuration is version-controlled. A service must be fully reproducible from source with no manual steps.
- Code is peer-reviewed, linted, and formatted to an agreed style before merge.

## 2. Testing and coverage

- **Automated test coverage must be at least 90%** (line and branch) for all new code.
- **Every acceptance criterion maps to a named automated test.** The test's identifier is recorded against the criterion in the feature specification and the traceability manifest, so an unimplemented criterion surfaces as a missing or failing test.
- Provide unit, integration, and end-to-end tests appropriate to the feature. Tests run in CI and block merge on failure.
- Accessibility and security checks are automated where possible (for example axe-core scans and dependency/SAST scanning) and included in CI.

## 3. Accessibility

- All user interfaces must meet **WCAG 2.2 level AA**.
- Every UI-facing user story includes accessibility acceptance criteria: keyboard operability, visible focus, sufficient colour contrast, correct semantic structure and labelling, error identification and suggestions, and support for assistive technologies.
- Follow the GDS Design System and content patterns where applicable.

## 4. Security by design

- Design and build to defend against the **OWASP Top 10**. Validate and sanitise all input at trust boundaries; encode output; use parameterised queries.
- Apply least privilege, secure defaults, and defence in depth. Never hard-code secrets — use a managed secret store.
- Authentication, authorisation, session handling, and audit logging are treated as first-class requirements.
- Security event logs are retained for a minimum of two years, in line with Defra SOC retention.

## 5. Observability and operability

- Emit structured logs, metrics, and traces. Forward the log categories required by the Business Impact Assessment (BIA) to the Defra SOC.
- Define health checks and alerting so that failures are detected before they affect users.
- Document runbooks and the recovery approach consistent with the service tier (see the `cloud-tier-architect` output).

## 6. Cloud and service tier

- Apply the non-functional and architecture requirements for the service's **cloud platform (AWS or Azure)** and **service tier (1a, 1b, 2, 3, 4)** as produced by the `cloud-tier-architect` agent and recorded in `output/architecture-requirements.md`.
- These tier requirements (availability, resiliency, RTO/RPO, backup, patching, connectivity, monitoring) are non-negotiable NFRs for the target service.

## 7. Definition of Done (no silent loss of functionality)

A feature is done only when **every** acceptance criterion is either:

1. **Implemented and verified** — the mapped automated test exists and passes, the UI meets WCAG 2.2 AA, coverage is at least 90%, and the security and NFR requirements are satisfied; **or**
2. **Explicitly deferred** — recorded in `output/descope-register.md` with a rationale (for example effort versus value), an owner, and a date, and marked `deferred` in `output/features/manifest.json`.

**Functionality must never disappear silently.** If a capability is not built, it must appear as an outstanding item in the manifest (`not-started` / `in-progress`) or as a justified `deferred` entry in the descope register. An autonomous build loop must keep working until only explicitly deferred items remain.

## 8. Traceability

Maintain an unbroken chain of stable identifiers: **legacy capability / business rule → PRD requirement → feature (FT-xxx) → user story (US-xxx) → acceptance criterion (AC-n) → test id**. This chain is recorded in both `output/traceability.md` (human-readable) and `output/features/manifest.json` (machine-readable) and is kept current by the `completeness-auditor` agent on each build iteration.
````

[Back to GitHub Copilot instructions]({{ "/pages/instructions/lap-gitHub-copilot" | relative_url }}) · [Back to Instructions]({{ "/pages/instructions" | relative_url }})
