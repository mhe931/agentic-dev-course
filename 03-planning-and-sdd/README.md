# Module 03: Planning and Spec-Driven Development

This module builds on the instruction files from [Module 01](../01-context-and-cost/README.md) and the guardrails from [Module 02](../02-guardrails-and-safety/README.md): with context and safety in place, you can now let the agent do bigger pieces of work — as long as you plan first. The `/spar` prompt file used in Exercise 3.1 also previews the reusable prompt files covered in depth in [Module 04](../04-commands-and-prompt-files/README.md).

## Learning Objectives
- Understand spec-driven development (SDD) principles
- Separate exploration, planning, and implementation into distinct phases
- Use Ask mode to explore the codebase before planning
- Use Plan mode to generate and review implementation plans
- Implement from a plan using commit checkpoints (for large tasks) to review progress

## Key Concepts
Spec-driven development (SDD) means writing a clear specification before any code is written. A good spec defines the problem, success criteria, constraints, and key decisions — it's the contract between intent and implementation.

The core discipline is to separate phases: **refine** the idea into a spec, **explore** the codebase to ground your approach, **plan** the implementation, then **implement**. Each phase uses a different mode — Ask mode for exploration, Plan mode for planning, Agent mode for implementation.

## Relevant Documentation
- [Using GitHub Copilot Chat in your IDE](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide) — Chat modes including Plan mode
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning) — The built-in Plan agent

## Exercises
| Exercise | Description |
|----------|-------------|
| [01-refine-idea](exercises/01-refine-idea.md) | Refine a feature idea into a feature ticket using the `/spar` prompt file |
| [02-explore](exercises/02-explore.md) | (Optional) Use Ask mode to explore the codebase before planning |
| [03-plan-mode](exercises/03-plan-mode.md) | Use Plan mode to generate an implementation plan from your ticket, optionally via a custom planning agent, and save it to `plans/` |
| [04-implement](exercises/04-implement.md) | Implement the plan using Agent mode with commit checkpoints |
