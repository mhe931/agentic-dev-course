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
1. In your project, open Copilot Chat and type `/init` in the chat input, followed by an explicit target for this exercise:

   ```
   /init create .github/copilot-instructions.md for this project
   ```

   `/init` takes an optional argument — a specific request for a customization file, or for a new project a description of it — and focuses on that. The argument matters here: **left to its own devices, `/init` prefers `AGENTS.md` when neither instruction file exists yet.** You'll create that file deliberately in [Exercise 1.4](04-agents-md.md), so for now steer it to the Copilot-specific file. If one of the two files already exists, `/init` updates that one rather than adding a second.

   Watch what it does. It discovers AI conventions already present in the workspace, inventories existing documentation (`docs/**`, `CONTRIBUTING.md`, `ARCHITECTURE.md`), analyzes your project structure and coding patterns, and then writes the instruction file. Note the "link, don't embed" principle it follows: rather than copying existing docs into the instruction file, it links to them.

   > **Note:** `/init` is broader than instructions — it can also create skills and custom agents. You'll meet those in Modules 06 and 08. Scoping it with an argument keeps this exercise focused.

   Two other routes exist: **Generate Instructions** from the dropdown in the Agent Customizations editor (the gear icon in the Chat view, or **Chat: Open Customizations** from the Command Palette), and `/create-instructions` followed by a description. `/create-instructions` is aimed at targeted, on-demand `.instructions.md` files — the kind you'll create in [Exercise 1.3](03-path-specific-instructions.md) — so `/init` remains the one for repo-wide always-on instructions.
2. Skim the generated instructions. It's quite common that the default prompt adds things that are a bit too verbose or not relevant to every agent turn. If you spot such content, ask Copilot to trim it, e.g.: *"Trim `.github/copilot-instructions.md` down to repo-wide conventions that aren't obvious from the code. Replace long explanations with short references to the relevant docs or files in the repo."*
3. Start a new chat and ask Copilot to make a small change in your project. Glance at the result to confirm it follows the conventions in your instruction file.
4. Confirm the file is actually being loaded: right-click in the Chat view and select **Diagnostics** to see every instruction file in context and any errors. This is the fastest way to tell "the instructions were ignored" apart from "the instructions were never loaded" — a distinction worth knowing before the later exercises stack more instruction files on top.

## Expected Outcome
- `.github/copilot-instructions.md` exists and is trimmed down to repo-wide conventions
- The Diagnostics view confirms the file is loaded into chat context
- Copilot generates code that follows the conventions you specified in your instruction file
