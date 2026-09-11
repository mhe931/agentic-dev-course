# Exercise 8.3: Frontend Specialist Agent with llms.txt

## Objective
Build a frontend agent that reads a locally committed `llms.txt` file as its component library reference — so it never guesses APIs and always produces code that compiles.

## Background

Generic coding agents hallucinate component APIs. They might use the right component name but wrong property names, outdated import paths, or missing module registrations — producing code that looks plausible but fails to compile. The root cause is that training data is a point-in-time snapshot; library APIs shift between major versions.

The `llms.txt` standard addresses this. Libraries publish a machine-readable documentation file at `/llms.txt` (and often a full dump at `/llms-full.txt`) that is optimized for LLM consumption. By copying this file into your repository and pointing an agent at it, you give the agent a current, authoritative reference it reads at task time rather than guessing from stale training data.

In this exercise you'll find an `llms.txt` for a component library, commit it to your project, and configure a frontend specialist agent to read it before writing any UI code.

## Prerequisites
- A component library installed in your project's frontend — or one you're willing to adopt now (step 1 helps you pick one)
- (Optional) The `frontend-design` skill from [Exercise 6.2](../../06-skills/exercises/02-use-frontend-design-skill.md), for step 6

## Steps

### 1. Choose your component library

Use the library already installed in your project's frontend. If you haven't used a component library yet, pick one now — you'll use the agent to port your existing UI to it.

Suggested libraries with known `llms.txt` support:

| Framework | Library | llms.txt URL |
|-----------|---------|--------------|
| Angular | PrimeNG | `https://primeng.dev/llms/llms.txt` |
| Angular | NG-ZORRO | `https://ng.ant.design/llms.txt` |
| React | shadcn/ui | `https://ui.shadcn.com/llms.txt` |
| React | Chakra UI | `https://chakra-ui.com/llms.txt` |
| React | Ant Design | `https://ant.design/llms.txt` |
| React | MUI | `https://mui.com/material-ui/llms.txt` |
| React | Mantine | `https://mantine.dev/llms.txt` |

If your library isn't in the list, check `[library-site]/llms.txt` — many libraries now publish one. If yours doesn't have one, pick a different library for this exercise.

### 2. Download both llms.txt variants

Open the `llms.txt` URL for your chosen library in your browser to confirm it loads, then ask Copilot to download both variants into `docs/`:

> Download `https://[library-site]/llms.txt` to `docs/llms.txt` and `https://[library-site]/llms-full.txt` to `docs/llms-full.txt`. Create the `docs/` folder if it doesn't exist.

| File | Purpose |
|------|---------|
| `[library-site]/llms.txt` → `docs/llms.txt` | Compact index — component names, import paths, brief descriptions. The agent reads this first on every task. |
| `[library-site]/llms-full.txt` → `docs/llms-full.txt` | Full API documentation. The agent reads this only when it needs prop-level detail beyond what the index covers. |

If the library only publishes one variant, have the agent save whichever exists under both names. To update when a new library version ships, re-run the same prompt — no changes to the agent prompt needed.

### 3. Copy and configure the agent

Copy `frontend.agent.md` from the `assets/` folder into `.github/agents/` in your project. Then ask Copilot to fill in the placeholders from your project:

> Open `.github/agents/frontend.agent.md` and replace `[FRAMEWORK]` and `[LIBRARY]` with the frontend framework and component library this project actually uses — check `package.json` to confirm.

The agent is already configured to read `docs/llms.txt` for component lookups and `docs/llms-full.txt` for deeper detail.

### 4. Test: build a new UI feature

Select the `frontend` agent from the agents dropdown in Chat and give it a concrete UI task:

> Add a data table to the dashboard that shows a list of users (id, name, email, role). The table should support column sorting and a text filter.

Watch the PLAN phase — does the agent read `docs/llms.txt` before writing any code? Note which components it identifies and whether it reaches for `docs/llms-full.txt` for deeper detail.

At the end of the VERIFY phase, the agent should run the build command. If the build passes, the component imports and property names were correct. If it fails, watch whether the agent re-consults the docs or attempts to fix blindly.

### 5. Test: port existing UI (if applicable)

If your project already has a frontend built without a component library, give the agent a porting task:

