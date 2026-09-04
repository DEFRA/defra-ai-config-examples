---
layout: default
title: LAP Analysis and Documentation Depth Instructions
---

# LAP Analysis and Documentation Depth Instructions — Example

This page shows the LAP analysis and documentation depth instruction file for GitHub Copilot.

## Example file contents

````markdown
---
description: "Rebuild-grade depth standard for every LAP analysis and the PRD: exhaustive, quantified, evidence-cited enumeration; grounding labels; and the discrepancy (D), findings (S), functionality-loss risk (R), open-question (OQ) and deferred-source (G) registers that make the documentation pack detailed enough to rebuild a legacy application from without losing functionality."
applyTo: "output/**,**/features/**,**/*PRD*.md,**/*-analysis.md"
---

# LAP Analysis & Documentation Depth Standard

Every analysis document and the PRD must be **detailed enough to rebuild the legacy application
from, without losing functionality**. Breadth (naming a capability exists) is not enough — each
artefact must have the **depth** that lets a developer implement it. Use British English throughout.

These rules apply on top of the delivery standards in
[lap-delivery-standards.instructions.md](lap-delivery-standards.instructions.md). Where an agent's
own template lists sections, these rules govern **how deep** each section must go.

## 1. Enumerate exhaustively — never summarise the detail away

- List **every** discrete item, not a representative sample: every screen, every field on every
  screen, every route/endpoint, every business rule, every validation rule, every table, every
  column, every stored procedure/parameter, every enum value, every report column, every
  integration message.
- **"etc.", "and so on", "such as", "e.g. …"** and truncated lists are not acceptable where a
  complete list is knowable from the source. If you find yourself abbreviating a set, enumerate it
  instead — in a table if it is long.
- Capture **exact values verbatim**: validation messages, error text, formulas, default values,
  regular expressions, file/record layouts, field lengths, status codes, permission names. Where the
  PRD summarises, the analysis must specify.

## 2. Quantify everything

- Give a **count** for every enumerated set (e.g. "Screens (137)", "Routes (147)", "Validation
  rules (261)", "Tables (90)"). Put a headline-figures line near the top of each analysis.
- Keep counts **canonical and consistent** across documents — the same set has the same number
  everywhere. Derive numbers mechanically (count the rows) rather than estimating; never round or
  guess. If two sources disagree on a count, record it as a discrepancy (§5) rather than picking one.

## 3. Cite evidence for every claim

- Every rule, field, workflow step and figure carries a **source citation**: the file path and,
  where practical, the line or symbol (e.g. `src/App/Billing.vb:212`, `SP usp_CalculateCharge`,
  `output/html/finance-invoice.html`). A reader must be able to verify any claim without re-reading
  the whole codebase.
- **Label how grounded each statement is:** `doc-grounded` (from approved documentation),
  `code-grounded` / `db-grounded` (from source), or `inferred` (interpretation). Flag every
  **inferred** item so reviewers know what still needs confirming.

## 4. Assign and preserve stable identifiers

Maintain the traceability chain. Assign stable, unique IDs and reuse them everywhere:

- `BR-xxx` application/business rules · `DB-BR-xxx` database rules · `DR-xxx` domain rules ·
  `VR-xxx` validation rules · `WF-xxx` workflows · `SC-xxx` screens · `REQ-xxx` other requirements.
- IDs never change meaning between runs. Every capability that must survive the rebuild carries one.

## 5. Keep the five registers (no silent loss of functionality)

Consolidated decision and risk tracking is what proves nothing is dropped by accident. Each analysis
contributes to, and the PRD/Open Items Register consolidates, these registers. Give every entry an ID,
a plain statement, the **owner** best placed to resolve it, and a **⚠ must-confirm** flag when it
moves money, safety, security or scope:

| Register | ID prefix | What it records |
|----------|-----------|-----------------|
| **Discrepancies** | `D-` | Where approved documentation and the delivered code/database disagree. Record both positions, the evidence, and the decision the rebuild must take. *(Only when reference documentation is available to compare against.)* |
| **Findings** | `S-` | Defects, security gaps and risks noticed **during extraction** — e.g. a route with no permission check, an empty validator, dead/unreachable logic, a hard-coded secret, money handled as float. State the evidence and the risk. |
| **Functionality-loss risks** | `R-` | Capabilities that may be **lost or are already broken** — behaviour that is hard to see, scheduled jobs, edge-case rules, anything a naive rebuild would silently drop. |
| **Open questions** | `OQ-` | Questions needing a person or a live-data lookup before the answer is certain. |
| **Deferred / unreadable sources** | `G-` | Inputs that could not be read (encrypted, blocked, video, missing) and what is therefore unverified, plus the revalidation trigger. |

Never delete an item to make a document look finished — resolve it, or carry it with an owner.

## 6. Reconcile, do not duplicate blindly

When several sources describe the same thing, produce **one** reconciled statement that cites all
sources. Where they conflict, keep both and open a `D-` or `OQ-` entry — do not silently pick one.

## 7. Depth is not licence to fabricate

Exhaustiveness must stay evidence-bound. If the detail is not in the source, say so and open an
`OQ-`/`G-` entry — never invent a validation message, formula, count or rule to fill a table.
"I could not determine X from the available material" is a valid, useful entry; a fabricated value
is not.
````

[Back to GitHub Copilot instructions]({{ "/pages/instructions/lap-gitHub-copilot" | relative_url }}) · [Back to Instructions]({{ "/pages/instructions" | relative_url }})
