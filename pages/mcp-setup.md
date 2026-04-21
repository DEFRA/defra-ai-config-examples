---
layout: default
title: MCP Setup
---

# MCP Setup — Configuring MCP Servers with GitHub Copilot

Model Context Protocol (MCP) servers extend what Copilot agents can do by giving them tools that reach outside the codebase — reading GitHub issues and pull requests, running Playwright tests, querying Azure DevOps work items, and more.

This page explains how to configure MCP servers for use with GitHub Copilot agent mode, and how to reference MCP tools in your agent files.

## Defra MCP guidance

Before enabling any MCP server, check the [Defra MCP guidance](https://defra.github.io/defra-ai-sdlc/pages/appendix/defra-mcp-guidance/){:target="_blank"} (opens in new tab) for the current approved list, data handling assessments, and any restrictions.

**Only use MCP servers from the Defra-approved list.** Do not enable community or self-built MCP servers on Defra projects.

## How MCP works with Copilot

When you configure an MCP server in `.vscode/mcp.json`, its tools become available in Copilot's agent mode. Agents can then list those tools in their `tools` frontmatter array and call them during a task without leaving the chat panel.

For example, with the GitHub MCP server configured, an agent can read open issues, search for pull requests, and post a review comment — all from within Copilot Chat.

## Configuration file

Configure MCP servers for your repository in `.vscode/mcp.json`. This file is workspace-scoped — it applies to everyone who opens the repository in VS Code with Copilot.

Commit `.vscode/mcp.json` to your repository so the configuration is shared across the team.

### Example `.vscode/mcp.json`

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "gallery": true
    },
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

> **Before committing this file**, confirm each server is on the [Defra approved list](https://defra.github.io/defra-ai-sdlc/pages/appendix/defra-mcp-guidance/){:target="_blank"} (opens in new tab) and review how it handles your data. Remove server entries that your project does not need.

## Referencing MCP tools in agents

Once you configure an MCP server, reference it in the `tools` array of your agent's frontmatter using the server key from `mcp.json`:

```markdown
---
description: Builds Defra-compliant applications with GitHub integration
tools: [codebase, editFiles, runTerminal, thinking, github]
---
```

The `github` entry here matches the `"github"` key in the `mcp.json` `servers` object.

## Common tool combinations

These examples use VS Code **tool sets** (`edit`, `execute`, `read`, `search`, `web`, `agent`) — each one bundles all the related built-in tools (e.g. `execute` includes `runInTerminal`, `createAndRunTask`, `testFailure`, `runNotebookCell`, `getTerminalOutput`). Using tool sets gives an agent full Agent-mode capability in one entry instead of a long brittle list, and is the recommended pattern in the [official VS Code chat tools reference](https://code.visualstudio.com/docs/copilot/reference/copilot-vscode-features#_chat-tools){:target="_blank"} (opens in new tab).

| Use case | Tools to include |
|----------|-----------------|
| Full autonomous developer (default) | `edit, execute, read, search, web, findTestFiles, githubRepo, usages, changes, todos, thinking` |
| Tester (TDD, runs and fixes test failures) | `edit, execute, read, search, web, findTestFiles, usages, changes, todos, thinking` |
| Read-only reviewer | `read, search, web, findTestFiles, githubRepo, usages, changes, thinking` |
| Frontend / accessibility (no terminal) | `edit, read, search, web, usages, changes, todos, thinking` |
| Orchestrator with subagents | Add `agent` |
| Repo / workspace scaffolding | Add `newWorkspace, vscode/extensions, vscode/getProjectSetupInfo, vscode/installExtension, vscode/runCommand` |
| With GitHub MCP integration | Add `github` |
| With Playwright end-to-end testing | Add `playwright` |

### What the tool sets bundle

- `edit` — `createDirectory`, `createFile`, `editFiles`, `editNotebook`
- `execute` — `runInTerminal`, `createAndRunTask`, `testFailure`, `runNotebookCell`, `getTerminalOutput`
- `read` — `readFile`, `problems`, `terminalLastCommand`, `terminalSelection`, `getNotebookSummary`, `readNotebookCellOutput`
- `search` — `codebase`, `fileSearch`, `textSearch`, `listDirectory`, `usages`, `changes`
- `web` — `fetch`
- `agent` — `runSubagent` (delegate to other agents)

### Other built-in tools worth knowing

- `todos` — visible progress checklist (essential for any multi-step agent)
- `newWorkspace` — scaffold a new VS Code workspace
- `selection` — current editor selection
- `vscode/extensions`, `vscode/installExtension`, `vscode/runCommand`, `vscode/getProjectSetupInfo` — VS Code-specific scaffolding helpers

VS Code [silently ignores tool names it does not recognise](https://code.visualstudio.com/docs/copilot/customization/custom-agents#_custom-agent-file-structure){:target="_blank"} (opens in new tab), so you can safely add MCP server tools (e.g. `github`, `playwright`) and extension-contributed tools without breaking the agent if they aren't installed.

## Security considerations

- MCP servers act with the permissions of the authenticated user — they can read and write to the connected service
- Do not configure MCP servers with access to production systems on development machines
- Review what data each MCP server sends to its backend before enabling it on a project handling sensitive or OFFICIAL-SENSITIVE data
- MCP server credentials must never be committed to source control — use environment variables or VS Code user settings for any required tokens
- The `.vscode/mcp.json` file itself should contain no secrets — only server configuration (URLs, commands, arguments)

## MCP and multi-agent workflows

MCP tools significantly extend what you can achieve with the [Orchestrator agent]({{ "/pages/agents/orchestrator" | relative_url }}). An orchestrator can use the GitHub MCP tool to read an issue description, delegate implementation to `@defra-app-developer`, then use the same tool to draft a pull request — without leaving the Copilot chat panel.

The Playwright MCP server is particularly useful with the [Tester agent]({{ "/pages/agents/tester" | relative_url }}), giving it the ability to run end-to-end tests directly and report results back into the conversation.

[Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
