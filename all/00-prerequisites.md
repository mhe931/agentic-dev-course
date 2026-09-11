# Module 00: Prerequisites

## Before We Start

This course is not about prescribing the one right way to do things — it's an exploration of the features and what's possible, hopefully inspiring you to think about how to apply these in your real development projects. There might be examples that are not using your preferred stack or domain, but they should enable you to use these tools in your own workflows and with technologies that you are familiar with.

How you eventually choose to configure your workflows and agents will depend entirely on your development environment and the challenges that exist there. To fully enable agentic workflows, we should eventually move from single-person experiments to full team enablement.

At any point, if you feel a topic is more relevant to you, feel free to expand on it. The course can be run through from beginning to end, or you may skip modules. It's worth noting that the course is designed with continuity in mind — if you skip modules you might have to make some adjustments to the exercises.

This is also the time for you to take a break from development deadlines and spend some quality time thinking about *how* you work, rather than *what*.

This module sets up the tooling and the project you will use in every later module; it leads directly into [Module 01](../01-context-and-cost/README.md), where you start shaping how Copilot works in that project.

**Safety and third-party tools:**

> **Warning:** The exercises in this course are designed for educational purposes. Code, configurations, and agent setups produced during the exercises should not be copied blindly into production environments — always adapt them to your organization's standards, security requirements, and review processes. Remember that AI-generated output — whether code, configurations, or documentation — should always be reviewed before use.

> **Warning:** Some exercises reference third-party tools, MCP servers, or skills. Always review the source code and understand what they do before installing them, especially if they require broad permissions or access to sensitive data. Your organization may have policies regarding their use — respect those. Any third-party tools you choose to install during or after the course are used at your own responsibility.

## What You'll Need

> **Prerequisite:** You need an active GitHub Copilot subscription (or a seat on your organization's Copilot Business/Enterprise plan). If your Copilot access is managed by an organization or enterprise, two separate Copilot policies must be enabled for you: **Chat in IDE** and **Agent mode in the IDE**. Agent mode is controlled independently from chat — if the Agent option is missing from the mode picker in Copilot Chat, ask your Copilot administrator to enable it under the organization or enterprise Copilot policies. Nearly every exercise in this course depends on Agent mode.

> **Note:** Hands-on exercises in this course are designed around VS Code and the Copilot extension, but can be adapted to other IDEs or, for example, Claude Code. Just be prepared to figure some things out as you go, since the configurations and certain features can work differently in other tools.

**Core requirements — install these before starting:**

- **VS Code** with the **GitHub Copilot** extension
- **Git**
- **[Node.js](https://nodejs.org/)** (v20+) and **npm** — the course app is a React + TypeScript frontend with a Node.js backend, so this is required from Module 00 onwards. Node is also needed for the MCP servers and CLI tools (namely Playwright CLI) in Modules 05–06; to install Playwright CLI, follow the instructions at [Playwright CLI GitHub](https://github.com/microsoft/playwright-cli).

**Nice to have:**

- **[uv](https://docs.astral.sh/uv/)** — a fast Python package and project manager. Some MCP servers are distributed as Python packages run with `uvx`. To install, follow the [uv installation guide](https://docs.astral.sh/uv/getting-started/installation/) and verify with `uv --version`.

## Sandbox App

You need a codebase to practice Copilot effectively, and everyone on the course uses the same one: the **Micro Journal** app — quick daily entries with mood tracking. You scaffold it with Copilot in [Exercise 0.1](exercises/01-project-setup.md) from the brief in [`assets/micro-journal.md`](assets/micro-journal.md).

The stack is fixed:

- **Frontend:** React + TypeScript (Vite)
- **Backend:** Node.js (Express + TypeScript)
- **Database:** SQLite

A shared app and a shared stack mean the exercises, file paths, and example prompts in later modules line up with what's actually on your screen — and that you can compare results with the person next to you. Later modules assume this app exists, so don't substitute your own project.

Open the app in its own VS Code workspace, separate from this course repo, so the instruction files, prompt files, and agents you create in later modules apply to your app only.

> **Note:** Copilot generates the app, so no two scaffolds will be identical — file layouts and dependency choices will vary. That's expected; the exercises only rely on the app having a React + TypeScript frontend, a Node.js API, and a dev script that runs both.

## Relevant Documentation

- [Setting up GitHub Copilot in VS Code](https://code.visualstudio.com/docs/setup/copilot) — Installing the extension and signing in
- [Managing policies and features for Copilot in your organization](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies) — Where the "Chat in IDE" and related policies live
- [Enterprise management of Copilot agents](https://docs.github.com/en/copilot/concepts/agents/enterprise-management) — The separate "Agent mode in the IDE" policy

## Exercises

| Exercise | Description |
|----------|-------------|
| [01-project-setup](exercises/01-project-setup.md) | Prepare a project to use throughout this course |

