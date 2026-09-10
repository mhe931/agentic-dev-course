---
name: review-turbo
description: Multi-model code review — spawns 3 sub-agents with different models and compiles findings into a single report. Uses significantly more tokens than the standard review.
tools: ['agent', 'read', 'search']
---

Perform a comprehensive code review of the provided code, following the structured [review guidelines](./structured-review.prompt.md). Focus on identifying issues across the specified dimensions: security, naming & readability, complexity, test coverage gaps, and error handling. Provide actionable feedback categorized by severity (critical, warning, suggestion) with specific locations and suggested fixes.

Spawn 3 sub-agents that each review the same code independently. Ask each sub-agent to use a different model if available — Gemini 3 Flash, GPT-5.2, and Claude Sonnet 4.5 — and pass each one the full review guidelines linked above together with the code or files to review. When all three have finished, deduplicate and merge their findings into a single report that uses the output format from the review guidelines, and note which findings were raised by more than one reviewer.
