# Exercise 6.2: Use the Frontend Design Skill

## Objective
Install a simple, single-file skill and observe how it changes the agent's design output compared to your project's existing frontend.

## Background
The frontend design skill (`frontend-design`) is a "knowledge skill" — it doesn't give the agent new tools, it changes *how the agent thinks* about a task. When loaded, it steers the agent away from generic AI aesthetics (predictable layouts, overused fonts like Inter/Roboto, clichéd purple gradients) and toward distinctive, intentional design choices. Because it's a single `SKILL.md` with no scripts or references, it's one of the simplest skills you can install — a good starting point before moving to more complex tool-based skills like the Playwright CLI skill in Exercise 6.3.

## Prerequisites
- Your project should have at least one page or component with existing UI that you can compare against.

## Steps
1. **Install the skill.** Copy the `frontend-design/` folder from the `assets/` folder into your project's `.github/skills/frontend-design/`.

2. **Trigger the skill and build a component.** Start a chat and ask the agent to use the skill by name — for example:

   > *"Use the frontend-design skill to create a landing page hero section for a coffee shop website."*

   The agent will recognize the skill name in your prompt and load it on demand. You can also trigger skills as slash commands: type `/frontend-design`, press **Tab** to select it, then continue typing your prompt after it.

   Watch the design choices the agent makes: fonts, colors, layout, overall feel.

3. **Compare with your existing frontend.** Open your project's current UI side-by-side with the skill's output. Glance at the differences in:
   - **Typography** — does the skill pick more distinctive, intentional fonts?
   - **Color palette** — committed aesthetic vs. safe/generic defaults?
   - **Layout** — unexpected composition vs. predictable patterns?
   - **Details** — animations, textures, hover states, spatial choices?

4. **Reflect on progressive loading.** Skills are only fully loaded when the agent decides it needs them — or when you explicitly request it. Invoking the skill as a slash command or naming it in your prompt is the most reliable way to ensure it's loaded. This is the efficiency benefit over instruction files, which load on every session regardless.

> **Note:** Skills vs. custom instructions — when to use which? You could achieve a similar effect by putting these design guidelines into an instruction file ([Module 01](../../01-context-and-cost/README.md)). The difference is *when* the guidance loads. Instruction files are injected into *every* conversation turn — great for rules that always apply (coding conventions, language standards, repo structure). A skill only loads when the agent decides it's relevant or when you explicitly trigger it. That makes skills the better fit for task-specific knowledge like frontend design: there's no reason to spend context tokens on typography guidelines when the agent is writing backend logic. As a rule of thumb: if the guidance applies to every task, use instructions; if it only matters for a specific type of work, use a skill or a specialized agent.
>
> In [Module 08](../../08-specialist-agents/README.md), you'll see how the frontend specialist agent can reference this skill — combining component-library correctness (via `llms.txt`) with design standards (via this skill) in a single workflow.

## Expected Outcome
A visibly distinctive design output compared to your project's existing frontend — demonstrating how a single `SKILL.md` file can meaningfully shift the agent's design behavior without any scripts or tool configuration.
