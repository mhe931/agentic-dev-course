# Exercise 4.1: Built-in /setupTests Command

> **Optional:** Skip this exercise if your project already has a test setup.

## Objective
Use Copilot's built-in `/setupTests` command to scaffold a test configuration for your project and understand what built-in commands give you out of the box.

## Background
Ideally, testing is part of spec-driven development — when you write a spec (Module 03), you're already defining how the feature should behave, and those behaviors translate directly into test cases. A good spec forces you to think about edge cases, expected outputs, and failure modes before any code is written. If you followed SDD and included acceptance criteria in your spec, you may already have tests or clear test scenarios. In that case, skip this exercise.

If your project doesn't have a test setup yet, this command gets you a baseline. The `/setupTests` command analyzes your project and generates a test configuration — framework setup, config files, and sample tests — following a fixed workflow. Understanding the scope and limits of built-in commands helps you decide when a custom prompt file is worth the effort.

## Steps

1. **Run the command.** Open Copilot Chat and type `/setupTests`. Copilot will analyze your project structure and propose a test setup (framework, config file, sample test).

2. **Review what it generated.** Glance at the key files — config, sample tests — and check whether they look reasonable for your project. If something's off, tell the agent what to change.

3. **Note the limits.** Built-in commands like `/setupTests` can ask clarifying questions (e.g., which framework to use), but they follow a fixed workflow — you can't customize the steps, enforce team conventions, or shape the output format.

4. **Reflect.** This is the baseline. The custom commands you'll build in the next exercises (code review, update instructions) go beyond what fixed-workflow commands can do — multi-step workflows, structured output, and domain-specific logic.

## Expected Outcome
A working test setup scaffolded by `/setupTests`. You understand what built-in commands handle well and where custom prompt files add value.
