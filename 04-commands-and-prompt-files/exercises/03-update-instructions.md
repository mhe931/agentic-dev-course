# Exercise 4.3: Update Instructions Command

## Objective
Create a utility command that checks recent code changes and updates Copilot instruction files to match.

## Background
As a codebase evolves, the instruction files that guide Copilot — custom instructions, prompt files, agent definitions — can drift out of sync with reality. A renamed directory, a changed convention, or a new module that existing instructions don't mention all reduce the quality of Copilot's assistance.

This command takes a simple approach: look at what changed in recent commits, read the instruction files, and update anything that's out of date.

It's also a pattern you can extend beyond instruction files. The same approach — diff recent changes, read docs, update what's stale — works for READMEs, architecture decision records, onboarding guides, or any documentation that should track the code. Once the basic command works, adapting it to other doc types is just a matter of changing which files it reads and updates.

## Prerequisites
- Your project has a `.github/copilot-instructions.md` from [Exercise 1.1](../../01-context-and-cost/exercises/01-create-copilot-instructions.md). Ideally you also have path-specific instructions from [Exercise 1.3](../../01-context-and-cost/exercises/03-path-specific-instructions.md) and an `AGENTS.md` from [Exercise 1.4](../../01-context-and-cost/exercises/04-agents-md.md) — the more instruction files exist, the more the command has to check.
- A few commits of history in your project (the command diffs the last 5 commits by default).

## Steps

1. **Create the prompt file.** Copy `update-instructions.prompt.md` from the `assets/` folder into `.github/prompts/` in your project. Skim it before continuing.

2. **Inspect the frontmatter.** Notice the `model` field is pinned to Claude Haiku 4.5 — a fast, cheap model well-suited for this kind of mechanical cross-referencing. The work is straightforward (diff → read → update), so a premium model would burn requests for little extra quality.

   > **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one.

3. **Test it.** Run `/update-instructions` in Copilot Chat. It will check the last 5 commits by default. Evaluate:
   - Did it find all your instruction files (`copilot-instructions.md`, `.github/instructions/`, `AGENTS.md`, prompts, agents)?
   - Did it correctly identify what needed updating?
   - Were the updates accurate and minimal — changing only what drifted?

4. **Introduce intentional drift.** Ask the agent to make a change that creates obvious drift, for example:

   > Rename the `src/utils` directory to `src/helpers` and update all imports.

   Pick a directory or convention that your instruction files actually mention. Then run `/update-instructions` again and check whether it catches the change and fixes the instruction file. (Uncommitted changes are included — `git diff HEAD~5` compares against your working tree.)

5. **Think about when to run it.** This command is most useful as a post-implementation step — after finishing a feature or refactor, run it to make sure your instruction files still match the code. You could also invoke it before a PR or commit to catch drift early, or extend it to cover other documentation like READMEs.

## Expected Outcome
A reusable `update-instructions.prompt.md` command that keeps Copilot instruction files in sync with code changes. You understand how the same diff-read-update pattern can be extended to maintain any documentation that should track the codebase.
