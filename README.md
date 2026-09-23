# Sipfront Plugin for Claude Code

The official Claude Code plugin for the **[Sipfront MCP server](https://sipfront.com/docs/integrations/mcp-server/)**. It connects Claude Code to your Sipfront account so you can create and run voice and voice AI tests, read results and analyze failed calls from your terminal, and it adds two commands for the most common tasks.

## What it does

- Connects to the hosted MCP server at `https://mcp.sipfront.net/mcp`. You sign in once with a Sipfront API key; no local server, no Docker.
- Gives Claude the Sipfront tools: projects, tests, runs, results, transcripts, conversation turns, SIP traces, call states, audio and RTP statistics, targets, credential pools, testbooks and a persistent memory for findings.
- Teaches Claude how Sipfront works (load the API docs first, fetch scenario parameters before creating a test, compare failed runs with passing ones, ask before changing anything).

### Commands

| Command | Description |
|---------|-------------|
| `/sipfront:run-test <test>` | Run a test now, wait for it and report the outcome |
| `/sipfront:analyze-failures <project or test>` | Compare failed runs with passing ones and explain the likely cause |

`<test>` and `<project or test>` are the name or numeric ID of a test or project as shown in the Sipfront app. Names may be partial; if several match, Claude lists the candidates and asks. Add `in <project>` to narrow a test name to one project, and a time range such as `last 7 days` to `analyze-failures` (default: last 24 hours). Without an argument, `analyze-failures` looks at all projects.

```
/sipfront:run-test booking-agent smoke
/sipfront:run-test 1234
/sipfront:analyze-failures Support Bot last 7 days
```

### Examples

Everything works in plain language as well. Some things you can ask:

- *"Call our voice agent on +1 555 0100, book a table for two tomorrow at 7 pm and check that it confirms the reservation."*
- *"Create a test that talks to the voice bot in this repo and fails if any response takes longer than two seconds."*
- *"Run the support-bot regression suite and tell me where the bot misunderstood the caller."*
- *"Which of our voice bot tests had the slowest time to first audio this week?"*
- *"Why did the appointment-booking test fail last night? Was it the bot or the SIP trunk?"*
- *"Set up a call quality test against the SIP endpoint configured in this repo and fail it if the MOS drops below 4."*

Claude picks the matching Sipfront scenario, fills in the required parameters, defines pass/fail conditions such as expected intents, transcript content, turn count or response latency, and reads the transcript, SIP trace and audio metrics of a run to explain what happened.

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
