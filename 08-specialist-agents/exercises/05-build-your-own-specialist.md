# Exercise 8.5: Build Your Own Specialist Agent

## Objective
Design and build your own specialist agent grounded in documentation that you source and commit to the repo — without relying on a published `llms.txt` file.

## Background

Exercises 8.3 and 8.4 used libraries that publish `llms.txt` files — a convenient, ready-made format. But most tools, frameworks, and internal systems don't have one. The grounding pattern still works; you just need to build the reference yourself — or rather, have the agent build it for you.

There are several ways to get authoritative documentation into your codebase:

| Approach | When to use | Example |
|----------|------------|---------|
| **Use `llms.txt`** | The tool publishes an `llms.txt` — check `[tool-site]/llms.txt`. This is the easiest option when available. | Many libraries already publish one — see [llmstxthub.com](https://llmstxthub.com) for a directory |
| **Fetch official docs pages** | The tool has good online docs but no `llms.txt`. Have the agent fetch the relevant pages and condense them into a markdown file. | Terraform provider docs, GitHub Actions syntax reference |
| **Export CLI help output** | The tool is CLI-driven and `--help` is the best reference. | `docker build --help`, `gh pr create --help` |
| **Write an internal reference** | The knowledge lives in your team's heads, not in any public docs. | Coding standards, deployment checklists, infrastructure policies |
| **Combine multiple sources** | No single doc covers what the agent needs. Curate from several. | A CI/CD agent that needs both GitHub Actions syntax and your project's deployment conventions |

The key constraint is the same as before: the agent reads the reference at task time instead of guessing from training data. How the reference was created doesn't matter — what matters is that it's in the repo and the agent knows where to find it.

## Prerequisites
- [Exercise 8.3](03-frontend-specialist-agent.md) and [Exercise 8.4](04-database-specialist-agent.md) completed — this exercise generalizes their pattern

## Steps

### 1. Pick a domain

Choose a tool, framework, or workflow in your project that an agent could specialize in. Some ideas:

- **CI/CD pipelines** — an agent that writes or reviews GitHub Actions workflows
- **Infrastructure** — an agent that audits Terraform/Bicep files against team policies
- **API design** — an agent that generates or reviews OpenAPI specs
- **Containerization** — an agent that writes or improves Dockerfiles
- **Testing** — an agent grounded in your testing framework's API docs
- **Anything else** where you've seen an agent hallucinate or give outdated advice

### 2. Source the documentation

This is the new challenge: there is no ready-made `llms.txt`, so have Copilot fetch and curate the reference for you and commit it to `docs/agent-context/` (or another subfolder of `docs/`). Pick the approach from the table above that fits your domain and prompt accordingly — for example, for online docs:

> Fetch `https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax` and write a compact reference of the workflow syntax (keys, allowed values, one-line descriptions) to `docs/agent-context/github-actions.md`. Keep it under 300 lines and link each section back to its source URL.

or, for a CLI tool:

> Run `docker build --help` and `docker compose --help` and turn the output into `docs/agent-context/docker.md` — group the flags by purpose and drop anything marked deprecated.

> **Note:** Fetching URLs requires the built-in fetch tool (`web` in an agent's `tools` list) to be enabled in the Chat tools picker. If it isn't available to you, reference the page with `#fetch <url>` in your prompt, or let the agent download it with `curl` in the terminal.

Before you accept the result, sanity-check two things:

- **Is the source authoritative?** Official docs and team-agreed standards are good. Blog posts and Stack Overflow answers are not — if the agent pulled from one, ask it to replace that section with the official source.
- **How will you keep it up to date?** Ask the agent to record the source URL and the tool version at the top of the file, so the same prompt regenerates the reference when the tool ships a new release. If you expect to do this often, turn the prompt into a prompt file ([Module 04](../../04-commands-and-prompt-files/README.md)) so anyone on the team can re-run it.

### 3. Structure it: full docs + compact index

Committing the docs is only half the job — how you structure them matters. [Vercel found](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) that agents with a compact index pointing to full docs in the repo achieved a 100% pass rate, compared to 53% with no docs at all. The two-layer pattern:

- **Compact index**: A concise summary of key concepts, API endpoints, or commands with links to the full docs. This is what the agent reads first — it gives the agent a mental model and a roadmap for where to find details.
- **Full docs**: The complete reference — the fetched pages, the raw `--help` output, or the internal standard in full — stored alongside the index. The agent reads this only when it needs detail beyond what the index covers.

This is the same pattern Exercises 8.3 and 8.4 used with `llms.txt` (index) and `llms-full.txt` (full docs) — but you can build it yourself for any documentation source. The index keeps the agent's context window small while ensuring it always knows where to look.

If step 2 produced a single large file, ask the agent to split it:

> Split `docs/agent-context/<tool>.md` into two files: keep `docs/agent-context/<tool>.md` as a compact index (under 150 lines, one line per concept with a link to the section in the full file) and move the complete content to `docs/agent-context/<tool>-full.md`.

> **Note:** Why this works — a compact index is passive context: the agent always has it loaded, so it never has to decide whether to look something up. Full docs without an index force the agent to search blind.

### 4. Build the agent

Decide the four ingredients first, then ask Copilot (or `/create-agent`) to generate `.github/agents/<name>.agent.md` from your description, tools, rules, and workflow:

- **Description** that explains what the agent does and what docs it reads
- **Tools list** — think carefully about what the agent needs. Does it need `execute`? If the domain involves destructive commands, leave it out.
- **Rules** — at minimum: "read the docs before acting" and "only touch files in your domain"
- **Workflow** — PLAN (read docs, understand current state) → BUILD (make changes grounded in docs) → a final phase (VERIFY, HAND OFF, or REPORT depending on whether the agent can safely check its own work)

For example:

> Create `.github/agents/ci.agent.md`: a specialist that writes and reviews GitHub Actions workflows. Before acting it must read `docs/agent-context/github-actions.md` and consult `docs/agent-context/github-actions-full.md` for details. Tools: `read`, `edit`, `search` — no `execute`. Rules: never guess workflow syntax, only touch files under `.github/workflows/`. Workflow: PLAN → BUILD → HAND OFF (tell me how to validate the workflow).

For a worked example of a specialist that isn't grounded in `llms.txt`, glance at `review.agent.md` in the `assets/` folder — a read-only review agent that selects static-analysis tools by changed file type and produces a severity-ranked report. Compare it with the `/structured-review` prompt file from [Exercise 4.2](../../04-commands-and-prompt-files/exercises/02-code-review.md): the prompt file defines the review steps, while the agent additionally constrains its tools (no `edit`), can be selected from the agents dropdown, and can be called as a sub-agent by other agents.

### 5. Test it

Give the agent a real task from your project. Watch for:
- Does it read your committed docs before acting?
- Does it stay within its guardrails?
- Is the output correct according to the docs you provided?

If it hallucinates despite having the docs, check whether your reference is specific enough — vague docs produce vague results. Ask the agent to expand the relevant section of the index.

### 6. Reflect

- How did you decide what to include in the reference and what to leave out?
- How does the quality of the committed docs affect the quality of the agent's output?
- Could someone else on your team use this agent without knowing how you built it? If not, what would you add?

## Expected Outcome

A new specialist agent in `.github/agents/` with a committed reference (compact index + full docs) in `docs/agent-context/`. The agent reads the reference before acting and stays within its defined guardrails. You've practiced the full loop: identify a domain, have the agent source and structure the docs, generate the agent, and test it — a pattern you can repeat for any tool or workflow.
