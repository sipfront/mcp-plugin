# Sipfront plugin for Claude Code

Plugin spec: <https://code.claude.com/docs/en/plugins>. Keep the number of
commands strictly low: two commands (`run-test`, `analyze-failures`) plus one
background skill (`sipfront-testing`) that applies when the user talks about
Sipfront, voice testing or voice AI testing. New capabilities go into the
background skill or plain-language instructions, not into new commands, unless
a task is both frequent and needs an argument.

The MCP tools are defined in the `mcp-server` repository
(`src/mcp_server_sipfront/server.py`); skill instructions must only name tools
that exist there.

## Testing

Load the plugin from disk without installing:

```bash
claude --plugin-dir /path/to/mcp-plugin
```

Then verify:
- `/help` lists `/sipfront:run-test` and `/sipfront:analyze-failures`
- `/mcp` lists the `sipfront` server; authenticate and run a read-only tool

Use `/reload-plugins` to pick up changes without restarting the session.

Full install path (marketplace resolution, copy to `~/.claude/plugins/cache/`):

```
/plugin marketplace add sipfront/mcp-plugin@<branch>
/plugin install sipfront@sipfront
```
