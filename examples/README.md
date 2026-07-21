# Example client configs

Drop-in configuration for connecting each client to the hosted Goalie IP trademark MCP server at
`https://www.goalieip.com/api/mcp`.

In every file, replace `gip_live_your_key_here` with a real key from your
[portal](https://www.goalieip.com/portal/api-keys). **Do not commit a real key** — treat it like a
password.

| File | Client |
|---|---|
| [`mcp.json`](mcp.json) | Claude Code, Cursor, and any client using the generic `mcpServers` HTTP config |
| [`claude-api.json`](claude-api.json) | The `mcp_servers` connector block for a Claude API `messages` request |
| [`claude-desktop-config.json`](claude-desktop-config.json) | Claude Desktop via the `mcp-remote` bridge (Windows form; see note) |

For Claude Code you can also skip the file and run:

```bash
claude mcp add --transport http goalieip \
  https://www.goalieip.com/api/mcp \
  --header "Authorization: Bearer gip_live_your_key_here"
```

Full setup and troubleshooting: https://www.goalieip.com/docs#mcp
