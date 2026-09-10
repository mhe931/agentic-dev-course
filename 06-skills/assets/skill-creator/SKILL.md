---
name: skill-creator
description: Scaffolds new Copilot skills with proper SKILL.md frontmatter, folder structure, and supporting files. Use when the user wants to create, design, or scaffold a new skill for their project.
---

## Skill anatomy

A skill is a folder with a `SKILL.md` file and optional supporting files:

```
.github/skills/<skill-name>/
├── SKILL.md              # Entry point — frontmatter + workflow
├── scripts/              # Executable scripts (Node, Python, shell)
├── templates/            # Response format templates
└── references/           # Detailed docs for complex topics
```

## Required frontmatter

Every `SKILL.md` must start with YAML frontmatter:

```yaml
---
name: <skill-name>
description: <specific description of what the skill does and when to use it>
---
```

The `description` is critical — it is the only thing the agent sees on first pass to decide whether to load the full skill. Make it specific and action-oriented. Mention the concrete tasks the skill handles (e.g., "generates changelog entries from git commits" not "helps with changelogs").

If the skill runs shell commands, add tool restrictions:

```yaml
allowed-tools: Bash(<command-prefix>:*)
```

## Workflow

When the user asks you to create a skill, follow these steps:

1. **Clarify the use case.** Ask what the skill should do, when it should trigger, and what commands or tools it needs. Identify 2–3 trigger phrases a user might say that should activate this skill.
2. **Plan the structure.** Decide what supporting files the skill needs:
   - **Scripts** — for gathering data, running tools, or automating tasks. Scripts should output to stdout.
   - **Templates** — for structuring the skill's output format. Use placeholders for dynamic data.
   - **References** — for detailed documentation on complex topics. Keeps SKILL.md short.
3. **Create the skill folder** at `.github/skills/<skill-name>/`.
4. **Write `SKILL.md`** with:
   - Proper frontmatter (`name`, `description`, optionally `allowed-tools`)
   - A concise workflow section written in imperative style
   - Relative-path references to any supporting files (e.g., `./scripts/gather.sh`, `./templates/response.md`)
5. **Create supporting files** (scripts, templates, references) as planned.
6. **Verify the structure** — confirm SKILL.md frontmatter is valid YAML, relative paths resolve correctly, and scripts are executable.

## Best practices

- Keep `SKILL.md` under ~100 lines. Move detailed docs to `references/`.
- Write descriptions that match how users naturally phrase requests.
- Use imperative style in workflows ("Run the script", "Format the output").
- Reference files with relative paths from `SKILL.md`.
- Restrict tool access with `allowed-tools` when the skill runs shell commands.
- Include a short example in `SKILL.md` showing the skill's expected input → output.
- One skill per capability — don't bundle unrelated tasks.
