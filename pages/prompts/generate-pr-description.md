---
layout: default
title: Generate PR Description Prompt
---

# Generate PR Description Prompt — Example

This is an example `.prompt.md` file for generating a structured pull request description from the current branch changes. Copy it into `.github/prompts/generate-pr-description.prompt.md` in your repository.

## Example file contents

The content below goes into your `.github/prompts/generate-pr-description.prompt.md` file:

---

````markdown
---
description: Generate a pull request description from the current branch changes
---

# Generate PR Description

Generate a pull request description for the changes on the current branch. Follow Defra PR conventions and GOV.UK plain English style.

## Context to gather first

Before generating the description, run these commands to understand the change:

1. `git diff main...HEAD --stat` — see which files changed and how many lines
2. `git log main...HEAD --oneline` — see the commit messages
3. Read the changed files to understand what the code does and why

## Output format

Write a PR description with this structure:

```markdown
## Summary

[2–3 sentences describing what this PR does and why, in plain English.
Start with the action — "Adds...", "Fixes...", "Updates..." — not "This PR..."]

## Changes

- [One bullet per logical change — what was added, modified, or removed]
- [Describe behaviour, not files — "Adds validation for empty form submissions" not "Updates handler.js"]

## Testing

- [ ] Unit tests added or updated
- [ ] Integration tests added or updated (if applicable)
- [ ] Journey tests added or updated (if applicable)
- [ ] All existing tests pass (`npm test`)
- [ ] Coverage does not decrease from baseline

## Checklist

- [ ] Linter passes (`npx eslint .`)
- [ ] Code is formatted (`npx prettier --check .`)
- [ ] No PII in logs or error messages
- [ ] No secrets or credentials committed
- [ ] SonarCloud quality gate passes
- [ ] Documentation updated (README, ADR, JSDoc) where needed

## Related

Closes #[issue number]
```

## Style rules

- Use plain English — GOV.UK style, active voice, British spelling
- Keep the summary under 60 words
- Do not list file names in the summary — describe the behaviour being changed
- If there is no linked issue, remove the "Related" section entirely
````

---

[Back to prompts index]({{ "/pages/prompts" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
