# Exercise 7.2: Build an Orchestration Framework

## Objective
Create a set of custom agents where one orchestrator delegates work to specialist sub-agents, each using a different model.

## Background
In Exercise 7.1 a plain prompt got you delegation, but no control over which model the sub-agent ran on or which tools it could reach. Custom agents fix that: each `.agent.md` file pins a role, a model, and a tool set. An **orchestrator** agent that owns only `read`, `search`, and `agent` tools can understand the project and hand work out, but can't implement anything itself — so the specialists (planner, coder, designer) stay in charge of *how* the work gets done. This exercise sets up that framework from ready-made asset files; you test it in Exercise 7.3.

## Prerequisites

> **Prerequisite:** Custom agents are discovered from `.github/agents/` (controlled by the `chat.agentFilesLocations` setting, on by default). GitHub's docs still document custom agents in the IDE as **public preview**, and organizations may restrict agent features via Copilot policies — check with your admin if agents don't appear in the agents dropdown. The `planner`, `coder`, and `designer` agents scope `context7/*` tools, so the **Context7 MCP server** from [Exercise 5.1](../../05-mcps-and-integrations/exercises/01-find-and-configure-mcps.md) must be configured in your project. The orchestrator also relies on the `agent/runSubagent` tool being enabled (see [Exercise 7.1](01-built-in-orchestration.md)).

## Steps

1. **Copy the agent files.** Copy these four `.agent.md` files from the `assets/` folder into `.github/agents/` in your project: `orchestrator`, `planner`, `coder`, and `designer`. Skim each one before continuing — just enough to spot the role, model, and tools in the frontmatter. Note that each agent can also be invoked individually from the agents dropdown, not just through the orchestrator.
2. **Review the orchestrator.** Open `orchestrator.agent.md`. Note that its `tools` are limited to `read`, `search`, and `agent` — it can understand the project but never implements anything itself. The key rule: it tells sub-agents *what* needs doing, never *how*. These three tools have distinct roles:
   - `read` — reads files from the workspace
   - `search` — searches the codebase for relevant context
   - `agent` — spawns and delegates work to a sub-agent

   Alongside `tools`, the frontmatter has an `agents` property: `["planner", "coder", "designer"]`. Where `tools` says *what the orchestrator can do*, `agents` says *which custom agents it may use as sub-agents* — an allowlist, so the orchestrator can't wander off and call some other agent in your repo. Its model is `Claude Sonnet 4.5`, a good fit for the coordinator role.
3. **Review the specialists.** Each sub-agent has a distinct role and model:
   - **Planner** — explores the codebase and creates implementation plans. Uses a strong reasoning model (`GPT-5.2-Codex`). Does not write code.
   - **Coder** — implements code with full tool access. Uses `Claude Sonnet 4.5`. Questions the orchestrator's assumptions and makes its own technical decisions.
   - **Designer** — handles UI/UX, styling, and other design work with full creative autonomy. Uses a model strong at visual output (`Gemini 3 Pro`).

   > **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one. Ask the agent to update the `model:` line for you, e.g. "Change the model in `.github/agents/designer.agent.md` to Gemini 3 Flash."

   Notice that all three have `disable-model-invocation: false` in their frontmatter. This controls whether **other agents** may invoke this agent as a sub-agent. With `false` (the default), the orchestrator can delegate to it via the `agent` tool — which is exactly what this framework relies on. If you set it to `true`, the agent can only be used when a person selects it from the agents dropdown; an orchestrator calling it by name would be blocked. There's a separate property, `user-invocable`, that controls whether the agent appears in the user-facing dropdown at all (set to `false` to make an agent accessible only as a sub-agent).
4. **Verify scope boundaries.** Each agent should have a clear role, a distinct model, and explicit autonomy language to counter orchestrator micromanagement.

## Expected Outcome
Four `.agent.md` files in `.github/agents/` — one orchestrator with delegation-only tools and an `agents` allowlist, and three specialists each configured with a model suited to their role.
