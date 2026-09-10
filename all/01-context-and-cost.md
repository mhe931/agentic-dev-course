# Module 01: Context and Cost Management

This module builds on the project you scaffolded in [Module 00](../00-prerequisites/README.md) and leads into the guardrails of [Module 02](../02-guardrails-and-safety/README.md): first you teach Copilot how your project works, then you constrain what it may do.

## Learning Objectives
- Create repository-level Copilot instructions
- Set user-level instructions in your VS Code profile for personal preferences that follow you across projects
- Use path-specific instructions with `applyTo` globs
- Add a tool-agnostic `AGENTS.md` file for cross-tool agent conventions
- Apply best practices for managing context, model selection, and AI credit usage

## Key Concepts
Copilot builds context from open files, instruction files, and conversation history. You shape that context with `.github/copilot-instructions.md` for repo-wide conventions, `.github/instructions/*.instructions.md` files for path-specific guidance (using `applyTo` globs in YAML frontmatter), and user-level instruction files in your VS Code profile for personal preferences. For teams using multiple agent tools, the tool-agnostic `AGENTS.md` standard provides a shared home for agent conventions that Copilot and other agents both read. Well-crafted instructions improve output quality; deliberate context and model selection help you get more out of your AI credits (Copilot's usage-based billing since June 2026). **Auto model selection** routes each request to a model matched to the task and is billed at a discount — hovering over a response shows which model handled it and what it cost.

> **Note:** Instruction files aren't the only persistent context mechanism anymore. **Copilot Memory** (public preview) stores repository-level facts and user-level preferences the agent builds up while working — cited when used, auto-expiring after 28 days. It is currently used by the Copilot cloud agent, Copilot code review, and the Copilot CLI — not by Copilot Chat in VS Code, so you won't see it in this module's exercises. Instruction files remain the right place for deliberate, reviewable conventions; memory captures what the agent learns along the way.

> **Prerequisite:** Copilot Memory is on by default for paid individual plans but off by default for organization- and enterprise-managed plans — an administrator must enable it under Copilot policies → Features → **Copilot Memory**, after which each user can toggle it at github.com/settings/copilot → Features.

> **Tip:** You can scaffold instruction files directly from Copilot Chat: type `/create-instruction` followed by a description of what the instructions should cover, and Copilot generates a properly structured `.instructions.md` file.

## Relevant Documentation
- [Adding custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions) — How to write and place instruction files
- [Auto model selection](https://docs.github.com/en/copilot/concepts/models/auto-model-selection) — How Auto routes requests and affects billing

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-create-copilot-instructions](exercises/01-create-copilot-instructions.md) | Add repo-level instructions to your project |
| [02-personal-instructions](exercises/02-personal-instructions.md) | Set user-level instructions that persist across projects |
| [03-path-specific-instructions](exercises/03-path-specific-instructions.md) | Add targeted instructions for specific paths |
| [04-agents-md](exercises/04-agents-md.md) | Add a tool-agnostic `AGENTS.md` for cross-tool agent instructions |
| [05-smart-usage](exercises/05-smart-usage.md) | Hands-on practice with context, model selection, and AI credit usage |
