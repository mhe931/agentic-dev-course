# Exercise 1.2: Personal Instructions

## Objective
Understand where user-level instructions live and what belongs in them versus repo-level instructions.

## Background
Personal instructions apply to **you**, across every project you open. In VS Code they live as `.instructions.md` files in your **user profile** (not the workspace), so they follow your VS Code profile — and if you use Settings Sync with **Prompts and Instructions** enabled, they travel with you to other machines. Because they are not tied to a repository, they should only contain preferences about your personal development environment and communication style, never project-specific conventions.

Good examples:
- `Always respond in Swedish.`
- `Prefer concise answers with code examples over long explanations.`
- `Never start your answers with "You're absolutely right."`

Bad examples (these belong in repo or org instructions):
- `Use Zod for input validation.` (project-specific)
- `Run tests with vitest.` (repo-specific tooling)

A user instruction file needs `applyTo: "**"` in its YAML frontmatter to apply to every conversation automatically. Without it, the file exists but won't activate on its own — you'd have to attach it manually each time.

> **Note:** GitHub.com also offers *personal custom instructions* (github.com/settings/copilot). That is a separate feature that applies only to Copilot Chat on GitHub.com — it does not affect VS Code. This exercise covers the VS Code user instruction file.

> **Note:** You may also see the `github.copilot.chat.codeGeneration.instructions` setting in `settings.json`. Settings-based instructions are deprecated since VS Code 1.102 — use instruction files instead.

## Steps
1. Open the Command Palette (`Ctrl+Shift+P` (Windows/Linux) / `Cmd+Shift+P` (macOS)), run **Chat: New Instructions File**, and select **New Instructions (User)** as the location (not Workspace). Name the file (e.g. `personal`). You can also get here from the **Configure Chat** gear icon in the Chat view.
2. With the new file open, ask Copilot to draft its content rather than writing it by hand, e.g.: *"Fill in this user instruction file: add `applyTo: "**"` to the frontmatter and one or two short personal preferences — prefer concise answers with code examples, and never start an answer with 'You're absolutely right.'"* Apply the result to the open file.
3. Start a new Copilot Chat conversation and verify it respects your preferences.

## Expected Outcome
Copilot tailors responses to your personal preferences without you repeating them in every prompt — and those preferences travel with you to any repository you open in VS Code.
