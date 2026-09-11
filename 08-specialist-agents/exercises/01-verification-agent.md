# Exercise 8.1: Build a Verification Agent

## Objective
Build a custom agent that verifies your changes by gathering context, running existing tests, and producing a structured report — without modifying any code.

## Background

After implementing a feature, how do you know it worked? A verification agent closes the loop: it gathers context about what changed, runs the existing test suite, and produces a structured report. Three patterns make it reliable:

1. **Context-gathering before acting** — The agent reads the branch diff and commit history to understand *what changed* before running anything. If a ticket exists in `tickets/`, it cross-references the acceptance criteria. This makes its observations specific to the current work.
2. **Negative constraints (guardrails)** — The agent is told what it must *not* do (no code changes, no new tests, no debugging), and its `tools` field omits `edit` to enforce this structurally. Guardrails make agents safe to run autonomously.
3. **Structured reporting** — The agent produces a consistent markdown report with a fixed schema. Predictable output is composable — it can feed into a PR comment, a CI check, or another agent's input.

## Prerequisites
- Your project has at least one existing test (unit, integration, or end-to-end) — if not, run `/setupTests` from Exercise 4.1
- You have uncommitted or recently committed changes to verify

## Steps

1. **Copy the agent file.** Copy `verifier.agent.md` from the `assets/` folder into `.github/agents/` in your project. Read through it — identify where each of the three patterns appears:
   - Context-gathering (Phase 1)
   - Guardrails (Rules section *and* the `tools` field — note that `edit` is absent)
   - Structured output (Phase 3 report template)

2. **Run it against your changes.** Select `verifier` from the agents dropdown in Chat and ask it to verify your changes. Observe:
   - Did it check the branch and diff before running tests?
   - Did it find the right test command for your project?
   - Did it stay within its guardrails (no edits, no new tests, no debugging)?

3. **Check the report.** Compare the output to the report template in Phase 3:
   - Does it have the Result, Branch, and Ticket fields?
   - Does the Test Summary match what you see when running tests manually?
   - Are the Change-Specific Observations relevant to your diff, or generic?

   > **Note:** This agent works locally against your branch diff. In a real workflow you could extend it to check for an open PR with `gh pr view` and include PR metadata in the report — making it composable with CI pipelines.

4. **(Optional) Iterate.** Ask the agent to improve one aspect of its report — e.g., add a coverage section noting which changed files have corresponding test files, or tighten the guardrail so it stops if it detects zero tests.

## Expected Outcome
A working `verifier.agent.md` in `.github/agents/` that gathers context, runs tests, and produces a structured report without modifying code. You understand the three patterns (context-gathering, guardrails, structured output) and how they make an agent safe to run autonomously.
