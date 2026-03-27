---
layout: default
title: .copilotignore and Advanced Configuration
---

# .copilotignore and Advanced Configuration

This page covers content exclusion using `.copilotignore` and a few other configuration settings that complement the main file types (instructions, agents, prompts, and skills).

## .copilotignore

A `.copilotignore` file tells Copilot which files to exclude from its context. Place it in the root of your repository. It uses the same syntax as `.gitignore`.

When a file matches a `.copilotignore` pattern, Copilot will not read it as context for completions or chat, even if the file is open in the editor. It will still complete changes to the file if you explicitly ask it to.

This is useful on government projects where some files contain sensitive data, real credentials, or OFFICIAL-SENSITIVE content that you do not want included in any AI inference call.

### What to exclude on government projects

Always add the following to your `.copilotignore`:

```
# Environment files with real values
.env
.env.*
!.env.example

# Private keys and certificates
*.pem
*.key
*.p12
*.pfx
*.crt
secrets/
config/secrets/

# Files that may contain real personal data
test/fixtures/real-data/
fixtures/pii/
test-data/production/
**/*-prod-data.*
**/*-real-data.*

# OFFICIAL-SENSITIVE documents
docs/official-sensitive/
OFFICIAL-SENSITIVE/
*-OFFICIAL-SENSITIVE.*

# Azure and cloud credentials
.azure/
.aws/credentials
service-account-key.json

# Generated secrets
*-secret-*
*credentials*.json
```

Adjust paths to match your project's structure. The `!` prefix allows a pattern after a broader exclusion (for example, `!.env.example` re-includes the committed example file after excluding `.env.*`).

### What .copilotignore does not do

- It does not prevent Git from tracking those files (use `.gitignore` or `.gitattributes` for that)
- It does not prevent Copilot from editing a file when you explicitly instruct it to
- It does not replace the need to avoid committing secrets — never commit real credentials regardless of this file

## Pinning a model in agent files

Individual `.agent.md` files support a `model` frontmatter key. This pins the agent to a specific model version for consistent results.

```markdown
---
description: Detailed security audit agent for OWASP and UK data classification review
tools:
  - codebase
  - terminal
model: gpt-4o
---
```

Omit the `model` key if you want the agent to use the user's current default model. Pinning is most useful for agents where predictable, consistent behaviour matters more than using the latest available model.

Supported model identifiers depend on your Copilot subscription and the models your organisation has approved. Check [GitHub Copilot model availability](https://docs.github.com/en/copilot/using-github-copilot/ai-models/changing-the-ai-model-for-copilot-chat){:target="_blank"} (opens in new tab) for current options.

## Enabling Agent Skills

Agent Skills require a VS Code setting before they activate. Without this, Copilot ignores all `.github/skills/` files.

Enable in VS Code settings:

1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Run **Preferences: Open User Settings (JSON)**
3. Add: `"chat.agent.skills": true`

Or use the Settings UI: search for **chat.agent.skills** and tick the box.

This setting must be enabled by each developer individually — it is a user-level setting, not a workspace setting. You can document it as a required local setup step in your project README.

See [Agent Skills]({{ "/pages/skills" | relative_url }}) for examples and the full skill format reference.

## Plan Mode

GitHub Copilot's Plan Mode (available in VS Code 1.99 and later) lets agents draft a step-by-step plan and show it to you before making any file changes. This is useful for large refactors, scaffolding new features, and any task where you want to review the approach before execution.

To use Plan Mode, open the Copilot Chat panel in Agent Mode and click **Plan** before sending your first message. Review the plan, edit it if needed, then click **Start** to execute.

Plan Mode works with all your custom agents — if you have an `orchestrator.agent.md`, it will produce a plan you can review before any sub-tasks run.

---

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
