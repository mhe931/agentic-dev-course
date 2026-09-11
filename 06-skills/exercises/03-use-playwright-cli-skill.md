# Exercise 6.3: Use the Playwright CLI Skill

## Objective
Prompt the agent to perform browser automation tasks using the Playwright CLI skill.

## Background
The Playwright CLI skill (`playwright-cli`) teaches the agent a new capability: controlling a real browser via `playwright-cli` commands. When the skill is installed, explicitly tell the agent to use it in your prompt — the agent does not always load skills automatically. Once loaded, the agent uses the skill's command vocabulary to drive the browser. You never run the commands yourself — you describe what you want and the agent drives the browser. If you glance at the skill's frontmatter, note that the `allowed-tools` line is Claude Code syntax and is ignored by VS Code.

> **Note:** The agent also has built-in browser tools. VS Code ships native browser tools for agents (enabled by default via `workbench.browser.enableChatTools`): the agent can open pages in the integrated browser, read content, click, type, take screenshots, and check console errors without any skill installed. So why still learn this skill? The CLI approach adds what the built-in tools don't cover — network request inspection, browser choice (Chrome/Firefox/WebKit), persistent profiles for authenticated sessions, and commands that work identically outside the IDE (scripts, CI). It's also this module's example of a *tool skill*: the same pattern applies to any CLI you want to teach the agent. If the agent reaches for its built-in browser tools during this exercise, remind it to use the Playwright CLI skill.

## Prerequisites
1. Node.js installed (the Playwright CLI is distributed as an npm package).
2. Playwright CLI installed: https://github.com/microsoft/playwright-cli
3. **Start your dev server**: Run `npm run dev` (or equivalent) so your app is accessible at localhost before beginning.

## Steps
1. **Install the skill.** Two options:
   - **From the CLI (recommended):** run `playwright-cli install --skills` in your project's terminal. The CLI installs its own agent skill locally, and it stays current with the tool. Glance at what the command created and where.
   - **From the course material:** copy the `playwright-cli/` folder from the `assets/` folder into your project's `.github/skills/playwright-cli/`. Works offline, and having the folder in front of you makes the skill's structure easy to read.

2. **Open and describe**: Tell the agent to use the Playwright CLI skill to open your app in a visible browser. For example:

   > *"Use the playwright-cli skill to open my app at http://localhost:5173 with a visible browser and tell me what's on the page."*

   Watch the agent load the skill, run `playwright-cli open --headed`, take a snapshot, and interpret the page structure for you.

3. **Perform an interaction**: Ask the agent to interact with the UI. For example:

   > *"Fill in the email field with test@example.com and submit the form."*

   Observe how the agent reads element references from the snapshot, picks the right `fill` and `click` commands, and takes a follow-up snapshot to confirm the result.

4. **Debug something**: Ask the agent to inspect diagnostics. For example:

   > *"Check the browser console for errors"* or *"Show me what network requests the page made."*

   The agent should use `console` or `network` commands from the skill without you specifying them.

5. **Reflect on the experience**: The agent chose which `playwright-cli` commands to run, interpreted snapshots, and adapted to what it found on the page. Compare this to prompting without a skill — what does having a structured command vocabulary in the skill body add? Then try one of the same tasks in a fresh chat *without* mentioning the skill and let the agent use its built-in browser tools — which approach would you pick for interactive poking around, and which for repeatable checks you'd also run in CI?

## Expected Outcome
The agent autonomously drives a browser session using the Playwright CLI skill — opening pages, interacting with elements, and reporting results — all from natural-language prompts.

> **Tip:** You can steer how the agent launches the browser by mentioning it in your prompt: ask for a *visible* browser (the agent uses `--headed` so you can watch), a specific browser (`--browser=chrome|firefox|webkit`), or a *persistent profile* (`--persistent`) to keep cookies and storage across sessions — handy for authenticated flows.
