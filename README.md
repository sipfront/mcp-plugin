# Sipfront Plugin for Claude Code

The official Claude Code plugin for the **[Sipfront MCP server](https://sipfront.com/docs/integrations/mcp-server/)**. It connects Claude Code to your Sipfront account so you can create and run telecom call tests, read results and analyze failed calls from your terminal, and it adds two commands for the most common tasks.

## What it does

- Connects to the hosted MCP server at `https://mcp.sipfront.net/mcp`. You sign in once with a Sipfront API key; no local server, no Docker.
- Gives Claude the Sipfront tools: projects, tests, runs, results, SIP traces, call states, RTP statistics, targets, credential pools, testbooks and a persistent memory for findings.
- Teaches Claude how Sipfront works (load the API docs first, fetch scenario parameters before creating a test, compare failed runs with passing ones, ask before changing anything).

### Commands

| Command | Description |
|---------|-------------|
| `/sipfront:run-test <test>` | Run a test now, wait for it and report the outcome |
| `/sipfront:analyze-failures <project or test>` | Compare failed runs with passing ones and explain the likely cause |

Everything else works in plain language, for example *"create a registration test for the PBX configured in this repo"*.

## Installation

1. Add the Sipfront marketplace and install the plugin:

    ```
    /plugin marketplace add sipfront/mcp-plugin
    /plugin install sipfront@sipfront
    ```

2. Reload plugins so the MCP server starts without restarting Claude Code:

    ```
    /reload-plugins
    ```

3. Authenticate: run `/mcp`, select **sipfront** and choose **Authenticate**. Your browser opens the Sipfront sign-in page; paste the public and secret key of an API key created under [Account › API Keys](https://app.sipfront.com/subscription/apikey) and press **Connect**.

You're ready. Try `/sipfront:analyze-failures` or ask a question.

## Manual setup

Without the plugin, the MCP server can be added directly:

```
claude mcp add --transport http --scope user sipfront https://mcp.sipfront.net/mcp
```

The [Sipfront MCP server documentation](https://sipfront.com/docs/integrations/mcp-server/) covers Claude Code, Claude, ChatGPT, Cursor, Visual Studio Code and other clients.

## Authentication

The MCP server uses OAuth with a browser sign-in in which you enter a Sipfront API key. The plugin never sees the key; it receives an encrypted token that renews itself. Revoking the key in Sipfront disconnects the plugin. Keep the key in your password manager: the secret is shown only once.

## Resources

- [Sipfront MCP server documentation](https://sipfront.com/docs/integrations/mcp-server/)
- [Sipfront documentation](https://sipfront.com/docs/)
- [Report an issue](https://github.com/sipfront/mcp-plugin/issues)

## License

MIT, see [LICENSE](LICENSE).
