# Module 05: MCP Servers and Integrations

This module builds on the tool-management guardrails from [Module 02](../02-guardrails-and-safety/README.md) — which tools the agent is allowed to use — by adding new tools from outside VS Code, and leads into [Module 06](../06-skills/README.md), where skills and agent plugins teach the agent *how* and *when* to use them.

## Learning Objectives

- Understand what MCP servers are and how they extend Copilot with external tools
- Configure MCP servers at the project level so the whole team shares the same setup
- Choose the right MCP servers for your team's stack and workflow

> **Prerequisite:** MCP servers require the **"MCP servers in Copilot"** policy — enabled by your organization or enterprise admin under Copilot policies on GitHub. Enterprises may additionally enforce an MCP allowlist (via the managed settings file) or a private MCP registry, which can block community servers such as the Context7 server used in Exercise 5.1. Check with your admin before the session.

## Key Concepts

MCP (Model Context Protocol) servers expose external tools to the Copilot agent via a standard protocol — documentation lookup, database queries, issue tracking, browser automation, and more. Each tool description from every MCP server is injected into the agent's context window, so fewer irrelevant tools means better tool-selection accuracy and more tokens for the actual task. The protocol keeps evolving: the emerging [MCP Apps extension](https://modelcontextprotocol.io/extensions/apps/overview) lets a tool call return an interactive UI component that renders directly in chat instead of plain text — client support is still rolling out, so treat it as a preview of where MCP is heading rather than something to rely on today.

**Project-level config is preferred over global.** Project-level config lives in `.vscode/mcp.json` — commit this file so every contributor gets the same tools without manual setup. Global config adds every tool to every project, wasting tokens and degrading tool selection.

Servers can be installed from the **MCP Registry** in VS Code's Extensions panel (`@mcp`), or configured in `.vscode/mcp.json`. See **[MCP_REFERENCE.md](MCP_REFERENCE.md)** for the full server list.

> **Note:** The GitHub MCP Registry integration in VS Code is in public preview and may change. No setting needs to be enabled.

> **Note:** MCP servers increasingly arrive bundled inside **agent plugins** ([Agent Plugins 1.0](https://github.blog/changelog/2026-08-12-agent-plugins-1-0-in-vs-code-copilot-cli-and-the-copilot-app/)) — a plugin can ship an MCP server together with the skill that knows how to use it, installed as one unit from a marketplace. [Exercise 6.6](../06-skills/exercises/06-install-agent-plugin.md) in Module 06 covers plugins; the committed `.vscode/mcp.json` remains the right tool for project-specific server config.

## Relevant Documentation
- [Extend Copilot Chat with MCP](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp) — GitHub Docs: prerequisites, policy, and setup in VS Code
- [VS Code MCP docs](https://code.visualstudio.com/docs/agent-customization/mcp-servers)
- [MCP server registry](https://registry.modelcontextprotocol.io/) — Community MCP server directory
- [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) — Official MCP servers GitHub repo

## Security Note

> **Warning:** MCP servers can access external services, APIs, and data sources. Before installing or configuring any MCP server, make sure it complies with your company's security policies, data governance rules, and approved tooling lists. Some servers may send code or context to third-party endpoints — always verify where data flows and get approval from your security team if in doubt.

## Exercises

| Exercise | Description |
|----------|-------------|
| [01-find-and-configure-mcps](exercises/01-find-and-configure-mcps.md) | Install a project-level MCP server, verify it, and measure the context cost of its tools |
