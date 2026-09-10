# Module 06: Skills

This module builds on the always-on instruction files from [Module 01](../01-context-and-cost/README.md) and the MCP servers from [Module 05](../05-mcps-and-integrations/README.md) — skills are the on-demand counterpart to both — and leads to [Module 07](../07-agent-roles-and-orchestration/README.md), where custom agents combine skills, tools, and MCP servers into role-specific workflows.

## Learning Objectives
- Discover existing skills from the skill ecosystem
- Use a simple knowledge skill (frontend-design) to change agent behavior
- Use a tool skill (Playwright CLI) for browser automation tasks
- Use a tool skill (Hurl) for API testing tasks
- Scaffold a custom skill with the built-in `/create-skill` command
- Install an agent plugin and understand how plugins bundle skills, MCP servers, and agents for team distribution

## Key Concepts

Skills are reusable, packaged capabilities defined in a `SKILL.md` file inside a named folder (e.g., `.github/skills/pdf/SKILL.md`). The frontmatter requires `name` and `description` — these are critical because skills are progressively loaded: on the first pass, only the name and description reach the model. A skill can bundle scripts, templates, and reference docs as separate files in the skill folder, referenced via relative paths from `SKILL.md`.

**Important:** The agent does not always load a skill automatically. For reliable results, trigger the skill via slash commands. Don't rely on the agent discovering the right skill on its own.

The skill ecosystem at [skills.sh](https://skills.sh) and [github.com/github/awesome-copilot](https://github.com/github/awesome-copilot) provides community and official skills. Skills differ from other instruction mechanisms: instruction files set project-wide conventions (loaded every prompt), prompt files are reusable prompts and multi-step workflows, custom agents define behavioral workflows, and skills teach the agent new capabilities on demand.

Skills are also the building block of **agent plugins** (Agent Plugins 1.0, an open standard): installable packages that bundle skills and MCP servers — plus Copilot-specific agents, commands, and hooks — and are discovered from marketplaces like Awesome Copilot, which VS Code knows about out of the box. Where a lone skill folder is copied into a repo, a plugin is installed, versioned, and shared across a team. Exercise 6.6 covers this.

> **Prerequisite:** Skills need no setting — VS Code discovers `.github/skills/` folders by default. Agent plugins, however, require the `chat.plugins.enabled` setting, which is **off by default**; enable it in VS Code Settings before Exercise 6.6. Your organization may also restrict which plugin marketplaces are available.

> **Tip:** Skill creation is built in: the `/create-skill` command walks you through a properly structured `SKILL.md` interactively, and the Agent Customizations editor (Configure Chat gear → Skills → New Skill) scaffolds one from the UI. Exercise 6.5 uses `/create-skill` hands-on.

> **Tip:** Skills can also bundle scripts. If you find yourself repeating the same skill interaction, ask the agent to formalize it into a script and add it to the skill folder — or wrap it as a new, more specific skill.

## Relevant Documentation
- [Skills for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills) — Skill folder structure, SKILL.md format, and how skills are loaded
- [Agent plugins in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-plugins) — Plugin structure, marketplaces, and settings
- [Agent Plugins 1.0 announcement](https://github.blog/changelog/2026-08-12-agent-plugins-1-0-in-vs-code-copilot-cli-and-the-copilot-app/)

## Exercises

> **Note:** Exercises 6.3 (Playwright CLI) and 6.4 (Hurl) use specific CLI tools as examples of tool skills. If you don't have these tools installed or they don't fit your workflow, you don't necessarily need to run them — but it's worth reading through the exercises and skill files to understand how tool skills are structured. You can always substitute a CLI tool that better matches your stack by scaffolding your own skill with `/create-skill`, as shown in Exercise 6.5. Also note that VS Code agents now ship with built-in browser tools (open pages, click, type, screenshot, console errors — enabled by default); Exercise 6.3 explains when the Playwright CLI skill still adds value over them.

| Exercise | Description |
|----------|-------------|
| [01-explore-skills](exercises/01-explore-skills.md) | Discover skills on skills.sh and understand how they're structured — nothing installed yet |
| [02-use-frontend-design-skill](exercises/02-use-frontend-design-skill.md) | Install a simple knowledge skill and see how it changes agent design output |
| [03-use-playwright-cli-skill](exercises/03-use-playwright-cli-skill.md) | Use the Playwright CLI skill for browser automation |
| [04-use-hurl-api-testing-skill](exercises/04-use-hurl-api-testing-skill.md) | Use the Hurl API testing skill for backend API testing |
| [05-create-a-custom-skill](exercises/05-create-a-custom-skill.md) | Scaffold a custom skill with the built-in `/create-skill` command; optionally compare with the community skill-creator meta-skill |
| [06-install-agent-plugin](exercises/06-install-agent-plugin.md) | Install an agent plugin from a marketplace and see how plugins bundle skills, MCP servers, and agents (requires `chat.plugins.enabled`) |
