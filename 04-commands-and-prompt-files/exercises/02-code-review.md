# Exercise 4.2: Code Review Command

## Objective
Create a custom prompt file that performs a structured code review across multiple dimensions — security, naming, complexity, test coverage, and error handling.

## Background
Asking Copilot to "review this code" gives you a loose, conversational response. A custom review command defines exactly what to look for and how to report it. The result is consistent, thorough, and actionable.

> **Note:** Before reaching for a custom prompt, it's worth knowing that Copilot has a built-in code review feature for **uncommitted changes** — in the **Source Control** panel, hover over **CHANGES** and click the **Copilot Code Review - Uncommitted Changes** button. Copilot reviews all your local changes at once. The native review is a solid quick-pass option. The custom prompt file approach in this exercise goes further — it gives you full control over the review dimensions, output format, and severity grouping.

## Steps

1. **Create the prompt file.** Copy `structured-review.prompt.md` from the `assets/` folder into `.github/prompts/` in your project. Skim it before continuing.

2. **Inspect the frontmatter.** The frontmatter currently has `name` and `description`. The `name` value is what becomes the slash command — `name: structured-review` is why you will type `/structured-review` in step 6 (without it, the file name would be used instead). The command is not named plain `review` because `/review` is already taken by Copilot's built-in review command — pick names that don't collide with built-ins. **(Optional)** Ask Copilot to add a `model:` field to the frontmatter to pin a model that's strong at catching security issues or reasoning about code quality, for example:

   > Add `model: GPT-5.2 (copilot)` to the frontmatter of `.github/prompts/structured-review.prompt.md`.

   > **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one.

3. **Review dimensions.** Notice how the file checks for issues across five dimensions — security, naming & readability, complexity, test coverage gaps, and error handling — each with concrete examples of what to look for. **(Optional)** Ask Copilot to add a dimension that matters to your team, e.g. "Add a PERFORMANCE dimension to the review prompt covering N+1 queries and unnecessary re-renders."

4. **Output format.** Notice how the file defines exactly how results are reported: a one-paragraph summary, then issues grouped by severity (critical, warning, suggestion), each with a file and line, a description, and a concrete fix. Categories with no findings still say "None found" so nothing is silently skipped.

5. **Guardrails.** Notice how the hard rules keep the review a review: don't rewrite the code, be specific about locations, separate style from logic, and flag uncertain findings as "worth checking" rather than definitive issues.

6. **Test it.** Run `/structured-review` on real code from your project (reference a file or select some code first). Evaluate:
   - Did it catch issues across multiple dimensions, or only one?
   - Is the output structured and easy to act on?
   - Did it flag anything you hadn't noticed?

7. **Reflect.** Compare the structured review output with what you'd get from a casual "review this code" prompt. The structured version should be more consistent, more thorough, and easier to triage.

### Optional: Turbo review

> **Prerequisite:** Sub-agents require the `agent/runSubagent` tool. Enable it in the Chat tools picker (the tools icon in the Chat view). The turbo prompt also lists it in its `tools` frontmatter as `agent`.

8. **(Optional)** **Run the turbo review.** The `assets/` folder also includes `review-turbo.prompt.md` — a variant that spawns three sub-agents, asks each to use a different model (Gemini 3 Flash, GPT-5.2, Claude Sonnet 4.5) if available, and compiles their findings into a single report. Copy it into `.github/prompts/` and run `/review-turbo` on the same code you used in step 6. Be aware that this consumes significantly more tokens than the standard review since it runs three reviews in parallel.

   > **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one. Per-sub-agent model selection is a request to the agent, not a guarantee; sub-agents may fall back to the current chat model.

9. **(Optional)** **Compare.** Skim both reports. Did the three reviewers agree on the critical findings? Did the turbo version surface anything the single review missed — and was the extra cost worth it?

## Expected Outcome
A reusable `structured-review.prompt.md` command that produces structured, severity-grouped code review feedback. You understand how defining clear dimensions and output formats makes AI-assisted reviews more useful than open-ended prompts.
