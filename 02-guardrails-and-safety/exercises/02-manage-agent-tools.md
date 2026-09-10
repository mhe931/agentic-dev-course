# Exercise 2.2: Manage Agent Tools

## Objective
Restrict an agent's available capabilities using the `tools` property in an `.agent.md` file.

## Background
Custom agents are Markdown files with an `.agent.md` extension: the YAML frontmatter holds configuration, and the body holds the persona and instructions. The `tools` property in the frontmatter lists the tool sets the agent may use — `read`, `search`, `edit`, `execute`, `web`, `agent`, and so on. Anything not listed is simply unavailable to the model, however it is prompted. That makes `tools` a *configured* guardrail: an instruction such as "don't edit files" is soft guidance the model can misinterpret or ignore, but a missing `edit` tool cannot be worked around.

This is the first `.agent.md` file in the course. [Module 07](../../07-agent-roles-and-orchestration/README.md) and [Module 08](../../08-specialist-agents/README.md) build on the same file format to define full agent roles (planner, coder, verifier, …). Here you only care about the `tools` list.

> **Prerequisite:** Custom agents are discovered from `.github/agents/` in your workspace — controlled by the `chat.agentFilesLocations` setting, which is on by default. GitHub documents custom agents in the IDE as a public preview, and organizations can restrict agent features through Copilot policies. If your agent does not appear in the agents dropdown, check your VS Code version and org policy first.

> **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one.

## Steps
1. Copy `read-only.agent.md` from the [`assets/`](../assets/read-only.agent.md) folder into `.github/agents/` in your project.
2. Glance at the frontmatter. `name` and `description` are what you see in the agents dropdown. `tools: ['read', 'search']` is the guardrail: the agent can read files and search the codebase but has no `edit`, `execute`, or `web` tools. `model` pins it to a fast, inexpensive model, which is all a read-only explainer needs. The body below the frontmatter is the persona — note that it *also* tells the agent not to edit, but the tool list is what actually enforces it.
3. Open Copilot Chat, open the agents dropdown below the chat input, and select **read-only**. Ask it something it *can* do, e.g. "Explain how this project is structured and where the entry point is." It should answer from the code.
4. Now ask it to make a change, e.g. "Add a comment at the top of the main file explaining what it does." Confirm it cannot: the `edit` tool is not available, so the agent should explain what it would change instead of changing it. Open the tools picker in the chat input to see that only read and search tools are listed.
5. Create a variant without writing frontmatter by hand. Type `/create-agent` in Copilot Chat (or run **Chat: New Custom Agent** from the Command Palette) and describe it:

   ```
   Create a "test-runner" agent that can read and search the codebase and run
   terminal commands to execute the test suite, but must never edit files.
   ```

   Glance at the generated `tools` list — it should include `execute` but not `edit` — then select the new agent and ask it to run the tests.

## Expected Outcome
You have a working read-only custom agent in `.github/agents/`, you can explain what each frontmatter property does, and you have verified that a tool missing from the `tools` list is enforced at runtime rather than merely requested. You also know how to generate new agent variants with `/create-agent` instead of hand-editing frontmatter.

> **Tip:** The `tools` list is only one of the frontmatter properties. Modules 07 and 08 introduce `agents`, `handoffs`, and per-agent `hooks` on top of it.
