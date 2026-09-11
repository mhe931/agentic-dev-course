# Exercise 7.3: Run and Iterate

## Objective
Test your orchestration framework on a real task, observe how the agents coordinate, and refine the prompts based on what you see.

## Background
An orchestration framework only proves itself on a real ticket. In this exercise you hand the orchestrator a refined feature spec and watch the delegation flow — which specialists it calls, in what order, and how much context it passes along. Two things usually need tuning after a first run: the orchestrator summarizing instead of forwarding the full plan, and the orchestrator micromanaging specialists with implementation details. Because the agents are just `.agent.md` files, you fix both by asking Copilot to revise the prompts, not by editing them by hand.

## Steps
1. **Pick a ticket.** Use the `/spar` prompt file from [Module 03](../../03-planning-and-sdd/README.md) to refine a new feature idea into a ticket (or reuse one from `tickets/`). If possible, pick something that touches multiple concerns (planning, code, and UI) so the orchestrator has meaningful work to delegate. If the spec includes acceptance criteria or test scenarios, even better — you'll use those to verify the result. Keep the ticket in `tickets/` as you did before.
2. **Run the orchestrator.** Open Copilot Chat and choose `orchestrator` from the agents dropdown. Give it the ticket from step 1 — paste or reference the ticket file so the orchestrator has the full spec, not just a one-liner.
3. **Watch the delegation.** Follow the chat history as it delegates. Note: which sub-agents did it call? In what order? Did it pass enough context to each one?
4. **Check the context budget.** Glance at the context window indicator after the run completes. Sub-agents use isolated context, so the main session should still have plenty of headroom.
5. **(Optional) Refine and re-run.** If you spot issues, ask the agent to revise the `.agent.md` files rather than editing them yourself. Common fixes, with example prompts:
   - The orchestrator passed a high-level summary instead of the full plan:

     > Update `.github/agents/planner.agent.md` so the planner saves each plan to `plans/<ticket>.md`, and update `.github/agents/orchestrator.agent.md` so it references that plan file in every coder and designer delegation instead of summarizing it.

   - The orchestrator micromanaged sub-agents with specific line changes:

     > Tighten the autonomy rules in `.github/agents/orchestrator.agent.md`: delegations must describe the desired outcome only — never file-level edits, function names, or code snippets. Add one more good/bad delegation example.

   Re-run the ticket with the updated agents and compare.

## Expected Outcome
A tested orchestration framework demonstrating the delegation flow (orchestrator → planner → designer → coder) and isolated sub-agent context windows.
