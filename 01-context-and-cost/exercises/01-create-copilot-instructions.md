# Exercise 1.1: Create Copilot Instructions

## Objective
Add a repo-level instruction file so Copilot follows your project's standards.

## Background
`.github/copilot-instructions.md` is the repo-wide instruction file: Copilot adds it to every chat request in the workspace automatically. That makes it the right place for things that affect the entire repo and are not already obvious from the code itself — and the wrong place for anything verbose or only occasionally relevant. Good candidates:

- Team conventions
- Coding style and conventions
- Testing — approach, libraries, how to run tests, linters, formatters
- Logging and monitoring conventions
- Soft guardrails — what the agent should/shouldn't do, common mistakes to avoid

Since this file gets added to every chat session, keep it as neat and minimal as possible. Especially in a large repo, it should ideally contain only references and perhaps an index to other documentation within the repo, to avoid duplication and maintenance overhead. Keep in mind that as you make changes or add new features to your codebase, this file might need updates as well — Copilot usually does not do that automatically unless instructed to.

## Steps
1. In your project, open Copilot Chat in Agent mode and type `/init`. The agent analyzes your codebase and generates `.github/copilot-instructions.md` — watch which files it reads to build the instruction file. (Alternatively, open **Configure Chat** (the gear icon in the Chat view) → **Agent Customizations** → **Generate Instructions** for the same result from the UI.)
2. Skim the generated instructions. It's quite common that the default prompt adds things that are a bit too verbose or not relevant to every agent turn. If you spot such content, ask Copilot to trim it, e.g.: *"Trim `.github/copilot-instructions.md` down to repo-wide conventions that aren't obvious from the code. Replace long explanations with short references to the relevant docs or files in the repo."*
3. Start a new chat and ask Copilot to make a small change in your project. Glance at the result to confirm it follows the conventions in your instruction file.

## Expected Outcome
Copilot generates code that follows the conventions you specified in your instruction file.
