# Module 09: Closing the Loop

This module builds on everything from Modules 01–08 — it is the capstone where you assemble the individual building blocks into an agent pipeline that fits your team's real workflow.

## Learning Objectives

- Map your team's real development workflow onto agent patterns learned in Modules 01–08
- Identify which pipeline stages benefit most from agent automation
- Define guardrails and human checkpoints for each stage
- Produce a prioritized pipeline blueprint to take back to your team

## Key Concepts

### From Patterns to Pipeline

Throughout this course you've learned individual building blocks:

| Module | Building Block |
|--------|---------------|
| 01 | Custom instructions & context management |
| 02 | Guardrails & safety controls |
| 03 | Spec-driven planning |
| 04 | Reusable prompt files & commands |
| 05 | MCP servers & integrations |
| 06 | Skills |
| 07 | Agent Roles and Orchestration |
| 08 | Specialist agents with grounded context |

This module is about **assembling those blocks into a pipeline that fits your workflow** — from backlog to production.

### Pipeline Stages

A typical agent-assisted pipeline covers:

1. **Backlog & Requirements** — Ticket triage, refinement, enrichment
2. **Planning & Specification** — Technical plans, architecture decisions
3. **Implementation** — Code generation with specialist agents
4. **Testing & Verification** — Automated test execution and validation
5. **Review & Quality** — First-pass automated review before human eyes
6. **Deployment & Operations** — CI/CD, environment setup, monitoring, incident triage

Not every team needs every stage automated. The exercise helps you figure out **where agents add the most value for your specific context**.

### Guiding Principles

- **Start small** — Automate 1-2 high-value stages first, then expand
- **Human-in-the-loop** — Keep humans at critical decision points (architecture, deployment, security)
- **Guardrails always** — Every agent needs clear boundaries on what it can and cannot do. For boundaries that must hold without exception, prefer programmatic enforcement with hooks ([Exercise 2.3](../02-guardrails-and-safety/exercises/03-agent-hooks.md)) — the IDE, the Copilot cloud agent, and the CLI share the same `.github/hooks/` location and JSON format (GitHub surfaces require `version: 1` and report tool names differently, e.g. `Bash`, `Edit`, so guard scripts need per-surface tool-name handling), turning your guardrails into team-wide quality gates
- **Standardize the setup** — A pipeline only works if everyone runs the same one. Commit shared config to the repo (instructions, `.vscode/mcp.json`, agents, hooks), package reusable capabilities as agent plugins (requires `chat.plugins.enabled`, see [Module 06](../06-skills/README.md)), and for larger rollouts distribute Copilot configuration centrally via enterprise managed settings
- **Iterate** — Your pipeline will evolve as you learn what works and what doesn't

> **Prerequisite:** Enterprise managed settings are available on enterprise plans only and are configured by Enterprise owners. Settings can be deployed server-managed (default; also applies to the Copilot cloud agent), MDM-managed, or file-based. See [Configuring enterprise managed settings](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/configure-enterprise-managed-settings).

## Exercises

| Exercise | Description |
|----------|-------------|
| [01-design-your-pipeline](exercises/01-design-your-pipeline.md) | Map your workflow to agent patterns, fill in a per-stage blueprint, and (optionally) have the agent draft `.agent.md`, prompt-file, and hook designs for your top two stages |

## Relevant Documentation

- [About hooks for Copilot agents](https://docs.github.com/en/copilot/concepts/agents/hooks) — the `.github/hooks/` mechanism for the Copilot cloud agent and the CLI
- [Hooks in VS Code](https://code.visualstudio.com/docs/agent-customization/hooks) — configuring and creating hooks in VS Code (Preview)
- [About agent plugins](https://docs.github.com/en/copilot/concepts/agents/about-plugins) — packaging agents, skills, prompts, and MCP servers for reuse
- [Configuring enterprise managed settings](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/configure-enterprise-managed-settings) — centrally distributing Copilot configuration across an enterprise
