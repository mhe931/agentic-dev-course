# Exercise 3.1: Refine a Feature Idea with the `/spar` Prompt File

## Objective
Use the `/spar` prompt file to turn a rough feature idea into a well-defined ticket that's ready for planning and implementation.

## Background
Writing a good spec starts with having a clear idea — and most ideas aren't clear until you pressure-test them. This exercise uses a **prompt file** (a reusable prompt workflow defined in markdown, invoked as a slash command in Copilot Chat) that guides the AI through a structured sparring conversation. By the end, you'll have a feature ticket you can feed into Plan mode in [Exercise 3.3](03-plan-mode.md).

A ready-to-use `/spar` prompt file is provided in [`../assets/spar.prompt.md`](../assets/spar.prompt.md). It walks through four phases:

1. **Capture** — Restate the idea in one sentence and confirm
2. **Clarify** — Ask 3-4 targeted questions (user, trigger, success criteria, scope)
3. **Challenge** — Surface edge cases and ambiguities
4. **Ticket** — Generate a feature ticket as a markdown file in `tickets/`

> **Note:** Prompt files are on by default in VS Code and are discovered from your project's `.github/prompts` folder (controlled by the `chat.promptFilesLocations` setting). If `/spar` doesn't show up in the slash-command list, check that setting. [Module 04](../../04-commands-and-prompt-files/README.md) covers prompt files in depth — here you only need to run one.

## Steps

1. **Create a feature branch.** Before starting, branch off from main so all your planning and implementation work stays isolated and easy to review.

2. **Set up.** Copy `spar.prompt.md` from the `assets/` folder into your project's prompt files directory, `.github/prompts/`. Skim the prompt — note the hard rules, the phase ordering, and the ticket format.

3. **(Optional) Customize.** If you want `/spar` to fit your domain or team template, ask the agent to revise the prompt file for you rather than editing it by hand. For example, in Agent mode:

   ```
   Update .github/prompts/spar.prompt.md: add a Clarify question about which API or service the feature touches, and change the ticket format to match our team's template (Summary, User Story, Acceptance Criteria, Out of Scope).
   ```

4. **Run it.** Type `/spar` in Copilot Chat (Agent mode) and follow the phases with a real feature idea — something you'd actually want to build. Don't rush the Clarify and Challenge phases; they're where the real value is.

5. **Evaluate the ticket.** Once `/spar` produces the ticket file, glance over it:
   - Are the acceptance criteria specific and testable?
   - Do the edge cases reflect real risks, not generic boilerplate?
   - Would a developer understand what to build from this ticket alone?

6. **(Optional) Iterate.** If the ticket quality is weak, ask the agent to sharpen the prompt file and run `/spar` again. For example:

   ```
   The ticket produced by /spar had vague acceptance criteria. Revise .github/prompts/spar.prompt.md so the Clarify phase asks for measurable outcomes, add one example of a good vs. bad acceptance criterion, and make the Challenge phase focus on failure modes specific to this codebase.
   ```

## Expected Outcome
A ticket saved as a markdown file in `tickets/` that is concrete enough for someone else (or an agent) to implement without ambiguity.
