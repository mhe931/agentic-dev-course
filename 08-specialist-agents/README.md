# Module 08: Specialist Agents

In [Module 07](../07-agent-roles-and-orchestration/README.md) you built an orchestration framework where a coordinator delegates to sub-agents. This module shifts focus to standalone specialist agents — purpose-built agents that excel at a single job — and leads into [Module 09](../09-closing-the-loop/README.md), where you assemble them into a pipeline.

## Learning Objectives
- Build agents with guardrails and negative constraints that are safe to run autonomously
- Integrate skills (the Playwright CLI skill) into agent workflows for browser-level diagnostics
- Use `llms.txt` to ground agents in authoritative, up-to-date library documentation
- Source and structure your own documentation (compact index + full docs) for domains that don't publish an `llms.txt`

## Key Concepts

Three patterns recur across all specialist agents:

1. **Guardrails** — Negative constraints (`tools` field omissions, explicit "do not" rules) keep agents safe. A verification agent that can't edit code can be trusted to run unattended.
2. **Skill integration** — Skills like the Playwright CLI skill (`playwright-cli`) give agents capabilities beyond code editing. A debugger agent with browser inspection can diagnose UI bugs autonomously. (VS Code's built-in browser tools cover interactive page inspection natively; the skill adds network inspection and CI-reusable commands — Exercise 8.2 explains the split.)
3. **Grounded context** — Committing authoritative references (like `llms.txt`) to the repo and pointing agents at them eliminates hallucinated APIs. The agent reads current docs at task time instead of guessing from training data.

> **Prerequisite:** Custom agents are discovered from `.github/agents/` — controlled by the `chat.agentFilesLocations` setting in VS Code (on by default). GitHub documents custom agents in the IDE as a public preview, and organizations can restrict agent features through their Copilot policies — if your agents don't appear in the agents dropdown, check with your admin. The optional sub-agent step in Exercise 8.3 additionally needs both: enable `agent/runSubagent` in the Chat tools picker **and** list `'agent'` in the agent's `tools`.

> **Tip:** The `/create-agent` command in Copilot Chat can scaffold `.agent.md` files interactively. For specialist agents, it's a good starting point — but you'll typically want to refine the generated file with specific guardrails, skill references, and grounded context as covered in the exercises below.

> **Tip:** Guardrails don't have to live only in the `tools` field. Custom agents also support a `hooks` field in the `.agent.md` frontmatter (Preview, requires the `chat.useCustomAgentHooks` setting), so a specialist agent can carry its own programmatic guardrails — e.g. a verification agent whose `PreToolUse` hook denies every file edit. See [Exercise 2.3](../02-guardrails-and-safety/exercises/03-agent-hooks.md) in Module 02 for the hooks basics.

> **Note:** Also included in `assets/`: `review.agent.md` — a read-only review specialist that picks lightweight static-analysis tools (`markdownlint`, `shellcheck`, `hadolint`, `actionlint`, `semgrep`, when installed) based on the changed file types and produces a severity-ranked report. Exercise 8.5 uses it as a worked example to compare with the `/structured-review` prompt file from Exercise 4.2; you can drop it into `.github/agents/` alongside that prompt at any time.

## Relevant Documentation
- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents) — `.agent.md` format, `tools` and `agents` properties, sub-agents
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration) — Frontmatter properties and tool names across Copilot surfaces
- [Creating custom agents in your IDE](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide) — Preview status and IDE setup
- [llms.txt](https://llmstxt.org) — The standard for LLM-readable documentation files

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-verification-agent](exercises/01-verification-agent.md) | Build a read-only agent that gathers context, runs tests, and produces a structured report |
| [02-debug-agent](exercises/02-debug-agent.md) | Build a debugger agent that uses the Playwright CLI skill for autonomous bug diagnosis |
| [03-frontend-specialist-agent](exercises/03-frontend-specialist-agent.md) | Commit both `llms.txt` variants for your component library, build UI features without hallucinated APIs, and optionally delegate doc lookups to a reader sub-agent |
| [04-database-specialist-agent](exercises/04-database-specialist-agent.md) | Apply the same `llms.txt` grounding pattern to an ORM — build a database agent that generates correct schema changes and migrations |
| [05-build-your-own-specialist](exercises/05-build-your-own-specialist.md) | Design your own specialist agent grounded in documentation the agent fetches, curates, and commits for you |
