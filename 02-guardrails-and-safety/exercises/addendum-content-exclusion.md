# Addendum: Content Exclusion and Terminal Sandbox

Reference notes for two guardrails that this module's exercises only touch in passing: content exclusion (configured on GitHub.com) and the terminal sandbox (configured in VS Code).

## Content Exclusion

### What It Is
Content exclusion stops Copilot from using the contents of specified files: it will not read them for context or base suggestions on them. It's configured on GitHub.com at the repository, organization, or enterprise level — not inside the IDE.

> **Prerequisite:** Content exclusion is available on Copilot Business and Copilot Enterprise plans only, and it can be configured only by repository administrators (repository level), organization owners (organization level), or enterprise owners (enterprise level). Individual plans have no content exclusion settings.

### Key Limitations
- **Business/Enterprise only** — not available on individual plans.
- **Scope** — excluded content is not used for inline suggestions, does not inform Copilot Chat responses, and is skipped by Copilot code review. It is currently **not supported in Agent mode or Edit mode** of Copilot Chat in VS Code and other editors, nor in Copilot CLI.
- Changes can take up to 30 minutes to propagate to the IDE.

### How to Configure
1. Go to your repository on GitHub > **Settings > Copilot > Content exclusion**.
2. Add path patterns (e.g., `"secrets.json"`, `"*.cfg"`, `"/scripts/**"`).
3. Org-level exclusions: **Organization Settings > Copilot > Content exclusion**.

### Reference
- [Excluding content from GitHub Copilot](https://docs.github.com/en/copilot/how-tos/configure-content-exclusion/exclude-content-from-copilot)

## Terminal Sandbox

### What It Is
VS Code includes Preview settings that sandbox the agent's terminal commands at the OS level. While the command allowlist ([Exercise 2.1](01-command-allowlist.md)) controls *which* commands the agent can run, the terminal sandbox controls *what those commands can do* — restricting file system and network access even for allowed commands.

The two settings involved — `chat.tools.terminal.blockDetectedFileWrites` (Experimental) and `chat.agent.sandbox.enabled` (Preview) — are walked through, with their JSON, in [Exercise 2.1, steps 6–7](01-command-allowlist.md#steps). This addendum only covers when to reach for the sandbox and what it does not do.

> **Note:** With `"on"`, sandboxed commands have no network access by default; with `"allowNetwork"`, all outbound traffic is allowed while file system rules stay in effect. To allow specific hosts instead, enable `chat.agent.networkFilter` and list them in `chat.agent.allowedNetworkDomains` (with `chat.agent.deniedNetworkDomains` taking precedence). These domain rules govern agent tools such as fetch and the integrated browser, and — when the sandbox is `"on"` — they additionally apply to the terminal commands the agent runs. In some recent VS Code builds the network toggle appears as a separate boolean, `chat.agent.sandbox.allowNetwork`, rather than as a value of `chat.agent.sandbox.enabled`; check the settings UI if the enum value is not accepted.

### When to Use
- **High-risk projects** — when the agent works on repos with credentials, infra configs, or sensitive data outside the workspace.
- **Untrusted prompts** — when running agent tasks that involve user-supplied input or third-party prompt files.
- **Compliance environments** — where you need to demonstrate that agent actions are contained.

### Limitations
- **macOS/Linux only** — the OS-level sandbox is not available on Windows.
- **Preview** — behavior and setting names may change between VS Code releases.
- **Not a full container** — this is a process-level sandbox, not Docker-style isolation. It reduces blast radius but is not a hard security boundary.
