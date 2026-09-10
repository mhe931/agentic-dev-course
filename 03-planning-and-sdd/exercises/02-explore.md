# Exercise 3.2: Explore Before You Plan

> **Optional:** This exercise is optional. You can also explore by iterating the plan in [Exercise 3.3](03-plan-mode.md).

## Objective
Use Ask mode to explore the codebase before generating a plan — so the plan is grounded in how the code actually works, not assumptions.

## Background
Jumping straight from a spec to a plan is tempting, but plans that ignore the existing codebase tend to drift. A short exploration phase lets you (and the AI) understand the current architecture, conventions, and constraints before committing to an approach.

## Steps
1. **Open Copilot Chat in Ask mode.** Ask mode is read-only — it can read files and answer questions but won't make changes. This is exactly what you want during exploration.
2. **Ask targeted questions about the area your feature touches.** Use the ticket from [Exercise 3.1](01-refine-idea.md) as context. Good questions to start with:
   - "Where does [relevant concept] live in the codebase?"
   - "How does the current [related feature] work end-to-end?"
   - "What patterns or conventions does this project use for [relevant area]?"
3. **Follow up.** If the answers reveal something unexpected — an abstraction you didn't know about, a naming convention, a dependency — dig in. The goal is to surface anything that would change your implementation approach.
4. **Summarize what you learned.** Before moving to Plan mode in [Exercise 3.3](03-plan-mode.md), note down 2-3 key findings that should inform the plan (e.g., "there's already a utility for X", "the project uses Y pattern for this kind of thing").

## Expected Outcome
You understand the relevant parts of the codebase well enough that the plan you generate next will work with the existing architecture, not against it.
