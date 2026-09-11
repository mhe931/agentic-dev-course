# Exercise 6.6: Install an Agent Plugin

## Objective
Install an agent plugin from a marketplace, see what it bundles, and understand how plugins solve the "how does my whole team get this setup?" problem.

## Background
In this course you've configured capabilities one mechanism at a time: instruction files ([Module 01](../../01-context-and-cost/README.md)), hooks and a first custom agent ([Module 02](../../02-guardrails-and-safety/README.md)), prompt files ([Module 04](../../04-commands-and-prompt-files/README.md)), MCP servers in `.vscode/mcp.json` ([Module 05](../../05-mcps-and-integrations/README.md)), and skills in skill folders (this module). Custom agents come back in depth in Modules 07–08. **Agent plugins** package all of these into a single installable unit.

Agent Plugins 1.0 is an open standard — published by GitHub together with AWS, Anysphere, Microsoft, OpenAI, and Vercel, with Google joining as a core maintainer — so the same plugin works across compatible tools (VS Code, Copilot CLI, and beyond). A plugin's structure mirrors what you already know:

```
my-plugin/
├── plugin.json              # manifest
├── skills/                  # skills (portable across tools)
├── mcp.json                 # MCP server config (portable)
└── com.github.copilot/      # Copilot-specific: custom agents,
                             # slash commands, hooks, rules
```

The portable parts (skills, MCP servers) work in any compliant client; Copilot-specific pieces like `.agent.md` files and hooks travel along in the namespaced directory. Plugins are discovered from marketplaces — the community **Awesome Copilot** marketplace is available by default, and teams can register their own via the `extraKnownMarketplaces` field in `.github/copilot/settings.json`. This is the natural evolution of the [awesome-copilot](https://github.com/github/awesome-copilot) repo linked from Module 04's README: from copy-paste files to versioned, installable packages.

> **Warning:** A plugin can bundle MCP servers, hooks, and scripts — everything the Module 05 security warning applies to, in one package. Review what a plugin contains and where its tools send data before installing, and stick to marketplaces your organization has approved.

## Prerequisites
> **Prerequisite:** Agent plugins require the `chat.plugins.enabled` setting, which is **off by default** — enable it in VS Code Settings (search for "plugins") and reload. Plugin tooling is new (the 1.0 spec shipped in August 2026) and the discovery UI may differ between VS Code versions; your organization may also restrict which marketplaces are available.

## Steps

1. **Browse the marketplace.** In the Extensions view, filter with `@agentPlugins` (add `@recommended` to narrow it down) and skim what's on offer. Notice how plugins describe their contents — skills, MCP servers, agents — like the skill listings you explored in [Exercise 6.1](01-explore-skills.md), but bundled.
2. **Pick and install one plugin** relevant to your stack. Good candidates are ones whose parts you can map to course concepts — e.g., a testing or deployment plugin that bundles a skill plus an MCP server.
3. **Inspect what arrived.** Ask the agent:

   > List the plugins currently installed and what each one contributes — skills, MCP servers, agents, commands, or hooks. Where do the files live?

   Cross-check against the structure diagram above.
4. **Use it.** Trigger one capability the plugin provides — invoke its skill or slash command, or let the agent use its MCP tools on a small task in your project.
5. **Reflect on team distribution.** Compare three ways your team could share the setup you've built in this course: committing config files to the repo (Modules 01–05), pointing colleagues at files to copy (awesome-copilot style), or packaging it as a plugin in a team marketplace. Which parts of *your* course setup — skills, agents, hooks, MCP config — would be worth packaging?
6. **(Optional) Package your own.** Ask the agent to scaffold a plugin from what you've already built:

   > Create an Agent Plugins 1.0 plugin from this project's setup: include our skill from Exercise 6.5 and our MCP config, and put our custom agents and hooks in the com.github.copilot directory. Generate the plugin.json manifest.

   To test it locally, do a one-time VS Code settings step: open Settings, search for `chat.pluginLocations` (experimental), and add the plugin folder so VS Code discovers it without a marketplace.

## Expected Outcome
A plugin installed from a marketplace and its capabilities exercised, plus a clear picture of how plugins bundle the mechanisms from Modules 01–06 into one shareable, versioned unit — and which parts of your own setup deserve that treatment.
