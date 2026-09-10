# Exercise 1.5: Smart Usage

## Objective
Make the most of your Copilot AI credits by managing context and model selection deliberately.

## Background
Since June 2026, Copilot uses usage-based billing: work consumes **AI credits**, and heavier models burn more of them. Every request counts — bloated context, stale conversations, and using frontier models for trivial tasks all cost more than necessary. The good news is the spend is now visible: hover over a response (or a delegated sub-agent section) to see which model handled it and what it cost in credits.

**Auto model selection** is a sensible default: it routes each request to a model based on task complexity (reasoning, code generation, bug diagnosis, tool orchestration) and is billed at a discount compared to pinning the same model manually. Override it deliberately: a heavyweight model for complex reasoning, architecture decisions, and large multi-file changes; a lighter, lower-cost model for boilerplate, docstrings, and simple refactors. Pick a model (or Auto) at the start of a session and stick with it: switching models mid-session invalidates the prompt cache, so the next request rebuilds the full context from scratch and costs more — GitHub's [Auto model selection](https://docs.github.com/en/copilot/concepts/models/auto-model-selection) docs note that mid-session switching "has shown increased cost without ample improvements in quality". Orgs can also bring their own keys (BYOK) for additional or local models — subject to policy.

Other habits that save credits: start fresh chats when you switch tasks (long conversations accumulate irrelevant context), attach only the files and selections that matter (noise dilutes signal and increases token cost), write clear, scoped prompts (one request instead of three), and break large tasks into smaller chunks, committing after each one.

## Steps
1. **See what you're spending.** In a chat session from an earlier exercise, hover over a completed response to see which model handled it and its AI credit cost. Compare a short question with a multi-file agent turn.
2. **Try Auto.** In the model picker, choose **Auto** and run a small task (e.g. "add a docstring to this function"). Hover over the response to see which model Auto chose and what it cost.
3. **Override deliberately.** Start a **fresh chat**, pin a lighter, lower-cost model, and re-run the same small task; then start another fresh chat, pin a heavyweight model, and give it something that needs reasoning (e.g. "propose a design for X"). Compare the costs on hover. Use fresh chats rather than switching the model within one session — a mid-session switch throws away the cached context, so the next turn is billed as if the whole conversation were new.
4. **(Optional)** **Keep side questions out of the main thread.** `/btw` opens a side chat that shares the current session's context, so you can ask a tangential question ("what does this error mean?") without polluting the main session. It is available in the **Agent Sessions view** (not the Chat view) — you'll meet this view in [Exercise 7.4](../../07-agent-roles-and-orchestration/exercises/04-parallel-sessions-and-worktrees.md). If you want to try it now, open a Copilot session in the Agent Sessions view and type `/btw` followed by your question; otherwise, open a new chat for side questions so the main thread stays focused.
5. **Trim context.** Start a new chat for your next task instead of continuing a long one, and attach only the files relevant to the request.
6. **(Optional)** Break a larger change into two or three smaller prompts and commit after each one; notice how each turn stays cheaper and easier to review than a single big request.

## Expected Outcome
You develop a habit of managing context and model choice intentionally, getting better results while spending fewer AI credits. This gets easier once commands and custom agents enter the workflow in later modules.