> Look at all components in `src/` and replace every raw HTML element (forms, buttons, tables, modals) with [LIBRARY] equivalents. Read docs/llms.txt first, then port everything in one pass.

### 6. Wire in the frontend-design skill

**(Optional)** If you installed the `frontend-design` skill in Exercise 6.2, you can give this agent both correctness *and* design standards. Ask Copilot to add the skill to the agent file:

> Add a section to `.github/agents/frontend.agent.md` telling the agent to use the `frontend-design` skill for layout, spacing, and visual hierarchy decisions during BUILD — `llms.txt` decides *which* components to use, the skill decides *how* to style and structure them.

Then run the same UI task from step 4 again. The agent should now consult `llms.txt` for *which* components to use and the skill for *how* to style and structure them. This is the layered pattern: the agent file defines the workflow, `llms.txt` provides component-level accuracy, and the skill encodes design conventions — each concern in its own file, loaded only when needed.

### 7. Reflect on the pattern

- What happens if you remove the `docs/llms.txt` file and run the same task? Does the agent still produce working code?
- The same pattern works for any library with an `llms.txt`. What other libraries in your project could benefit from this approach?
- Committing `llms.txt` to the repo means the whole team's agents share the same reference. Who should be responsible for keeping it up to date when the library releases a new major version?

### Optional: Add a dedicated llms.txt reader sub-agent

The frontend agent currently reads `docs/llms.txt` directly. For a compact index this is fine, but for large full-text dumps it loads the entire file into the main agent's context window on every doc lookup — and every token the main agent reads costs the same as the tokens it uses to write code.

A cheaper pattern: delegate all doc lookups to a small, fast sub-agent running on a cheaper model. The main agent stays focused on building; the reader handles scanning. You'll design the reader and have Copilot build it.

> **Prerequisite:** For the frontend agent to delegate, its `tools` list must include `'agent'` (the alias for the `agent/runSubagent` tool), and `agent/runSubagent` must be enabled in the Chat tools picker. If the agent never delegates in step c below, check both.

**a. Design and create the reader agent.** Decide before you prompt: which model (cheap and fast — e.g. `Claude Haiku 4.5`), which tools it actually needs (`read` and `search` — nothing else), whether it should be `user-invocable` or only callable as a sub-agent, and what it should do when the requested component isn't found. Then ask Copilot (or `/create-agent`) to write it:

> Create `.github/agents/llms-reader.agent.md`: a sub-agent whose only job is to look up components in `docs/llms.txt` and `docs/llms-full.txt` and return, for each requested component, its import path, inputs/outputs, and usage constraints. Use Claude Haiku 4.5 as the model, give it only the `read` and `search` tools, set `user-invocable: false`, and tell it to answer "not found" explicitly instead of guessing.

Skim the result — the prompt should be tight. This agent has exactly one job.

> **Note:** Model names change frequently — if a model isn't in your picker, choose the closest available one.

**b. Wire the frontend agent.** Ask Copilot to update `frontend.agent.md` so it delegates instead of reading the file itself:

> Update `.github/agents/frontend.agent.md`: add `'agent'` to its `tools` list, add `agents: ['llms-reader']` so it can only call that sub-agent, and rewrite the PLAN phase so all doc lookups go to the `llms-reader` sub-agent instead of reading `docs/llms.txt` directly.

**c. Test the delegation.** Run the same UI task from step 4. Watch for the main agent invoking the reader as a sub-agent during PLAN. Does it get the right docs back? Does BUILD use them correctly?

**d. Reflect.**
- What is the main agent's job now, versus the reader's?
- What would happen if you set `disable-model-invocation: true` on the reader?
- This is the same orchestration pattern from [Module 07](../../07-agent-roles-and-orchestration/README.md) — one coordinator, one specialist — applied to a concrete cost problem. Where else in your project could you apply it?

## Expected Outcome

Both `docs/llms.txt` and `docs/llms-full.txt` committed to your project, and a `frontend.agent.md` in `.github/agents/` pointing at them. When you give the agent a UI task it reads the compact index first and falls back to the full file for prop-level detail. The build passes without hallucinated component APIs. To update for a new library version, re-download the files into `docs/` — the agent needs no changes. If you did the optional part, the frontend agent now delegates doc lookups to an `llms-reader` sub-agent and keeps its own context window for building.
