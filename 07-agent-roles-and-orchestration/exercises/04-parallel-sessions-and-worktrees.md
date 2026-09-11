# Exercise 7.4: Parallel Sessions and Worktrees

## Objective
Run two agent sessions side by side — one of them in an isolated git worktree — and learn when parallel sessions beat the orchestrator pattern from Exercises 7.2–7.3.

## Background
The orchestration framework you just built is one way to multiply agents: a coordinator delegating to sub-agents *inside a single session*. VS Code now has a second, native pattern: the **Agent Sessions view** runs multiple independent sessions *in parallel*, each with its own context, model, and optionally its own **git worktree** — an isolated copy of the repository, so two agents can edit code at the same time without stepping on each other's changes.

The Agent Sessions view brings all of this into one place: local sessions, background sessions, and each session's sub-agents (with their model, elapsed time, and active tool calls visible). You can group and reorder sessions, review diffs alongside the chat, and — depending on your setup — run sessions on other harnesses like Claude or Codex under the same Copilot subscription.

Rule of thumb for choosing a pattern:

| Pattern | Best for |
|---------|----------|
| Orchestrator + sub-agents (7.2–7.3) | One task with dependent steps — plan feeds code feeds design; results must combine into one change |
| Parallel sessions + worktrees (this exercise) | Independent tasks — a feature and a bugfix, or two features touching different areas; nothing needs to wait |

The patterns compose: each parallel session can itself be your orchestrator agent.

## Prerequisites

> **Prerequisite:** The Agent Sessions view shipped in VS Code **1.109 (January 2026)** and is controlled by the `chat.viewSessions.enabled` setting (default on). Worktree sessions and the surrounding UI still move between releases, and your organization may restrict agent features via Copilot policies — if you don't see the options described below, check your VS Code version and settings first. Running other harnesses is optional: Claude sessions require `github.copilot.chat.claudeAgent.enabled` in VS Code settings, and Codex sessions are only available on Copilot Pro+ and Max plans. The exercise works fine with Copilot-only sessions.

## Steps

1. **Prepare two independent tasks.** Ask the agent to help you split work, e.g.:

   > Look at tickets/ and my open TODOs. Propose two small, genuinely independent tasks that touch different parts of the codebase — no shared files. One should be a code change, the other can be a fix, cleanup, or docs task.

   If you have no backlog handy, use the `/spar` prompt file from [Module 03](../../03-planning-and-sdd/README.md) to refine two quick ideas.
2. **Open the Agent Sessions view** and start a session for the first task in your normal working tree. Run at least one of the two sessions with your `orchestrator` agent from [Exercise 7.2](02-build-orchestration-framework.md) (choose it from the agents dropdown) so it delegates to sub-agents — that gives step 4 something to expand.
3. **Start the second session in a worktree.** When creating the new session, pick the worktree option so this agent works on an isolated copy of the repo. Give it the second task (with the `orchestrator` agent if you didn't use it in step 2) and let both sessions run concurrently.
4. **Watch them work.** Switch between the sessions: note each session's status, and in the orchestrator-driven session expand a sub-agent entry to see its model, elapsed time, and tool calls. This is the same delegation you built in Exercise 7.2 — now visible in the UI.
5. **Review and land the results.** Use the diff view next to each chat for a quick review of what changed. For the worktree session, have the agent bring the work back rather than doing git surgery yourself:

   > Your changes look good. Commit them on a branch and merge that branch back into my current branch.

6. **Reflect.** Both tasks finished without sharing a context window or a working tree. When would you reach for this instead of the orchestrator — and when would you combine them (e.g., two parallel sessions, each running the orchestrator on its own worktree)?

## Expected Outcome
Two completed tasks from two concurrent agent sessions, one merged back from an isolated worktree — and a working decision rule for orchestrator-style delegation versus parallel sessions.
