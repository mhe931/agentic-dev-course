# Exercise 1.4: AGENTS.md — Cross-Tool Agent Instructions

## Objective
Add an `AGENTS.md` file to your project and understand when to use it instead of (or alongside) `.github/copilot-instructions.md`.

## Background
`AGENTS.md` is an open, tool-agnostic standard for agent instructions — a single Markdown file in the repo root that many coding agents read, not just Copilot. GitHub Copilot supports it across surfaces: Copilot Chat in VS Code, the Copilot cloud agent, and the Copilot CLI (the cloud agent and CLI also recognize `CLAUDE.md` and `GEMINI.md`).

Why this matters: teams rarely standardize on a single tool. If some colleagues use Copilot, others Claude Code or Codex, maintaining separate instruction files per tool means duplication and drift. `AGENTS.md` lets you keep one shared set of agent conventions that every tool picks up.

How it relates to what you built in Exercises 1.1–1.3:

| File | Scope | Read by |
|------|-------|---------|
| `.github/copilot-instructions.md` | Repo-wide, every chat request | Copilot only |
| `.github/instructions/*.instructions.md` | Path-specific via `applyTo` globs | Copilot only |
| `AGENTS.md` | Repo-wide agent conventions | Copilot **and** other agent tools |

If your team is Copilot-only, `copilot-instructions.md` is enough. If multiple agent tools are in play (or might be), `AGENTS.md` is the better home for shared conventions — keep any Copilot-specific guidance in `copilot-instructions.md` and avoid duplicating content between the two, since VS Code combines all matching instruction files into context.

> **Prerequisite:** In VS Code, `AGENTS.md` support is controlled by the `chat.useAgentsMdFile` setting — check that it is enabled before starting. Support for nested `AGENTS.md` files in subfolders (useful in monorepos) is experimental and requires `chat.useNestedAgentsMdFiles`.

## Steps
1. In VS Code settings, search for `chat.useAgentsMdFile` and verify it is enabled.
2. In Copilot Chat (Agent mode), prompt the agent to create the file rather than writing it by hand, e.g.:

   ```
   Create an AGENTS.md file in the repo root following the agents.md convention.
   Base it on our existing .github/copilot-instructions.md, but include only
   tool-agnostic conventions: how to build and run the project, how to run
   tests and linters, coding conventions, and things agents should not do.
   Don't duplicate anything that should stay Copilot-specific.
   ```

3. Glance through the result — check that nothing in `AGENTS.md` merely repeats `copilot-instructions.md`. If there's overlap, prompt the agent to deduplicate: shared conventions live in `AGENTS.md`, Copilot-specific guidance stays in `copilot-instructions.md`.
4. Start a **new** chat session and ask something your `AGENTS.md` should influence (e.g., "how do I run the tests in this project?"). Check the context/references list in the response to confirm `AGENTS.md` was picked up.
5. **(Optional)** For monorepos: if your project has distinct subprojects, enable `chat.useNestedAgentsMdFiles` and ask the agent to add a subfolder-level `AGENTS.md` with conventions specific to that part of the codebase.

## Expected Outcome
Your repo has a single, tool-agnostic `AGENTS.md` that Copilot loads automatically — and that any other agent tool your team adopts can read too — with a clear division of responsibility between it and your Copilot-specific instruction files.
