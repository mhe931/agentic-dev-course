# Exercise 1.3: Path-Specific Instructions

## Objective
Add targeted instructions that apply only to specific directories or file types in your project.

## Background
Repo-wide instructions apply to every request. Path-specific instruction files (`.github/instructions/*.instructions.md`) narrow that down: an `applyTo` glob in the YAML frontmatter tells Copilot to include the file only when the request involves matching files. This keeps area-specific rules (API validation, test conventions, frontend styling) out of the always-loaded repo-wide file. `.github/instructions/` is the default location — no setting is needed.

For example, `.github/instructions/api.instructions.md`:

```markdown
---
applyTo: "src/api/**/*.ts"
---

Use Zod for all input validation in API handlers. Never use plain `any` types.
```

## Steps
1. In Copilot Chat (Agent mode), ask Copilot to create the file, e.g.: *"Create `.github/instructions/api.instructions.md` with an `applyTo` glob matching `src/api/**/*.ts` and our API conventions: use Zod for all input validation in API handlers, never use plain `any` types, and return errors in our standard error envelope."* Adjust the path and conventions to match your project. (Alternatively, run **Chat: New Instructions File** from the Command Palette to scaffold the file first, then ask Copilot to fill it in.)
2. Glance at the result: the frontmatter should contain the `applyTo` glob, and the body should hold only rules for that area of the codebase.
3. Test by asking Copilot Chat to generate code for a file matching the glob, and check the references list in the response to confirm the instruction file was picked up.

## Expected Outcome
Copilot applies different guidance depending on which part of your codebase you're working in.
