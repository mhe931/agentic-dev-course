# Exercise 9.1: Design Your Agent Pipeline

> **Tip:** [Download this exercise as DOCX](01-design-your-pipeline.docx) to fill in with your team.

## Objective

Map your team's development workflow onto the agent patterns you've learned throughout this course. The result is a pipeline blueprint you can take back and start building.

## Background

Throughout this course you've built up a toolkit: custom instructions, prompt files, skills, MCP servers, specialist agents, and orchestrators. Now it's time to decide which of these patterns fit **your** actual workflow — and where the biggest wins are.

Not every stage needs an agent. The goal is to identify where automation adds real value and where human judgment stays in the loop.

## Steps

Work through each pipeline stage below. For each one, discuss with your team and fill in the template. Skip stages that don't apply to your workflow, and add stages that are missing.

### Pipeline Stages

#### 1. Backlog & Requirements

*How does work enter your pipeline? Where do requirements live?*

| Question | Your Answer |
|----------|-------------|
| Where do tasks/tickets come from? (e.g., Jira, Linear, GitHub Issues) | |
| What information does a good ticket contain? | |
| What information does the agent need to triage, refine, or enrich tickets? | |
| What MCP server or integration would be needed? | |
| **Agent/skill idea** | |

---

#### 2. Planning & Specification

*How do you go from a ticket to an implementation plan?*

| Question | Your Answer |
|----------|-------------|
| Who creates the technical plan today? | |
| Should planning happen in Ask mode / Plan mode or via a dedicated planner agent? | |
| What context does the planner need? (e.g., architecture docs, API specs, existing code) | |
| **Agent/skill idea** | |

---

#### 3. Implementation

*How does code get written?*

| Question | Your Answer |
|----------|-------------|
| What coding standards or conventions must be followed? | |
| What documentation should the agent be grounded in? (e.g., SDK docs via `llms.txt`) | |
| Should implementation be split across specialist agents? (e.g., frontend, backend, database) | |
| What guardrails are needed? (e.g., no direct DB access, no force-push) | |
| **Agent/skill idea** | |

---

#### 4. Testing & Verification

*How do you verify that the implementation is correct?*

| Question | Your Answer |
|----------|-------------|
| What types of tests does your project use? (unit, integration, e2e, visual) | |
| Could a verification agent run tests and report results without editing code? | |
| Do you need browser-based testing? (e.g., the Playwright CLI skill, VS Code's built-in browser tools) | |
| What does "done" look like for a test pass? | |
| **Agent/skill idea** | |

---

#### 5. Review & Quality

*How does code get reviewed before merging?*

| Question | Your Answer |
|----------|-------------|
| What does your current review process look like? | |
| What are the most common review findings? (e.g., missing tests, style issues, security) | |
| Could an agent do a first-pass review before human review? | |
| Could the review agent also run static analysis tools to improve review quality? | |
| **Agent/skill idea** | |

---

#### 6. Deployment & Operations

*How does code go from merged to running in production?*

| Question | Your Answer |
|----------|-------------|
| What does your CI/CD pipeline look like today? | |
| Are there manual steps in deployment that could be automated? | |
| Could an agent help with environment setup, config generation, or IaC? | |
| What about post-deployment? (smoke tests, health checks, monitoring) | |
| Could an agent help triage incidents or alerts? | |
| **Agent/skill idea** | |

---

### Putting It Together

Now sketch your end-to-end pipeline. For each stage you identified above, note:

| Stage | Agent / Skill / Prompt File | Key Tools & MCP Servers | Guardrails | Human Checkpoint? |
|-------|----------------------------|-------------------------|------------|-------------------|
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

#### Discussion Questions

1. **Quick wins:** Which 1-2 stages would give you the most value with the least effort?
2. **Orchestration:** Does your pipeline need a central orchestrator, or can stages run independently?
3. **Human-in-the-loop:** Where must a human approve before the pipeline continues?
4. **Risks:** What could go wrong if an agent misbehaves at each stage? What guardrails prevent that?
5. **Iteration:** What's your plan for evolving the pipeline as you learn what works?
6. **Ownership:** Who will take ownership of maintaining and improving the agents over time?

### Optional: Let the Agent Draft the First Building Blocks

**(Optional)** Paste your filled-in tables (the stage tables and the "Putting It Together" summary) into Copilot Chat in Agent mode and ask the agent to turn your top two stages into concrete designs. For example:

```text
Here is our team's agent pipeline blueprint (tables below). For the two stages we
marked as quick wins, propose concrete designs: a `.agent.md` custom agent (frontmatter
plus prompt body), any supporting `.prompt.md` prompt files, and any hooks needed to
enforce the guardrails we listed. Explain which tools and MCP servers each one needs
and where the human checkpoint sits.
```

Glance at the proposals and ask follow-up questions — for example, "Tighten the guardrails so this agent can never run destructive git commands" — rather than editing the drafts by hand. These become the starting point for what you build first back at work.

## Expected Outcome

Your team has a concrete pipeline blueprint with:
- Identified stages that map to your real workflow
- Agent/skill ideas for the highest-value stages
- Clear human checkpoints and guardrails
- A prioritized list of what to build first
- (Optional) Agent-drafted `.agent.md`, prompt-file, and hook designs for your top two stages
