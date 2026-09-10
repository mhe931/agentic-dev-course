# Module 02: Guardrails and Safety

This module builds on [Module 01](../01-context-and-cost/README.md), where instruction files gave the agent *soft* guidance, and adds enforced guardrails — configuration and code the agent cannot talk its way around. It leads into [Module 03](../03-planning-and-sdd/README.md), where you let the agent plan and implement larger changes inside those boundaries.

## Learning Objectives
- Control which terminal commands the agent can auto-run
- Manage agent tool permissions with custom agents
- Enforce guardrails programmatically with agent hooks (Preview)
- Understand content exclusion and the terminal sandbox, and when each applies

## Key Concepts
AI-assisted development needs guardrails. VS Code's terminal auto-approve rules (`chat.tools.terminal.autoApprove`) control which commands the agent can run without prompting — useful for allowlisting safe commands like test runners while forcing approval for destructive ones. Custom agents defined in `.agent.md` accept a `tools` property that restricts available capabilities; this module introduces the course's first custom agent, which Modules 07 and 08 build on.

Agent hooks (Preview) add a third, programmatic tier on top of these: hook configuration in `.github/hooks/*.json` pointing at scripts that run at fixed lifecycle points (`PreToolUse`, `PostToolUse`, `SessionStart`, …) and can deterministically deny tool calls, require approval, or run follow-up automation like formatters. Where instructions are soft guidance and allowlists are configuration, hooks always execute — and when mechanisms overlap, the most restrictive wins. The Copilot cloud agent and CLI use the same `.github/hooks/` location and JSON format; GitHub surfaces require `version: 1` and report tool names differently (e.g. `Bash`, `Edit`), so guard scripts need per-surface tool-name handling.

Two further terminal settings add deeper enforcement: `chat.tools.terminal.blockDetectedFileWrites` (Experimental, on by default) requires approval when an allowed terminal command writes files outside the workspace, and `chat.agent.sandbox.enabled` (Preview, macOS/Linux) runs terminal commands in an OS-level sandbox with configurable file system rules and network access that is either blocked, fully allowed, or limited to the domains in `chat.agent.allowedNetworkDomains` when `chat.agent.networkFilter` is on.

## Relevant Documentation
- [Approvals and terminal auto-approve in VS Code](https://code.visualstudio.com/docs/agents/run/approvals) — `chat.tools.terminal.autoApprove`, file-write detection, and sandboxing
- [VS Code AI settings reference](https://code.visualstudio.com/docs/agents/reference/ai-settings) — Exact setting names, types, defaults, and Preview/Experimental labels
- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents) — `.agent.md` format and the `tools` property
- [Managing Copilot policies](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies) — Organization-level policy controls
- [Agent hooks in VS Code (Preview)](https://code.visualstudio.com/docs/agent-customization/hooks) — Hook events, configuration format, and permission decisions
- [GitHub Copilot hooks reference](https://docs.github.com/en/copilot/reference/hooks-reference) — Hooks across Copilot surfaces (cloud agent, CLI)
- [Excluding content from GitHub Copilot](https://docs.github.com/en/copilot/how-tos/configure-content-exclusion/exclude-content-from-copilot) — Content exclusion (Business/Enterprise)

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-command-allowlist](exercises/01-command-allowlist.md) | Allowlist safe terminal commands with `chat.tools.terminal.autoApprove`; optional steps cover file-write detection and the terminal sandbox |
| [02-manage-agent-tools](exercises/02-manage-agent-tools.md) | Restrict agent capabilities using the `tools` property in a `.agent.md` file — the course's first custom agent (asset: `assets/read-only.agent.md`) |
| [03-agent-hooks](exercises/03-agent-hooks.md) | Enforce guardrails programmatically with `PreToolUse` and `PostToolUse` hooks (Preview) |
| [addendum-content-exclusion](exercises/addendum-content-exclusion.md) | Reference notes: org/repo-level file exclusion (Business/Enterprise only; not supported in Agent/Edit mode or Copilot CLI) and the Preview OS-level sandbox for agent terminal commands (macOS/Linux) |
