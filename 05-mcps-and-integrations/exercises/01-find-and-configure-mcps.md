# Exercise 5.1: Find and Configure MCP Servers

## Objective
Learn where to discover MCP servers and how to configure one at the project level — and measure for yourself why every extra tool costs context, which is why project-level config is preferred over global.

## Background

See the [Module 05 README](../README.md) for key concepts on MCP servers, config locations, and why project-level config is preferred. [MCP_REFERENCE.md](../MCP_REFERENCE.md) lists common MCP servers by category.

## Prerequisites

> **Prerequisite:** MCP servers require the **"MCP servers in Copilot"** policy — enabled by your organization or enterprise admin under Copilot policies on GitHub. If your enterprise enforces an MCP allowlist or a private MCP registry, Context7 may be blocked; ask your admin or pick an approved server from the registry instead.

- **Node.js** installed locally — Context7 is launched via `npx`.

## Steps

1. **Count your tools before adding anything.** Open Copilot Chat in **Agent mode** and click the **tools** icon in the chat input to open the tools picker. Note how many tools are currently enabled (the picker shows a count). Then ask the agent:

   > "How many tools do you have available right now, and roughly how many tokens do their descriptions consume in your context?"

   Keep the answer — you will compare it after adding a server.

2. **Install a server from the MCP Registry.** This is the easiest way to add a project-level MCP server:
   1. Open the **Extensions** panel (`Ctrl+Shift+X` (Windows/Linux) / `Cmd+Shift+X` (macOS)).
   2. Type `@mcp` in the search bar to filter for MCP servers from the registry.
   3. Search for **Context7** (documentation lookup).
   4. Click **Install** and select **Install in Workspace** when prompted. This writes the config to `.vscode/mcp.json` automatically — no manual JSON editing required. You can leave the API key blank.

   > **Note:** The GitHub MCP Registry integration in VS Code is in public preview and may change. No setting needs to be enabled — this is disclosure only.

3. **Verify the config file.** Glance at `.vscode/mcp.json` and confirm the server entry was added. You should see a **Start** or **Stop** button above the server.

4. **Count again — this is the context bloat.** Reopen the tools picker and note the new total, then ask the agent the same question as in step 1 and compare. The agent's token figure is an estimate, not a measurement — the tool count from the picker is the hard number; the token answer just gives you a feel for the scale. Context7 only adds a couple of tools, but a server like the GitHub MCP server adds dozens — and every tool description is sent with every request, whether or not the tool gets used. Now imagine that server configured globally: those tokens would be spent in every project, including ones that never need it. That is why project-level config is the default recommendation, and why it pays to uncheck tools you don't need in the picker.

5. **Test it.** Ask a question that should trigger the server's tools. For Context7: *"Using Context7, look up the latest docs for [a library in your project]."* Confirm the Context7 tools appear in the tool invocation output.

   > **Warning:** MCP servers like Context7 fetch external content and inject it directly into the agent's context — even a well-maintained server (Context7 is maintained by Upstash) cannot vet every page it returns. Malicious docs could contain prompt injection: instructions embedded in content that try to hijack the agent's behavior. Review what the agent fetched before acting on its output.

6. **Look at what the registry wrote.** Glance at `.vscode/mcp.json` again — the registry install produced an entry with a `command` and `args` (and possibly `env` for the API key). That is all an MCP server config is, which means any server can be configured the same way, whether or not the registry lists it.

   **(Optional)** To add a server the registry doesn't list, ask the agent rather than editing the file by hand:

   > "Add the `<server-name>` MCP server to `.vscode/mcp.json`, launched with `npx -y <package>`."

   The resulting entry follows the same shape as the registry-generated one:

   ```json
   {
     "servers": {
       "<server-name>": {
         "command": "npx",
         "args": ["-y", "<package>"]
       }
     }
   }
   ```

## Expected Outcome
A working project-level MCP server that you can invoke from Agent mode, plus a before/after tool count that shows concretely what every added tool costs — and therefore why global config is the wrong default and project-level config is the recommendation.
