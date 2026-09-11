# Exercise 3.4: Implement from the Plan

## Objective
Use Agent mode to implement your feature from the plan generated in Exercise 3.3, using commits as checkpoints to review progress.

## Background
Implementation is where the earlier phases pay off: a clear ticket and a grounded plan mean the agent can work with far less back-and-forth. The remaining risk is drift — the agent taking a step the plan didn't call for and then building on top of it. Committing after each logical chunk gives you cheap rollback points, and Copilot's own checkpoints give you a second, git-free way to rewind.

## Steps
1. **Switch to Agent mode.** Agent mode can autonomously edit files, run terminal commands, and use tools. Feed it the plan from [Exercise 3.3](03-plan-mode.md) — reference `plans/<feature>.md` in your prompt, or continue in the session you handed off with **Start Implementation**, where the plan and conversation context are already loaded.
2. **For larger features, implement in steps.** If the plan has multiple logical chunks, tackle them one at a time — pause and commit after each. These commits are your checkpoints; they let you review what changed and revert if the agent went off track. For smaller, self-contained features you can let the agent run through the whole plan in one pass.
3. **Glance at the diff at each checkpoint.** Before moving to the next step, skim the changes. Ask yourself:
   - Does this match what the plan said?
   - Does it follow the conventions you found in [Exercise 3.2](02-explore.md)?
   - Is there anything you'd change before building on top of it?
4. **Rewind if needed.** If a step went wrong, you have two options:
   - **Git reset** — revert to the last good commit and re-prompt with more specific guidance.
   - **Copilot checkpoints** — Copilot keeps a checkpoint per request (`chat.checkpoints.enabled`, on by default). Hover a previous request in the chat and choose **Restore Checkpoint** to roll your files back. No git commands needed.

   Either way, rewinding early is cheaper than letting errors compound.

## Expected Outcome
Your feature is implemented incrementally with a clean commit history. You've practiced the full cycle: spec → explore → plan → implement with checkpoints.
