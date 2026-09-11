# Exercise 2.1: Configure the Terminal Command Allowlist

## Objective
Control which terminal commands the Copilot agent can run without asking for approval.

## Background
By default, Agent mode asks for confirmation before running a terminal command — except for a built-in set of safe commands that VS Code auto-approves and a built-in set of risky ones (such as `rm` and `del`) that always require approval. The `chat.tools.terminal.autoApprove` setting lets you extend this. It is an object that maps a command (or a `/regex/` pattern) to `true` (auto-approve) or `false` (always require approval). For a command line to be auto-approved, every subcommand in it must match a `true` rule and none may match a `false` rule.

Two related settings complete the picture: `chat.tools.terminal.enableAutoApprove` (default `true`) is the master toggle — set it to `false` and every command prompts, regardless of rules. `chat.tools.terminal.ignoreDefaultAutoApproveRules` (default `false`) drops VS Code's built-in allow and deny rules so that only yours apply.

All of these can be set at user level or at workspace level in `.vscode/settings.json`. Committing the workspace file lets a team share one policy with the repo.

## Steps
1. Ask Copilot how to do it: `@vscode how do I control which terminal commands the agent can run without asking?` The answer should point you to `chat.tools.terminal.autoApprove` and mention that you can configure it in user or workspace settings.
2. Open your settings JSON (Command Palette: `Ctrl+Shift+P` (Windows/Linux) / `Cmd+Shift+P` (macOS) → **Preferences: Open User Settings (JSON)**) and add rules — safe commands set to `true`, dangerous ones to `false`:
   ```json
   "chat.tools.terminal.autoApprove": {
     "npm test": true,
     "git status": true,
     "/^npm run (lint|test)/": true,
     "rm": false
   }
   ```
3. Test it: switch to Agent mode, ask Copilot to run an allowed command (e.g. `npm test`) and confirm it executes without a prompt. Then ask it to run something that matches a `false` rule (e.g. remove a scratch file with `rm`) and confirm you are asked to approve.
4. Add the same setting to `.vscode/settings.json` (workspace level) so the policy travels with the repo, and confirm your rules still apply from there.
5. Keep in mind that when the agent asks for terminal permission, the approval dropdown lets you allow the command once, for the session, for the workspace, or always. Approving for the workspace or always is a natural way to build out your allowlist over time.

### Optional: Additional Terminal Guardrails

Beyond the allowlist, VS Code offers two deeper terminal guardrails. Both live in Settings under **Chat › Tools › Terminal** and **Chat › Agent › Sandbox**, or in `settings.json`.

6. **(Optional)** Look at `chat.tools.terminal.blockDetectedFileWrites` (Experimental). It is a string setting whose default, `"outsideWorkspace"`, is already active: when a terminal command is detected to write files outside your workspace (e.g. `echo "..." > ~/file.txt`, `curl ... -o ~/Downloads/file`), VS Code requires your approval even if the command itself is auto-approved. It does not block the write, and writes inside the workspace are unaffected.
   ```json
   "chat.tools.terminal.blockDetectedFileWrites": "outsideWorkspace"
   ```
   Ask the agent to run an allowed command that writes a file outside the workspace and watch the approval prompt appear.
7. **(Optional)** (macOS/Linux only) Turn on the terminal sandbox with `chat.agent.sandbox.enabled` (Preview). It is a string, not a boolean: `"off"` (default), `"on"` (file system and network isolation), or `"allowNetwork"` (file system isolation only; all outbound network traffic is allowed). Because the sandbox enforces the boundary at the OS level, commands that run inside it are auto-approved without a prompt. File system rules are configured per platform:
   ```json
   "chat.agent.sandbox.enabled": "on",
   "chat.agent.sandbox.fileSystem.mac": {
     "allowWrite": ["/tmp"],
     "denyRead": ["~/.ssh"]
   }
   ```
   On Linux use `chat.agent.sandbox.fileSystem.linux`; both accept `allowRead`, `allowWrite`, `denyRead`, and `denyWrite` path lists. Ask the agent to run a harmless command, then to write a file outside the allowed paths, and compare what happens.
8. **(Optional)** For the strongest isolation, run the project in a [dev container](https://code.visualstudio.com/docs/devcontainers/containers) — then everything the agent does in the terminal happens inside a container rather than on your machine.

## Expected Outcome
Commands matching a `true` rule run automatically; commands matching a `false` rule always ask for approval; anything not covered by your rules or VS Code's built-in defaults prompts as before. You know where the rules live (user vs. workspace `settings.json`) and what the master toggle does. If you did the optional steps, you have also seen how the file-write check and the sandbox add OS-level enforcement on top of pattern matching.
