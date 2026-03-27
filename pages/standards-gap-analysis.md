---
layout: default
title: Standards Gap Analysis
---

# Standards Gap Analysis

This page analyses the standards landscape that this template repository encodes, draws from, and references. It identifies gaps — standards that apply to Defra digital services but are not yet covered — and surfaces tensions and conflicts that Defra needs to resolve to give teams unambiguous guidance.

Use this alongside the [Getting Started guide]({{ "/pages/getting-started" | relative_url }}) and the [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab).

---

## What this repo currently covers

The agents, instructions, prompts, and skills in this template reference and encode the following standards:

### Defra software development standards

The repo is explicitly rooted in the [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) as the primary authority. Within that repo, the following specific standards documents are referenced:

| Standard | Where referenced in this template |
|----------|----------------------------------|
| [Common coding standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/common_coding_standards.md){:target="_blank"} (opens in new tab) | All agents, root instructions |
| [Node.js standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/node_standards.md){:target="_blank"} (opens in new tab) | Node.js backend instructions, Defra App Developer agent |
| [JavaScript standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/javascript_standards.md){:target="_blank"} (opens in new tab) | Node.js backend instructions |
| [Logging standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/logging_standards.md){:target="_blank"} (opens in new tab) | All agents, security-and-pii skill |
| [Security standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/security_standards.md){:target="_blank"} (opens in new tab) | Code reviewer and tester agents, security-review prompt |
| [Container standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/container_standards.md){:target="_blank"} (opens in new tab) | Defra App Developer agent, Defra Standards skill |
| [Quality assurance standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/quality_assurance_standards.md){:target="_blank"} (opens in new tab) | Defra App Developer and Tester agents |
| [.NET standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/net_standards.md){:target="_blank"} (opens in new tab) | C# backend instructions |
| [C# coding standards](https://github.com/DEFRA/software-development-standards/blob/main/docs/standards/csharp_coding_standards.md){:target="_blank"} (opens in new tab) | C# backend instructions |

### GOV.UK and GDS standards

| Standard | Where referenced |
|----------|-----------------|
| [GOV.UK Service Standard](https://www.gov.uk/service-manual/service-standard){:target="_blank"} (opens in new tab) | Defra App Developer agent |
| [Technology Code of Practice](https://www.gov.uk/government/publications/technology-code-of-practice/technology-code-of-practice){:target="_blank"} (opens in new tab) | Defra App Developer agent |
| [GOV.UK content style guide](https://www.gov.uk/guidance/style-guide/a-to-z-of-gov-uk-style){:target="_blank"} (opens in new tab) | Defra App Developer agent |
| GOV.UK Design System (WCAG, macros, patterns) | Frontend instructions, GOV.UK Accessibility skill, Accessibility Advisor agent |
| [GOV.UK One Login](https://www.sign-in.service.gov.uk/){:target="_blank"} (opens in new tab) | Root instructions, Defra App Developer agent |

### Legal and regulatory obligations

| Standard / Legislation | Where referenced |
|-----------------------|-----------------|
| [Public Sector Bodies Accessibility Regulations 2018](https://www.legislation.gov.uk/uksi/2018/952/made){:target="_blank"} (opens in new tab) | Accessibility Advisor agent, GOV.UK Accessibility skill |
| WCAG 2.2 Level AA | Frontend instructions, GOV.UK Accessibility skill, Accessibility Advisor agent, Code Reviewer agent |
| UK GDPR / Data Protection Act 2018 | Security and PII skill (data classifications, PII categories) |
| [Open Government Licence v3](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/){:target="_blank"} (opens in new tab) | Root instructions template |

### Security frameworks

| Standard | Where referenced |
|----------|-----------------|
| [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/){:target="_blank"} (opens in new tab) | All agents, security-review prompt, C# and Python instructions |
| [OWASP Top 10](https://owasp.org/www-project-top-ten/){:target="_blank"} (opens in new tab) | Security-review prompt, security-and-pii skill |
| [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/){:target="_blank"} (opens in new tab) | Tester agent |
| [NCSC Secure Development and Deployment Guidance](https://www.ncsc.gov.uk/collection/developers-collection){:target="_blank"} (opens in new tab) | Security-review prompt |

### Engineering methodology and platform

| Standard / Framework | Where referenced |
|---------------------|-----------------|
| [12-factor app methodology](https://12factor.net/){:target="_blank"} (opens in new tab) | Root instructions, Node.js instructions, Defra App Developer agent |
| Defra Core Delivery Platform (CDP) conventions | Defra Standards skill |
| Defra Docker base images (`defradigital/node`, `defradigital/dotnet`) | Defra Standards skill, Defra App Developer agent |
| SonarCloud / SonarWay quality gate | All agents, testing instructions |
| [Microsoft Entra ID (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-js){:target="_blank"} (opens in new tab) | Root instructions, Defra App Developer agent |

### Language-specific standards

| Standard | Where referenced |
|----------|-----------------|
| [PEP 8 (Python style)](https://peps.python.org/pep-0008/){:target="_blank"} (opens in new tab) | Python backend instructions |
| [Microsoft C# coding conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions){:target="_blank"} (opens in new tab) | C# backend instructions |

---

## Gaps — standards not yet covered

These are standards that apply to Defra digital services but are not currently encoded in the template.

### 1. GDS API technical and data standards

**What is missing:** The [GDS API technical and data standards](https://www.gov.uk/guidance/gds-api-technical-and-data-standards){:target="_blank"} (opens in new tab) define how government APIs must be designed and documented — RESTful conventions, versioning, error formats, and OpenAPI specification requirements. Most Defra services expose or consume APIs, but the template has no API design guidance.

**What Defra should do:** Add an `api.instructions.md` example and extend the Defra Standards skill to cover API naming, versioning (`/v1/`), OpenAPI documentation, and rate limiting. Align with the [HMRC API standards](https://developer.service.hmrc.gov.uk/api-documentation){:target="_blank"} (opens in new tab) where Defra's services interact with HMRC systems.

---

### 2. Supply chain security and dependency management

**What is missing:** The template mentions `npm audit` and asks that no known vulnerabilities exist, but there is no guidance on:

- Configuring Dependabot for automated dependency updates
- Software Bill of Materials (SBOM) — a growing requirement under NCSC and US Executive Order 14028, increasingly referenced in UK public sector procurement
- Pinning dependency versions in production vs using ranges
- Verifying package integrity (npm provenance, sigstore)

**What Defra should do:** Add a supply chain security section to the security standards and create a checklist prompt or skill covering Dependabot setup, lock file hygiene, and SBOM generation (`npm sbom --workspace=...`).

---

### 3. Green software and environmental sustainability

**What is missing:** Defra is the UK government's environment department. There is no coverage of sustainable software engineering practices — an omission that is both a values gap and an emerging cross-government standard.

The [Green Software Foundation's Software Carbon Intensity (SCI) specification](https://sci.greensoftware.foundation/){:target="_blank"} (opens in new tab) and the [Sustainable Web Design Model](https://sustainablewebdesign.org/){:target="_blank"} (opens in new tab) provide measurable frameworks. The [DDAT framework](https://www.gov.uk/guidance/data-and-technology-roles){:target="_blank"} (opens in new tab) is beginning to incorporate these principles.

**What Defra should do:** Introduce a sustainability section into the Defra App Developer agent's standards, covering: prefer static assets over server-rendered pages where appropriate, cache aggressively, avoid polling where webhooks are available, right-size infrastructure, and measure page weight against a target. This would differentiate Defra's template from any other department's and is directly on-brand.

---

### 4. Welsh language and internationalisation

**What is missing:** Services affecting Welsh citizens have obligations under the [Welsh Language (Wales) Measure 2011](https://www.legislation.gov.uk/mwa/2011/1){:target="_blank"} (opens in new tab) and the Welsh Language Commissioner's standards. Defra delivers services (land management, environmental permitting) that affect rural Wales. There is no i18n guidance in the template.

**What Defra should do:** Add a note in the frontend instructions and Defra App Developer agent on how to structure content for translation, how to implement language switching using GOV.UK Frontend patterns, and when a service requires a Welsh translation. Reference the [Welsh Language Standards](https://www.gov.uk/guidance/welsh-language-scheme){:target="_blank"} (opens in new tab).

---

### 5. Data standards and open data

**What is missing:** Defra holds significant environmental data. There is no guidance on:

- GOV.UK open data standards (data.gov.uk publishing requirements)
- FAIR data principles (Findable, Accessible, Interoperable, Reusable)
- Geographic information systems (GIS) data standards (relevant for land-based services)
- Environmental monitoring data formats and exchange standards

**What Defra should do:** Add a data skill (`defra-data-standards/SKILL.md`) covering open data publishing, metadata requirements, and data format standards relevant to Defra services.

---

### 6. Accessibility testing methodology

**What is covered:** WCAG 2.2 AA criteria are well covered in the GOV.UK Accessibility skill and the Accessibility Advisor agent.

**What is missing:** No guidance on the *process* of accessibility testing:

- Which automated tools to use (Axe, Lighthouse, Wave) and their known limitations
- Manual testing requirements (keyboard navigation, screen reader testing with NVDA/JAWS/VoiceOver)
- The legal requirement for an **accessibility statement** (separate from passing WCAG) under the Public Sector Bodies Accessibility Regulations 2018
- How to handle non-compliance — the regulations require a roadmap for known failures
- User research with disabled users

**What Defra should do:** Add an accessibility testing checklist prompt and extend the Accessibility Advisor agent with specific tooling commands and the requirement to generate an accessibility statement template.

---

### 7. Infrastructure as Code (IaC) standards

**What is missing:** The template covers containerisation (Docker), but Defra services are deployed to cloud platforms (Azure, AWS). There is no guidance on:

- Terraform or Bicep coding standards
- Secret scanning in IaC files
- Least-privilege IAM patterns for service accounts
- Network segmentation and ingress control patterns

**What Defra should do:** Add an IaC skill (`defra-iac-standards/SKILL.md`) and extend the security-review prompt to check IaC files for hardcoded secrets, overly permissive IAM roles, and public storage bucket exposure.

---

### 8. Performance standards and Core Web Vitals

**What is missing:** No performance budgets, no Core Web Vitals targets, no guidance on measuring page weight or API response times. GDS expects government services to respond quickly on all devices and connection speeds.

**What Defra should do:** Add performance targets to the Defra App Developer pre-commit checklist (e.g. Largest Contentful Paint < 2.5s, Time to First Byte < 200ms) and extend the frontend instructions with asset optimisation guidance.

---

### 9. Data Protection Impact Assessment (DPIA) triggers

**What is missing:** The security-and-pii skill covers what PII is and how to handle it in code, but does not mention that new services processing personal data require a DPIA under UK GDPR Article 35. This is a governance obligation, not just a coding one — but AI-assisted code generation should flag the trigger points.

**What Defra should do:** Add a DPIA trigger checklist to the security-review prompt — flagging any route handler that processes special category data, introduces profiling, or involves large-scale systematic processing of personal data.

---

### 10. Incident response and operational runbooks

**What is partially covered:** There is a `write-runbook.prompt.md` prompt, but no standard structure for what a Defra runbook must contain, no SRE/on-call standards, and no guidance on how to define SLOs and error budgets.

**What Defra should do:** Extend the write-runbook prompt with required sections aligned to GDS service assessment criteria (Point 12 — Support a live service) and define minimum runbook content: on-call contacts, escalation paths, known failure modes, rollback procedure, and SLO definitions.

---

## Conflicts and tensions Defra needs to resolve

These are areas where the template's guidance is ambiguous, internally inconsistent, or conflicts with evolving practice — and where a clear decision from Defra is needed.

### Conflict 1: TypeScript versus vanilla JavaScript

**The tension:** The template says "use vanilla JavaScript — do not use TypeScript without an approved exception." This directly conflicts with the direction of modern Node.js development, where TypeScript is now the default in many organisations and increasingly expected by developers entering the profession.

**The problem:** There is no defined process for obtaining the "approved exception." Teams wanting to use TypeScript have no clear path. The restriction discourages modern tooling without a compensating mechanism.

**What Defra needs to decide:**
- What does the exception process look like? Who approves it?
- Should new services be allowed to start with TypeScript?
- Is the restriction specific to the CDP platform, or universal?

A practical resolution might be: "TypeScript is allowed on new services with Lead Developer sign-off. Existing JavaScript services should not migrate solely to adopt TypeScript — refactor only when the service is being substantially rebuilt."

---

### Conflict 2: Test framework inconsistency — Jest versus Vitest

**The tension:** The template says "Use Jest for most Node.js projects; CDP frontend projects use Vitest — follow whatever is already set up." This is a pragmatic statement, but it means the template simultaneously recommends two different test frameworks with no clear decision criteria.

**The problem:** New project teams have no guidance on which to choose. Vitest is faster and has better ESM support, but Jest has broader adoption. Mixing frameworks across a portfolio complicates shared knowledge and tooling.

**What Defra needs to decide:** Define a default (e.g. "use Jest unless starting a new Vite-based frontend, in which case use Vitest") and document it clearly in the quality assurance standards.

---

### Conflict 3: SonarCloud thresholds versus tiered coverage targets

**The tension:** The template specifies tiered coverage targets (≥90% global, ≥95% business logic, 100% error handling). It also says "do not decrease from the SonarCloud baseline." These could conflict: a new service starting with 0% coverage has no SonarCloud baseline yet, and SonarWay's default coverage gate may differ from the tiered targets.

**The problem:** If a project is new, the SonarCloud baseline is 0% — meaning any coverage passes "do not decrease." The tiered targets are the intended minimum, but that is not how SonarCloud enforces them by default.

**What Defra needs to decide:** Define a minimum coverage gate configuration for new projects (e.g. set the SonarCloud coverage threshold to 80% on first project onboarding, then enforce the tiered targets in the CI pipeline separately).

---

### Conflict 4: Defra Docker base images versus security patch currency

**The tension:** The template requires use of `defradigital/node` and `defradigital/dotnet` base images. If these images lag behind security patches in the official `node:lts` or `mcr.microsoft.com/dotnet` images, teams face a choice: use a potentially vulnerable Defra image, or use a current image that breaks the standard.

**The problem:** There is no guidance on what to do if a CVE is published against a Defra base image before Defra has patched it. This is a real operational risk, not a hypothetical one.

**What Defra needs to decide:** Define an SLA for patching Defra base images after CVE publication, and define the escalation path if the SLA cannot be met. Allow temporary exception to use the official image with documented justification.

---

### Conflict 5: Python coverage in Defra standards

**The tension:** Python backend guidance is included in this template but the references at the bottom of `python-backend.md` only point to "Defra software development standards" generically — unlike Node.js and C# pages, which reference specific numbered standards documents (`node_standards.md`, `csharp_coding_standards.md`).

**The problem:** It is unclear whether Defra has published Python-specific standards. If not, the Python instructions in this template are based on community best practice (PEP 8, FastAPI conventions) but lack the Defra-authoritative backing that Node.js and C# instructions have.

**What Defra needs to decide:** Either publish Python-specific standards within the software development standards repo and link to them, or acknowledge that Python guidance here is advisory only (not a formal Defra standard) and label it accordingly.

---

### Conflict 6: Authentication boundary — internal versus public services

**The tension:** The template instructs: internal services use Microsoft Entra ID; public-facing GDS services use GOV.UK One Login. But many Defra services exist at the boundary: services used by farmers, land agents, or businesses who are not internal staff but also not general members of the public.

**The problem:** "Internal" and "public" is a binary that does not reflect Defra's service landscape. A service used by regulated businesses, land managers, or third-party agents may not fit either category cleanly.

**What Defra needs to decide:** Define a clear taxonomy of service user types and the authentication approach for each, covering: Defra staff, contracted professionals (agents, consultants), regulated businesses, and members of the public.

---

### Conflict 7: MCP server approval process — point-in-time versus living list

**The tension:** Several files instruct teams to "only use approved MCP servers — check the Defra approved list." That list lives on a separate site and will change over time. The template files have no versioning, so there is no way to know if a team's copy reflects current approvals.

**The problem:** An agent file copied into a repo on day one may reference an MCP server that is subsequently removed from the approved list. The team's repo will still say "approved" even after approval is revoked.

**What Defra needs to decide:** Define a process for how teams are notified of MCP approval changes and how quickly they must update their configuration. Consider embedding a version or date stamp in the approved list URL pattern so old copies are clearly flagged as stale.

---

### Conflict 8: ESLint and Prettier — no canonical shared config

**The tension:** The template specifies ESLint and Prettier, but there is no published `@defra/eslint-config` or `@defra/prettier-config` npm package. Each team configures its own linting rules, which leads to divergence across the portfolio.

**The problem:** The instruction "lint with ESLint and format with Prettier" without a shared config means every team makes different decisions on things like semicolons, import ordering, and rule severity. This undermines the goal of consistent, reviewable code across Defra.

**What Defra needs to decide:** Publish `@defra/eslint-config` and `@defra/prettier-config` as shareable configs on npm (either public or via the Defra GitHub Packages registry), and reference them explicitly in the Node.js instructions template.

---

## Proposed names for this template

The feedback on this project asked for a name — something recognisable, memorable, and that Defra associates with the work done here.

Below are five candidate names with brief rationale:

### Option 1: **Runway**
A runway is where AI assistance takes off — but safe takeoff requires guardrails, guidance, and clear procedures. It suggests both forward momentum and structure. Short, memorable, and cross-tool (not Copilot-specific).

> "Set up Runway in your repo to get Defra-compliant AI from day one."

### Option 2: **Rails**
Direct reference to "rules and rails" — the concept that AI tools need constraints to be useful in a regulated environment. Concise and honest about what the repo does: it gives AI the rails to run on.

> "Defra AI Rails — guardrails for AI coding assistants in government services."

### Option 3: **Blueprint**
A blueprint is what you hand to builders before they start. This template is the AI coding equivalent — a standard layout before work begins. Familiar to government and procurement professionals.

> "The Defra Copilot Blueprint — encode your standards before the AI writes a line."

### Option 4: **GUIDE**
An acronym: **G**overnment-grade **U**nified **I**nstructions for **D**eveloper **E**nvironments. Subtle nod to the public service context. Can be used as a proper noun: "the GUIDE template."

### Option 5: **Scaffold**
Scaffolding is temporary structure that enables permanent construction — exactly what this repo does. It frames the project for the AI before stepping aside. Aligns with the `scaffold-repo` prompt already in the repo.

> "Defra AI Scaffold — the structure that lets Copilot build government-standard code."

**Recommendation:** **Rails** is the most direct expression of the repo's purpose. It is short enough to remember, honest about the mechanism (rules and constraints), and does not imply a single tool. "Defra AI Rails" or "Defra Copilot Rails" both work.

---

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
