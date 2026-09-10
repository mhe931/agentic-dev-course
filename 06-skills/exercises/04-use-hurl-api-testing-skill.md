# Exercise 6.4: Use the Hurl API Testing Skill

## Objective
Prompt the agent to test API endpoints using the Hurl API testing skill.

## Background
The Hurl API testing skill (`hurl-api-testing`) teaches the agent a new capability: testing HTTP APIs via the `hurl` command-line tool. When the skill is installed, explicitly tell the agent to use it in your prompt — the agent does not always load skills automatically. Once loaded, the agent uses the skill's command vocabulary and `.hurl` file syntax to create and run API tests. You never write the hurl files yourself — you describe what you want tested and the agent builds and executes them.

This is the backend equivalent of [Exercise 6.3](03-use-playwright-cli-skill.md) (Playwright CLI). Where Playwright gives the agent a browser to drive, Hurl gives the agent an HTTP client with built-in assertions, variable capture, and request chaining — purpose-built for API testing.

## Prerequisites
1. Hurl installed: https://hurl.dev/docs/installation.html (`brew install hurl` on macOS).
2. **Start your dev server**: Run `npm run dev` (or equivalent) so your API is accessible at localhost before beginning.

## Steps
1. **Install the skill**: Copy the `hurl-api-testing/` folder from the `assets/` folder into your project's `.github/skills/hurl-api-testing/`.

2. **Test a basic endpoint**: Use the `/hurl-api-testing` slash command to activate the skill, then tell the agent what to test. For example:

   > **/hurl-api-testing** test GET /api/products endpoint at http://localhost:3000 — check that it returns 200 with a JSON array.

   Alternatively, use a natural-language prompt and explicitly mention the skill:

   > *"Use the hurl-api-testing skill to test the GET /api/products endpoint — check that it returns 200 with a JSON array."*

   Watch the agent load the skill, create a `.hurl` file with the request and assertions, and run it with `hurl --test`.

3. **Test a write operation with chaining**: Ask the agent to test a create-read-delete flow. For example:

   > *"Write a hurl test that creates a new product via POST, then fetches it by ID to verify it was saved, then deletes it and confirms the 404."*

   Observe how the agent uses `[Captures]` to grab the created resource's ID and chain it into subsequent requests — all in a single `.hurl` file.

4. **Test error cases**: Ask the agent to test validation and auth. For example:

   > *"Add hurl tests for error cases: missing required fields should return 400, unauthenticated requests should get 401."*

   The agent should create requests with intentionally bad input and assert on the expected error responses.

5. **Reflect on the experience**: The agent chose which hurl syntax to use, structured the `.hurl` files, wrote assertions, and chained requests with captured variables. Compare this to prompting without a skill — what does having a structured command vocabulary in the skill body add?

## Expected Outcome
The agent autonomously creates and runs `.hurl` files that test your API — sending requests, asserting on responses, chaining requests with captured values, and reporting pass/fail results — all from natural-language prompts.

> **Tip:** If a test fails and you want more detail, ask the agent to rerun it verbosely (it will use `hurl --verbose` to show full request/response details). Ask it to parameterize the base URL with `--variable base_url=http://localhost:3000` so the same tests run against other environments, and note that the files it creates are CI-ready as-is: `hurl --test --report-junit report.xml` produces a report your pipeline can consume.
