# Exercise 6.5: Create a Custom Skill

## Objective
Scaffold a new custom skill with the built-in `/create-skill` command, test it end-to-end, and (optionally) compare the result with what the community skill-creator meta-skill produces.

## Background
Creating a skill needs no third-party tooling. VS Code ships three native paths: the `/create-skill` chat command (the agent asks clarifying questions, then generates the folder, `SKILL.md`, frontmatter, and workflow), the Agent Customizations editor (Configure Chat gear → **Skills** → **New Skill**), and extracting a skill from an ongoing conversation. This is the same pattern you've used all course: `/init` for instructions, `/create-agent` for agents, `/create-hook` for hooks.

The community ecosystem also has a **skill-creator** — a meta-skill, a skill that creates other skills. It encodes its own authoring opinions (progressive loading discipline, tight descriptions, bundled scripts and references). The optional last step compares the two so you can judge for yourself when the built-in is enough and when the meta-skill's opinions add value.

## Steps
1. **Pick something repetitive.** Think of a task in your workflow that could benefit from a packaged skill. Some ideas:
   - A skill that generates changelog entries from recent git commits
   - A skill that scaffolds new API endpoints with tests
   - A skill that summarizes open pull requests for a standup

2. **Run `/create-skill`.** Type it in Copilot Chat and describe the skill. For example:

   > */create-skill a skill that generates a changelog entry from the last 5 git commits, formatted as a markdown list with commit message, author, and date.*

   Answer the clarifying questions the agent asks — scope and output format decisions here become the skill's workflow.

3. **Glance at the generated structure.** Check that it includes:
   - A `SKILL.md` with valid frontmatter (`name` and `description`) — remember from Exercise 6.1 that these two fields decide whether the skill ever loads
   - A workflow section with clear, imperative steps
   - Any supporting files (scripts, templates, references) referenced by relative paths from `SKILL.md`

4. **Test the skill.** Invoke it by name or as a slash command — for example, *"Use the changelog skill to summarize the last few commits"* or `/changelog`. Auto-detection based on description alone is unreliable, so explicitly naming the skill is the recommended approach (consistent with Exercises 6.2, 6.3, and 6.4).

5. **Iterate if needed.** If the skill doesn't behave as expected, prompt the agent to adjust it. For example:

   > *"The changelog skill should also group commits by author."*

   The agent should update the existing skill files rather than starting from scratch.

6. **(Optional) Extract a skill from a conversation.** The second native path: after any chat where you walked the agent through a multi-step procedure, ask it to *"turn what we just did into a skill"*. This captures working procedures as reusable capabilities without describing them from scratch.

7. **(Optional) Compare with the skill-creator meta-skill.** Copy the `skill-creator/` folder from the `assets/` folder into `.github/skills/skill-creator/`, then regenerate the same skill through it:

   > *"Use the skill-creator skill to create a skill that generates a changelog entry from the last 5 git commits."*

   Put the two scaffolds side by side. Where do they differ — description quality, workflow structure, bundled scripts? A skill that creates skills is itself a nice demonstration of what skills can package: not just knowledge or tools, but authoring judgment.

## Expected Outcome
A working custom skill scaffolded by the built-in `/create-skill` command — created by the agent, not by hand — with proper frontmatter and workflow, triggering when invoked by name or slash command. If you did the optional comparison, you also have a feel for what the community skill-creator's authoring opinions add over the native scaffold.
