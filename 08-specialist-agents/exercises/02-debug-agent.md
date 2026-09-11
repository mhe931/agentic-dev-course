# Exercise 8.2: Debug Agent with Playwright CLI Skill

## Objective
Create a standalone debugger agent that can run commands and tests, read logs, and inspect the browser via the Playwright CLI skill (`playwright-cli`) — then test it on a bug so you can see it gather diagnostic information autonomously.

## Background
Copilot's built-in `/fix` command proposes a fix for selected code in one pass, without investigating first. Real debugging is rarely one step: you need to reproduce the issue, check terminal output and browser console logs, trace the root cause, apply a targeted fix, and verify it worked. A custom agent encodes this discipline as a persona you can select from the agents dropdown, hand off to, or call as a sub-agent — and, like a prompt file, lets you scope which tools are available. The Playwright CLI skill exposes browser automation commands (`playwright-cli console`, `playwright-cli network`, `playwright-cli snapshot`, etc.) that let the agent inspect what's happening in the browser without you having to manually copy-paste errors. You just describe the bug; the agent gathers the evidence.

> **Note:** The agent also has VS Code's built-in browser tools (open pages, click, screenshot, read console errors) and may reach for those instead — that's fine for reproducing the bug visually. The skill still earns its place in a debugger agent: `playwright-cli network` covers failed API calls, which the built-in tools don't inspect, and the same commands run outside the IDE if you later move this agent's checks into CI. If the agent skips network inspection during OBSERVE, point it back to the skill.

## Prerequisites
- The Playwright CLI skill is installed in your project — via `playwright-cli install --skills` or copied to `.github/skills/playwright-cli/` from assets, completed in [Exercise 6.3](../../06-skills/exercises/03-use-playwright-cli-skill.md).

## Steps

1. **Copy the agent file.** Copy `debugger.agent.md` from the `assets/` folder into `.github/agents/` in your project. Skim it — note the four-phase flow (observe → diagnose → fix → validate) and the `playwright-cli` command references.

2. **Review the Playwright CLI tools.** The agent's `tools` field is omitted, meaning it gets access to all available tools — and can load the Playwright CLI skill. The agent prompt references these commands for browser diagnostics:

   | Command | What it gathers |
   |---------|----------------|
   | `playwright-cli goto [url]` | Opens the page where the bug occurs |
   | `playwright-cli console` | JS console errors, warnings, and logs |
   | `playwright-cli network` | Failed API calls, status codes, missing responses |
   | `playwright-cli snapshot` | Accessibility tree showing the current page state |
   | `playwright-cli click [ref]` / `playwright-cli type [text]` | Reproduces user interactions that trigger the bug |
   | `playwright-cli screenshot` | Visual snapshot of the failure |

   Combined with terminal access (running commands, reading logs, running the test suite) and file tools (reading and editing source code), the agent has everything it needs to debug autonomously.

3. **Plant a bug.** In a fresh chat (with the default agent, not the `debugger` agent), ask Copilot to introduce a subtle bug in a named area of your project — and to keep the details to itself so you can't accidentally lead the debugger agent:

   > Introduce a subtle bug in the checkout form submission handler so that submitting the form silently does nothing. Don't add comments and don't tell me what you changed.

   Other good targets: an API endpoint that returns the wrong data, or a typo in a route path.

4. **Test the debugger agent on the bug.** Start a new chat, select **debugger** from the agents dropdown, and describe the symptom briefly — then let the agent work:

   > The checkout form is broken — submitting it does nothing.

   Watch how it proceeds through the phases:
   - **OBSERVE**: Does it run the test suite? Does it use `playwright-cli goto` to open the page, `playwright-cli console` to check for JS errors, and `playwright-cli network` to inspect API calls?
   - **DIAGNOSE**: Does it cross-reference terminal output with browser findings to pinpoint the root cause?
   - **FIX**: Is the fix minimal and targeted — just the broken code, no surrounding cleanup?
   - **VALIDATE**: Does it re-run the tests and re-check the browser to confirm the fix?

5. **Iterate on the prompt.** If the agent skips evidence-gathering and jumps to a fix, ask it to tighten the OBSERVE instructions in `debugger.agent.md`. If it over-fixes, have it reinforce the "only fix the reported issue" rule. If it doesn't run the test suite, ask it to make the test-running instruction more prominent.

## Expected Outcome
A `debugger.agent.md` in `.github/agents/` that uses the Playwright CLI skill for browser inspection. When you select the debugger agent and describe a bug, it autonomously gathers terminal output, runs the test suite, inspects browser console and network logs, diagnoses the root cause, applies a minimal fix, and validates it — without you needing to manually copy errors or guide each step.
