# Exercise 8.4: Database Specialist Agent with llms.txt

## Objective
Build a database agent that reads a locally committed `llms.txt` file as its ORM reference — so it never guesses schema types, relation syntax, or migration commands.

## Background

This exercise applies the same `llms.txt` grounding pattern from [Exercise 8.3](03-frontend-specialist-agent.md) (Frontend Specialist) to a backend concern: database schema design and migrations.

Generic coding agents hallucinate ORM APIs just as readily: the right decorator name with the wrong options, outdated relation syntax, or non-existent CLI flags — producing migrations that fail at runtime or silently corrupt data. The root cause is the same as with frontend component libraries: training data is a point-in-time snapshot and ORM APIs shift between major versions.

By committing the ORM's `llms.txt` to your repo and pointing an agent at it, you give it a current, authoritative reference it reads at task time instead of guessing.

## Prerequisites
- An ORM installed in your project — or one you're willing to adopt now (step 1 helps you pick one)
- [Exercise 8.3](03-frontend-specialist-agent.md) completed — this exercise reuses its download-and-configure pattern

## Steps

### 1. Choose your ORM

Use the ORM already installed in your project. If you haven't set one up yet, pick one now.

ORMs with known `llms.txt` support:

| ORM | llms.txt URL |
|-----|-------------|
| Prisma | `https://www.prisma.io/docs/llms.txt` |
| Drizzle ORM | `https://orm.drizzle.team/llms.txt` |

If your ORM isn't listed, check `[orm-docs-site]/llms.txt` — the standard is spreading quickly. If yours doesn't have one, use Prisma or Drizzle for this exercise.

### 2. Download both llms.txt variants

Open the `llms.txt` URL for your chosen ORM in your browser to confirm it loads, then ask Copilot to download both variants into `docs/`, exactly as in Exercise 8.3:

> Download `https://[orm-site]/llms.txt` to `docs/llms.txt` and `https://[orm-site]/llms-full.txt` to `docs/llms-full.txt`.

| File | Purpose |
|------|---------|
| `[orm-site]/llms.txt` → `docs/llms.txt` | Compact index — schema types, relation patterns, CLI commands. The agent reads this first on every task. |
| `[orm-site]/llms-full.txt` → `docs/llms-full.txt` | Full API documentation. The agent reads this only when it needs detailed syntax beyond what the index covers. |

> **Note:** If your project already has `docs/llms.txt` from Exercise 8.3 (frontend), have the agent put the ORM docs in a subfolder instead: `docs/orm/llms.txt` and `docs/orm/llms-full.txt`. Step 3 then has the agent point the agent file at the new paths.

### 3. Copy and configure the agent

Copy `database.agent.md` from the `assets/` folder into `.github/agents/` in your project. Then ask Copilot to configure it for your project:

> Open `.github/agents/database.agent.md` and replace `[ORM]` with the ORM this project uses — check `package.json` to confirm. If the ORM docs live in `docs/orm/`, update every `docs/llms.txt` and `docs/llms-full.txt` reference in the file to match.

### 4. Test: add a new table with relations

Select the `database` agent from the agents dropdown in Chat and give it a schema task:

> Add a `posts` table with columns: id (primary key), title (string), body (text), published (boolean, default false), createdAt (timestamp). Each post belongs to a user — add the foreign key and relation.

Watch the PLAN phase:
- Does the agent read `docs/llms.txt` before writing any schema code?
- Does it identify the correct relation syntax from the docs?
- Does it flag anything about the existing schema it needs to understand first?

At the end, the agent should hand off — telling you the exact CLI command to generate the migration. Run it yourself. If the migration applies cleanly, the types and relation syntax were correct. Notice that the agent can't run commands itself (`execute` is not in its tools list) — this is a deliberate guardrail since migration commands can be destructive.

### 5. Test: a trickier migration

Give the agent a task that requires more careful handling:

> Add a many-to-many relation between `posts` and a new `tags` table. Each tag has an id and a name (unique). A post can have many tags, and a tag can belong to many posts.

Many-to-many relations are where agents most often hallucinate — the join table syntax varies significantly between ORMs and versions. This is where `llms.txt` grounding earns its keep.

### 6. Reflect on the pattern

- Compare this exercise to Exercise 8.3 (Frontend Specialist). What's the same? What's different? The pattern — commit docs, point the agent at them, constrain what it can touch — is identical. Only the domain changed.
- What happens if you remove the `docs/llms.txt` file and run the same task? Does the agent still produce a working migration?
- The agent's guardrail prevents it from touching application code. Why is this important for a database agent specifically? (Think about what happens if a schema change requires service-layer updates.)

## Expected Outcome

Both `docs/llms.txt` (or `docs/orm/llms.txt`) and the full variant committed to your project, and a `database.agent.md` in `.github/agents/` pointing at them. When you give the agent a schema task it reads the compact index first and falls back to the full file for detailed syntax. The migration applies cleanly without hallucinated ORM APIs. To update for a new ORM version, re-download the files into `docs/` — the agent needs no changes.
