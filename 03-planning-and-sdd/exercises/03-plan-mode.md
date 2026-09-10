# Exercise 3.3: Plan Mode

## Objective
Use Plan mode to generate an implementation plan from your ticket, and understand how the built-in Plan agent's prompt shapes that plan.

## Background
Plan mode is a chat mode where the built-in **Plan agent** researches your codebase with read-only tools, breaks the task into actionable steps, and lists open questions — without making any code changes until you approve. Once approved, you hand the plan off with **Start Implementation** or open it in the editor to keep a copy. The Plan agent is driven by a prompt you can inspect — and, if you want plans that follow your team's conventions, you can define your own planning agent as a custom agent — so it's worth reading that prompt once so you know what the plan will and won't cover.

## Steps
1. **Review the Plan agent's prompt.** Open the Command Palette (`Ctrl+Shift+P` (Windows/Linux) / `Cmd+Shift+P` (macOS)), run **Chat: Configure Custom Agents**, and select **Plan**. Skim how the prompt steers the agent toward research, step breakdown, and open questions.
2. **(Optional) Create a custom planning agent.** The built-in Plan agent's prompt can't be edited in place — VS Code's "Customize planning" guidance is to define a custom agent with your own planning instructions instead. In Agent mode, ask the agent to create one in `.github/agents/` (the same custom-agent format you met in [Exercise 2.2](../../02-guardrails-and-safety/exercises/02-manage-agent-tools.md)). For example:

   ```
   Create a custom agent at .github/agents/team-planner.agent.md that plans implementation work like the built-in Plan agent: research the codebase with read-only tools, break the task into actionable steps, and list open questions — without making code changes. In addition, every plan must start with a "Branch & commits" section that names the feature branch and defines one commit per plan step, following our team's branching and commit strategy.
   ```

   If you create one, select it from the agents dropdown in the next step instead of **Plan**.

3. **Switch to Plan mode.** Open Copilot Chat and select **Plan** from the agents dropdown.
4. **Generate the plan.** Reference the ticket from [Exercise 3.1](01-refine-idea.md) and the exploration findings from [Exercise 3.2](02-explore.md) (if you did 3.2). Let the Plan agent generate a step-by-step implementation plan.
5. **Review the generated plan.** Glance over the steps and open questions; answer the questions or ask for changes until the plan matches your intent.
6. **Break it down if needed.** If it's a large task, ask the Plan agent to split it into smaller sub-tasks that can each be implemented and committed on their own.
7. **Save the plan.** The Plan agent only uses read-only tools, so it won't write the plan to your repo itself — it auto-saves to session memory (`/memories/session/plan.md`, viewable via **Chat: Show Memory Files**), which is cleared when the conversation ends. When you're happy with the plan, use the option to open the plan in the editor and save it as `plans/<feature>.md` (use the same kebab-case feature name as your ticket) so you can reference it in [Exercise 3.4](04-implement.md). Alternatively, select **Start Implementation**, pick an implementation agent, and make your first request to that agent "save this plan to `plans/<feature>.md` before doing anything else" — the plan and conversation context carry over to it.

## Expected Outcome
You have an implementation plan saved in `plans/` that was generated from your ticket, and you understand how the Plan agent's prompt shapes it — and how a custom planning agent can change that.
