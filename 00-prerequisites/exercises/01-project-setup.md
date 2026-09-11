# Exercise 0.1: Project Setup

## Objective

Scaffold the Micro Journal app you will use throughout this course.

## Background

Every later module adds something to a real codebase — instruction files, prompt files, custom agents, MCP servers — and then asks you to run and test the result. That codebase is the **Micro Journal** app: quick daily entries with mood tracking, built from the brief in [`../assets/micro-journal.md`](../assets/micro-journal.md).

Everyone on the course builds the same app on the same stack — a **React + TypeScript** frontend, a **Node.js** backend, and SQLite for storage. That keeps the file paths, example prompts, and expected outcomes in later modules aligned with what's actually in your project, and lets you compare results with other participants.

Keep the app in its own VS Code workspace, separate from this course repo. Copilot picks up customizations from the workspace root, so the files you create in later modules should live in your app, not next to the course material.

## Steps

1. Create a new empty folder outside this course repo and open it in its own VS Code window.

2. Initialize Git: `git init`

3. Open [`../assets/micro-journal.md`](../assets/micro-journal.md) and copy the prompt from the **Prompt** section. The tech stack is already filled in — leave it as is.

4. Open Copilot Chat and select **Agent** in the agents dropdown (mode picker). Paste the prompt.

   > **Note:** In Ask mode Copilot only answers in the chat and will not create files. If **Agent** is missing from the picker, see the prerequisite callout in the [module README](../README.md#what-youll-need).

5. Let Copilot generate the app structure, approving the file and terminal actions it asks for.

6. Follow any setup instructions Copilot provides (install dependencies, start the dev script) and confirm the app runs locally without errors — frontend and backend both up, and you can save an entry. If something fails, paste the error back into the chat and ask Copilot to fix it.

7. Commit the initial scaffold.

## Expected Outcome

- The Micro Journal app runs locally without errors, with the React + TypeScript frontend talking to the Node.js API
- A local Git repository is initialized, with the initial scaffold committed
- The app is open in its own VS Code workspace, ready for the Copilot exercises in subsequent modules
