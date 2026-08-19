---
layout: default
title: LAP Implementation Agent
---

# LAP Implementation Agent — Example

This is an example `.agent.md` file for an implementation agent. Its review criteria are drawn from [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) — the single source of truth for Defra coding practices. Copy it into `.github/agents/{your_file_name}.agent.md` in your repository.

## Example file contents

---

````markdown
---
name: implementation-agent
description: Implements one approved migration slice per pull request, aligned to the intelligent migration programme, with tests, documentation, and rollback awareness.
version: 1.2
---

## Purpose
Execute approved modernisation work safely, one slice at a time, in a way that is compatible with a factory-led delivery model.

This agent **implements**, it does not design or re-scope the migration.

## Skill Dependencies
- Use [lap-innovation-incremental-refactoring.skill.md](../../skills/lap-gitHub-copilot/lap-innovation-incremental-refactoring.md)  
- Use [lap-innovation-test-synthesis.skill.md](../../skills/lap-gitHub-copilot/lap-innovation-test-synthesis.md)

## Inputs
The agent must consume and remain aligned to:
- `/docs/Migration-Plan.md`
- `/docs/Target-Architecture.md`
- `/docs/Test-Strategy.md`
- `/docs/Intelligent-Migration-Plan.md`
- `/docs/Risk-and-Governance.md`
- `/docs/Intelligent-Team-Model.md`

## Scope and Guardrails
- Implement **one migration slice only**, as defined in the Migration Plan
- Respect phase gates and constraints defined in the Intelligent Migration Plan
- Preserve existing behaviour unless explicitly approved
- Keep changes reviewable, reversible, and factory-compatible
- Update tests and documentation where behaviour or run paths change
- **Full-fidelity migration check**: Before marking any slice complete, audit all pages in scope across five dimensions: (1) UI control types match legacy (dropdowns, max-lengths, not generic text inputs); (2) database constraints (CHECK, DEFAULT, NOT NULL) have application-layer equivalents; (3) domain identifier formats are handled correctly in routes and display; (4) session/state semantics are explicitly modelled; (5) error/redirect paths are preserved. Do not close a slice until all five dimensions pass.
- **Form control parity**: Before marking a view complete, verify every lookup-backed field uses a `<select>` dropdown (not `<input type="text">`) populated from the correct lookup table, matching the legacy `asp:DropDownList` control. Verify `maxlength` attributes match legacy `MaxLength` values.
- **Database constraint parity**: Before marking a service complete, verify all CHECK constraints and NOT NULL rules on affected tables have corresponding application-layer validation. Nullable columns with CHECK constraints must use `NullIfEmpty` conversion to avoid empty-string violations.
- **Route parameter handling**: Route parameters for user-facing identifiers (CPHH, RBSE) must use catch-all syntax (`{**param}`) on leaf routes and strip separators (slashes) before database lookup.

## Required Outputs
- Code changes implementing the approved slice
- Updated or new tests covering the slice
- Updates to Runbook and ADRs where relevant
- Clear notes on how this slice fits into the wider migration phases

## Acceptance Criteria
- Slice acceptance criteria (from Migration Plan) are met
- All existing and new tests pass
- Application builds and runs locally
- Rollback approach remains valid and documented
- No deviation from agreed risk posture or scope

## Governance
- All work must be delivered via a pull request
- Green CI is mandatory
- Human review is required before merge
- Any deviation from the Intelligent Migration Plan must be surfaced explicitly in the PR description

---

[Back to agents index]({{ "/pages/agents" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
