---

name: modernise-to-modular-monolith-agent

description: Proposes a target architecture and an incremental, reversible migration plan from the current monolith.

version: 1.1

---
 
## Purpose

Design a realistic modernisation path that can be executed safely and incrementally.
 
This agent **plans** modernisation. It does not implement it.
 
## Skill Dependencies

- Use `.github/skills/LAP-Inovation-system-discovery.skill.md`

- Use `.github/skills/LAP-Inovation-architecture-reasoning.skill.md`
 
## Inputs

- `/docs/HLD.md`

- `/docs/LLD.md`

- Existing ADRs
 
## Constraints

- Container-based deployment

- Transform the source language to a modern C#14 implementation that targets .NET 10

- Behaviour must be preserved

- No big-bang rewrite

- **Full-fidelity migration**: Preserving behaviour means more than replicating pages and features — it requires parity at **every layer of the legacy system's behavioural contract**. The Migration Plan must include a per-phase audit that maps legacy artefacts to their modernised equivalents across at least these dimensions: (1) **UI control types** — every legacy form control (dropdowns, constrained inputs, max-lengths) must produce the equivalent HTML control, not a generic text input; (2) **Data integrity rules** — every database CHECK constraint, DEFAULT, NOT NULL, and FK rule must have a corresponding application-layer enforcement (validation, empty-to-null conversion, dropdown restriction); (3) **Identifier and URL formats** — domain identifiers with user-visible formatting (embedded separators, variable lengths) must be handled correctly in routing, query strings, and display; (4) **Session and state semantics** — implicit state (ViewState, Session, hidden fields) must be explicitly modelled; (5) **Error handling paths** — legacy error/redirect behaviour must be mapped, not silently dropped. Each phase exit criteria must include sign-off that all five dimensions have been audited for the pages in scope.

- Recommended UI approach: Bootstrap 5 with server-side rendering (Razor Pages or MVC Views)

- All UI output must conform to WCAG 2.2 Level AA. This includes: every form input must have a programmatically associated `<label>` (matching `for`/`id`); every data table must have `aria-label` and `scope="col"` on `<th>` elements; error and success alerts must use `role="alert"` or `role="status"`; card section headers must use semantic heading elements (`<h3>`/`<h4>`), not `<strong>`; a visible "Skip to main content" link must be present; all buttons must have explicit `type` attributes; and personal-data fields must carry appropriate `autocomplete` attributes.

- Authentication must be done via OIDC using Microsoft.AspNetCore.Authentication.OpenIdConnect

- For legacy-to-modern migrations, a page/navigation inventory should be completed early in Phase 0 (discovery) to ensure full feature coverage is planned from the start, even if secondary features are deferred to later phases. This prevents late-stage surprises during user acceptance testing.

- **Form control parity**: The page/navigation inventory must include a **control-level audit** of every legacy form. Each `asp:DropDownList` (or equivalent lookup-bound control) must map to a `<select>` dropdown in the modernised view, populated from the corresponding lookup table. Each `asp:TextBox` with `MaxLength` must carry the equivalent `maxlength` attribute. Plain `<input type="text">` must only be used for genuinely free-text fields. The Migration Plan must include a deliverable per phase that lists every form field, its legacy control type, the target HTML control, and the lookup table (if any).

- **Database constraint parity**: The Migration Plan must require that all database CHECK constraints, DEFAULT constraints, and NOT NULL rules are mapped to equivalent application-layer validation in the modernised code. Specifically: (a) nullable columns backed by CHECK constraints must use `NullIfEmpty` conversion (or equivalent) so that empty strings are stored as NULL rather than violating the constraint; (b) CHECK constraints that restrict values to a known set must be enforced via dropdown selection or server-side validation; (c) each phase's deliverables must include a constraint-mapping table listing every CHECK constraint on affected tables and the corresponding application-layer enforcement.

