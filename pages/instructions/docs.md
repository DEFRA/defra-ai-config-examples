---
layout: default
title: Documentation Instructions
---

# Documentation Instructions — Example

This is an example scoped instruction file for documentation. It activates when Copilot is working on Markdown files. Copy it into `.github/instructions/docs.instructions.md` in your repository.

## Example file contents

The content below goes into your `.github/instructions/docs.instructions.md` file:

---

````markdown
---
applyTo: "**/*.md"
---

# Documentation

## Writing style

- Use plain English — common words, sentences under 25 words, active voice
- Use British English spelling throughout (organisation, colour, behaviour, licence)
- Address the reader as "you" — direct and consistent with GOV.UK style
- Start instructions with verbs — "Run the command" not "The command should be run"
- Avoid: "please", "simply", "just", "easy", "straightforward"
- Use consistent terminology — define technical terms on first use

## README

Every service repository must have a `README.md` in the root with these sections in order:

```markdown
# Service name

Brief description of what the service does and who uses it.

## Prerequisites

List what must be installed before running the service locally.

## Getting started

Step-by-step local setup instructions.

## Running tests

How to run the test suite and check coverage.

## Environment variables

Table of all required and optional environment variables, with descriptions and example values.

## Contributing

Link to CONTRIBUTING.md or notes on branch naming, commit format, and PR process.

## Licence

State the licence (Open Government Licence v3 for Defra services).
```

## Architecture Decision Records (ADRs)

Use the standard ADR format. Store ADRs in `docs/adr/` numbered sequentially
(e.g. `docs/adr/0001-use-hapi-for-http-server.md`).

```markdown
# ADR-NNNN: Title

## Status

Proposed | Accepted | Deprecated | Superseded by ADR-NNNN

## Context

Why did this decision need to be made?

## Decision

What was decided?

## Consequences

What are the results of this decision — both positive and negative?
```

## JSDoc comments (JavaScript projects)

Write JSDoc comments for all exported functions and classes:

```javascript
/**
 * Retrieves a permit application by its reference number.
 *
 * @param {string} referenceNumber - The unique reference number assigned at submission
 * @returns {Promise<Application|null>} The application, or null if not found
 * @throws {DatabaseError} When the database connection fails
 */
export async function getApplicationByReference(referenceNumber) {}
```

- Place the comment immediately above the function — no blank line between comment and function
- Use `@param` for every parameter with type and description
- Use `@returns` with the return type and description
- Use `@throws` for documented error conditions
- Describe intent, not mechanics — do not restate what the code already does

## XML doc comments (C# projects)

```csharp
/// <summary>
/// Retrieves a permit application by its reference number.
/// </summary>
/// <param name="referenceNumber">The unique reference number assigned at submission.</param>
/// <returns>The application, or <see langword="null"/> if not found.</returns>
/// <exception cref="DatabaseException">Thrown when the database connection fails.</exception>
public async Task<Application?> GetApplicationByReferenceAsync(string referenceNumber) {}
```

## Inline comments

Write inline comments only where the logic is not self-evident:

✅ Good — explains why:

```javascript
// Delay is required because the upstream API enforces a 100ms rate limit per request
await delay(100)
```

❌ Bad — restates what the code already says:

```javascript
// Increment counter by 1
counter++
```

## What to avoid

- Do not use placeholder text ("lorem ipsum", "TODO: fill this in")
- Do not commit empty doc stubs — write real content or omit the comment
- Do not copy documentation from another service without verifying it accurately describes this one
- Do not include internal Defra system names, internal URLs, or OFFICIAL-SENSITIVE information in public repositories
````

---

[Back to instructions index]({{ "/pages/instructions" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
