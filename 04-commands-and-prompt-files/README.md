# Module 04: Commands and Prompt Files

This module builds on the `/spar` prompt file you used in [Exercise 3.1](../03-planning-and-sdd/exercises/01-refine-idea.md) and leads to [Module 06](../06-skills/README.md), where skills take over many of the use cases prompt files cover today.

## Learning Objectives
- Create custom prompt files (`.prompt.md`) as reusable, structured workflows
- Use frontmatter to control tools and model selection
- Build an analysis command that produces structured feedback (code review)
- Build a utility command that keeps instruction files in sync with code

## Key Concepts
A prompt file is a markdown file with a `.prompt.md` extension that defines a reusable workflow for Copilot Chat. Prompt files live in `.github/prompts/` and are invoked as slash commands in your IDE (e.g., `/structured-review`, `/update-instructions`). Each prompt file has a clear purpose, a sequence of steps, and sometimes a defined output format.

> **Note:** Prompt files are on by default in VS Code — no setting to enable. Their location is controlled by `chat.promptFilesLocations` (default `.github/prompts`). You already used one in Exercise 3.1 when you ran `/spar`.

> **Note:** With the introduction of **Skills** (covered in Module 06), prompt files are becoming less central — skills offer richer capabilities like assets and other resources. We still cover prompt files here because they're simpler to create and a good stepping stone to understanding how reusable workflows work in Copilot before moving on to skills.

Copilot already ships with built-in slash commands — `/fix`, `/tests`, `/explain`, `/setupTests` — that handle common tasks with a fixed workflow. Custom prompt files let you go further: multi-step workflows with guardrails, structured output, and domain-specific logic that the built-in commands don't cover.

Prompt files support YAML frontmatter that controls behavior beyond what the prompt body can do: `name` sets the slash command (`name: structured-review` gives you `/structured-review`; without it the file name is used), `description` explains what the prompt does, `model` pins a specific model, `agent` selects which agent runs the prompt, and `tools` lists the tools the prompt may use — the turbo review in Exercise 4.2 uses `tools` to enable sub-agents.

You don't always have to write prompt files from scratch. The `/create-prompt` command in chat scaffolds a new `.prompt.md` file interactively, and **Chat: New Prompt File** in the Command Palette (`Ctrl+Shift+P` (Windows/Linux) / `Cmd+Shift+P` (macOS)) creates an empty one in the right place. When you find yourself repeating a prompt that works well, turn it into a prompt file and refine it into a proper command.

## Relevant Documentation
- [Using GitHub Copilot Chat in your IDE](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide) — Prompt files, custom agents, and enabling sub-agents
- [Adding repository custom instructions and prompt files in your IDE](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide) — Creating and using prompt files
- [Customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet) — When to use prompt files vs. instructions, agents, and skills

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-setup-tests](exercises/01-setup-tests.md) | (Optional) Run the built-in `/setupTests` command and understand what it gives you out of the box |
| [02-code-review](exercises/02-code-review.md) | Create a code review command that produces structured, severity-grouped feedback (optional turbo variant with sub-agents) |
| [03-update-instructions](exercises/03-update-instructions.md) | Create a utility command that keeps instruction files in sync with code changes |

## Resources
The [github/awesome-copilot](https://github.com/github/awesome-copilot) collection offers community-contributed prompt files, agent definitions, and skills — and has grown into a full **plugin marketplace** in VS Code (requires the `chat.plugins.enabled` setting, which is off by default — see Module 06), where entries install as versioned agent plugins rather than files you copy by hand (see Module 06, Exercise 6.6). Browse it for inspiration, but treat entries as starting points rather than ready-to-use solutions; community contributions may be outdated or not match your project's conventions — and review what a plugin bundles before installing.
