# Module 07: Agent Roles and Orchestration

This module builds on the first `.agent.md` you wrote in [Exercise 2.2](../02-guardrails-and-safety/exercises/02-manage-agent-tools.md) and the reusable prompt files from [Module 04](../04-commands-and-prompt-files/README.md), and leads into the specialist agents of [Module 08](../08-specialist-agents/README.md).

## Learning Objectives
- Understand the orchestrator pattern: one agent delegating to specialist sub-agents
- Build a multi-agent framework using custom agents with different models
- Use sub-agent isolation to preserve context window budget
- Run parallel agent sessions with git worktree isolation, and choose between delegation and parallelism

## Key Concepts

Orchestration takes custom agents further: a single coordinating agent breaks down work and delegates to specialist sub-agents — a planner, a coder, a designer — each running a model suited to its role. The orchestrator never implements anything itself; it coordinates. Sub-agents run in isolated context windows, so the main session stays lean even on large tasks. The key challenge is preventing the orchestrator from micromanaging — sub-agents need autonomy to do their best work. This pattern works in both VS Code (custom agents calling sub-agents via the `agent` tool) and the Copilot CLI (prompting one model to delegate to others).

> **Prerequisite:** Custom agents are discovered from `.github/agents/` in your workspace (controlled by the `chat.agentFilesLocations` setting, on by default). GitHub's docs still document custom agents in the IDE as **public preview**, and organizations may restrict agent features via Copilot policies — check with your admin if agents don't appear in the dropdown. Sub-agent delegation additionally requires the `agent/runSubagent` tool to be enabled in the Chat tools picker (or listed in the agent's `tools`).

Delegation within a session isn't the only way to multiply agents. VS Code's **Agent Sessions view** (VS Code 1.109+) runs multiple sessions in parallel — each with its own context and model, optionally in an isolated **git worktree** so concurrent agents never conflict on files. The Agent Sessions view also surfaces each session's sub-agents (model, elapsed time, tool calls) and lets you review diffs alongside the chat. Use the orchestrator when steps depend on each other; use parallel sessions when tasks are independent — and combine them freely. Sessions can also sync to your GitHub account (`chat.sessionSync.enabled`), making your agent conversation history searchable across machines.

> **Tip:** You can scaffold agent files directly from Copilot Chat using the `/create-agent` command. It walks you through creating a properly structured `.agent.md` file interactively — useful for bootstrapping new agents before asking the agent to refine them.

## Relevant Documentation
- [Multi-agent development in VS Code](https://code.visualstudio.com/blogs/2026/02/05/multi-agent-development) — Agent Sessions view, sub-agents, local vs. cloud agents
- [VS Code AI settings reference](https://code.visualstudio.com/docs/agents/reference/ai-settings) — Agent Sessions view and agent-related settings

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-built-in-orchestration](exercises/01-built-in-orchestration.md) | Spawn a sub-agent for Context7 doc lookup with a single prompt — built-in orchestration without custom agent files |
| [02-build-orchestration-framework](exercises/02-build-orchestration-framework.md) | Create an orchestrator + specialist sub-agents, each on a distinct model |
| [03-run-and-iterate](exercises/03-run-and-iterate.md) | Refine a new feature idea, test the framework on it, and optionally ask the agent to refine the prompts |
| [04-parallel-sessions-and-worktrees](exercises/04-parallel-sessions-and-worktrees.md) | Run two concurrent sessions in the Agent Sessions view — one in an isolated git worktree, at least one driven by your orchestrator — and learn when parallelism beats delegation |