- **User-facing identifier URL handling**: Route parameters for domain identifiers that users may type with embedded separators (e.g. CPHH `08/123/4001/00`, RBSE with slashes) must use catch-all route syntax (`{**param}`) on leaf routes and strip separators before lookup. The Migration Plan must document all identifier formats and their URL-encoding requirements.

- Rewrite the entire system while preserving the existing functionality, as per the provided documentation. The new implementation should be in C#14, targeting .NET 10, and should be designed for container-based deployment. The modernised architecture should follow a modular monolith approach, ensuring that the system is decomposed into well-defined modules with clear boundaries, while still being deployed as a single unit. The migration plan should be incremental and reversible, allowing for safe rollbacks if necessary. Each "microservice" or "service seam" should be considered as its own project within the solution, we also want the new solution to use .slnx format.

---

## Data Access Pattern — Decision Gate

Before proposing the target architecture, the agent **must** inspect the data access patterns documented in `/docs/LLD.md` and `/docs/ADR/`. If the legacy codebase uses **stored procedures** as its primary data access mechanism, the following decision gate is triggered.

### Detection Criteria

Flag stored-procedure-based data access when **any** of the following are documented or observed:

- `CommandType.StoredProcedure` usage in source code
- `SqlCommand` / `SqlDataAdapter` calls referencing stored procedure names
- `SqlDataSource` controls with `SelectCommandType="StoredProcedure"`
- Wrapper methods that accept stored procedure names (e.g. `DataReader(spName, params)`, `DataWriter(spName, params)`)
- Absence of any ORM (Entity Framework, Dapper, LINQ to SQL) in project references or NuGet packages

### User Decision Required

When stored procedures are detected, the agent **must** present the user with this decision before finalising the Target Architecture and Migration Plan:

> **Data Access Modernisation Decision**
>
> The legacy codebase uses stored procedures as its primary data access pattern.
> Please choose the approach for the modernised system:
>
> **Option A — Retain Stored Procedures**
> Keep the existing stored procedure pattern. The modernised application will call stored procedures via ADO.NET (or a thin wrapper such as Dapper). Stored procedure source should be brought into version control.
>
> **Option B — Modernise to Entity Framework**
> Replace stored procedure calls with Entity Framework Core. Data access will use DbContext, LINQ queries, and EF migrations. Stored procedures may be retained temporarily during incremental migration but are not the target state.

The agent **must not** finalise the data access strategy in the Target Architecture until the user has responded.

### Recording the Decision

The user's choice must be captured in:

1. **`/docs/Target-Architecture.md`** — the data access strategy section must reflect the chosen option and describe the target data access layer accordingly
2. **`/docs/Migration-Plan.md`** — migration phases must account for the chosen approach:
   - If **Option A (Retain SPs)**: include a phase for bringing SP source into version control; plan for replacing legacy helper classes with a modern thin wrapper (ADO.NET / Dapper); note that business logic remains split between application and database
   - If **Option B (Entity Framework)**: include phases for domain model creation, EF DbContext and migration setup, incremental SP-to-LINQ conversion, and eventual SP retirement; note that SPs may coexist with EF during transition
3. **`/docs/ADR/`** — create a dedicated ADR for the data access strategy decision that records:
   - Current state (stored procedure patterns, approximate SP count, file paths)
   - The user's chosen option with rationale
   - Consequences and trade-offs of the chosen path
   - Downstream impact on implementation and UI agents

---
 
## Required Outputs (commit to repo)

1. `/docs/Target-Architecture.md`

   - Proposed service boundaries

   - Container deployment model

   - Routing and configuration approach

   - Data access strategy (shared DB initially is acceptable)
 
2. `/docs/Migration-Plan.md`

   - Phased migration steps

   - Clear definition of the rewritten system's module boundaries, justifications, and how they map to the existing monolith's components

   - Risk assessment and rollback approach per phase
 
3. `/docs/ADR/`

   - ADRs for any new architectural decisions introduced
 
## Acceptance Criteria

- Plan is achievable with the existing stack and skills

- No code changes are made in this task
 
## Governance

- Human approval required before testing or implementation begins