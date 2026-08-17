# Trademark MCP Server (USPTO)

[![MCP Badge](https://lobehub.com/badge/mcp-full/goalieip-trademark-mcp?theme=light)](https://lobehub.com/mcp/goalieip-trademark-mcp)

A remote **[Model Context Protocol](https://modelcontextprotocol.io)** server that lets an AI agent
**search and filter 14M+ USPTO trademark records** — by mark text, owner, goods/services, class,
status, and date — then pull any full record by serial number.

Point Claude Code, the Claude API, Cursor, or any MCP client at one URL and your agent can query the
US federal trademark register with **no integration code to write**.

> **Get a key and full docs:** **[goalieip.com/mcp](https://www.goalieip.com/mcp)** ·
> setup for every client at **[goalieip.com/docs#mcp](https://www.goalieip.com/docs#mcp)**

---

## Not another TSDR wrapper

Most "trademark MCP" servers wrap the USPTO's **TSDR** endpoint, which returns a single record only
when you *already know* its serial or registration number. That's a lookup tool. This server
**searches and filters the whole register**, so your agent can find the marks it doesn't already
have the numbers for.

| Capability | TSDR-based MCP | Goalie IP MCP |
|---|:---:|:---:|
| Retrieve a record by serial or registration number | ✅ | ✅ |
| Search by mark text — exact, contains, or fuzzy | ❌ | ✅ |
| Filter by owner, goods/services, and class | ❌ | ✅ |
| Filter by status and filing / registration date | ❌ | ✅ |
| Surface similar marks for clearance-style questions | ❌ | ✅ |
| Search across all 14M+ US federal records at once | ❌ | ✅ |

See [`examples/prompts.md`](examples/prompts.md) for two real sessions — including a ten-name
clearance screen that a lookup-only tool cannot begin, because no serial numbers are known going in.

---

## Quickstart (Claude Code)

Create an API key in your [Goalie IP portal](https://www.goalieip.com/portal/api-keys), then:

```bash
claude mcp add --transport http goalieip \
  https://www.goalieip.com/api/mcp \
  --header "Authorization: Bearer gip_live_your_key_here"
```

Prefer not to paste a key? Omit the `--header` and Claude Code opens a browser to sign in with your
Goalie IP account over OAuth.

Then just ask — the agent picks the tool and filters on its own:

```
> Are there any live trademarks similar to "GOALIE" for software in class 9?

  Searching US federal trademarks…
  Found 94 matches. 12 are live in class 9, including:
    GOALIEO   — Goalieo Inc      — Live/Pending  — filed 2026-05-29
    …
```

---

## The two tools

| Tool | What it does |
|---|---|
| `search_trademarks` | Search 14M+ US federal records by mark text (exact, contains, or fuzzy), owner, goods/services, serial or registration number, international class, status code, and filing or registration date ranges. Returns compact summaries with a total match count. |
| `get_trademark` | Retrieve the complete record for one serial number — full goods/services text, owner details, status history, classifications, and prior registrations. |

`search_trademarks` returns compact summaries (page size defaults to 10, capped at 15) sized to sit
in a model's context window; call `get_trademark` for the full record. Every search must include at
least one narrowing filter (`markLiteral`, `ownerName`, `serialNumber`, `registrationNumber`,
`goodsAndServices`, or another text filter) — class, status, and date filters alone match too much
of the register to run. When a query is rejected or times out, the tool replies with the specific
parameter to change, so the agent can correct itself and retry.

---

## In the MCP Registry

This server is published to the official
[MCP Registry](https://registry.modelcontextprotocol.io) as **`com.goalieip/trademark`**, under a
namespace verified against the `goalieip.com` domain — so the listing is first-party, not a
third-party mirror.

```bash
curl "https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark"
```

Clients that read the registry can add the server by name and will prompt for authorization
themselves — an OAuth sign-in, or an API key. Clients that don't, use the manual configs below.

---

## Connect from any client

Two ways to authenticate, both on the same endpoint:

- **OAuth** — easiest for apps with a *Connect* / *Add connector* screen (Claude Desktop, claude.ai).
  Paste the endpoint URL and sign in with your Goalie IP account when prompted; the client registers
  itself automatically (Dynamic Client Registration + PKCE) and needs no key.
- **API key** — for CLI and programmatic clients. Send your key as an `Authorization: Bearer` header.
  Create one in your [portal](https://www.goalieip.com/portal/api-keys).

Drop-in config files for the API-key path are in [`examples/`](examples/); replace
`gip_live_your_key_here` with a real key.

### Claude API (MCP connector)

```json
{
  "mcp_servers": [
    {
      "type": "url",
      "url": "https://www.goalieip.com/api/mcp",
      "name": "goalieip",
      "authorization_token": "gip_live_your_key_here"
    }
  ]
}
```

### Any MCP client / Cursor (generic config)

```json
{
  "mcpServers": {
    "goalieip": {
      "type": "http",
      "url": "https://www.goalieip.com/api/mcp",
      "headers": {
        "Authorization": "Bearer gip_live_your_key_here"
      }
    }
  }
}
```

### Claude Desktop & claude.ai

Use OAuth — no bridge, no key file. Open **Settings → Connectors → Add custom connector** and paste
the endpoint URL:

```
https://www.goalieip.com/api/mcp
```

Claude registers itself automatically (PKCE), sends you to sign in with your Goalie IP account, and
stores the token. Approve the `trademark.read` scope and you're connected.

*No-OAuth fallback (older clients):* attach an API key through the
[`mcp-remote`](https://github.com/geelen/mcp-remote) bridge — config in
[`examples/claude-desktop-config.json`](examples/claude-desktop-config.json), full steps (including
the "fully quit before editing" gotcha) at [goalieip.com/docs#mcp](https://www.goalieip.com/docs#mcp).

---

## Server details

| | |
|---|---|
| **Registry name** | `com.goalieip/trademark` ([MCP Registry](https://registry.modelcontextprotocol.io)) |
| **Endpoint** | `https://www.goalieip.com/api/mcp` |
| **Transport** | Streamable HTTP (remote — nothing to install or self-host) |
| **Protocol revisions** | `2026-07-28` native; `2025-11-25` also answered on the same URL |
| **Authentication** | OAuth 2.1 (PKCE + Dynamic Client Registration, scope `trademark.read`), or a Bearer API key identical to the REST API — same endpoint |
| **Coverage** | US federal (USPTO) applications and registrations, refreshed daily |

Always configure the `www` host. `goalieip.com` 301-redirects to `www`, and many clients drop the
`Authorization` header across the redirect (→ a `401`). More troubleshooting:
[goalieip.com/docs#mcp](https://www.goalieip.com/docs#mcp).

---

## Pricing

MCP access is **included with every Goalie IP API plan** at no extra cost — it uses the same key and
the same monthly quota as the REST API. The **free tier is 200 calls/month** (no credit card).
Compare plans at [goalieip.com/subscribe#api](https://www.goalieip.com/subscribe#api).

One note on sizing: agents typically make several tool calls to answer one question, so MCP consumes
quota faster than a scripted one-request-per-lookup integration. Budget for that when picking a plan.

---

## Scope & safety

- **US federal data only.** This is a continuously-refreshed copy of the USPTO trademark register,
  updated daily and occasionally a day behind. It does **not** cover US state registrations,
  unregistered common-law use, or trademark offices outside the United States. Goalie IP is **not
  affiliated with or endorsed by the USPTO**.
- **Record text is third-party input.** Mark text, owner names, and goods/services descriptions are
  written by whoever filed the application, and every US filing becomes a public record. Tool
  responses fence that content and label it as data — but if you build your own agent on this data,
  apply your own checks before letting record text drive tool calls or privileged actions.
- **Results are data, not legal advice.** These tools return records from the USPTO register. They do
  not assess registrability, likelihood of confusion, or infringement, and using them creates no
  attorney-client relationship. For an attorney-led clearance opinion or enforcement work,
  [talk to the Goalie IP team](https://www.goalieip.com/contact).

---

## Links

- **Example prompts & real results:** [`examples/prompts.md`](examples/prompts.md)
- **Overview:** https://www.goalieip.com/mcp
- **MCP Registry listing:** https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark
- **Full docs:** https://www.goalieip.com/docs#mcp
- **Get an API key:** https://www.goalieip.com/portal/api-keys
- **Plans & pricing:** https://www.goalieip.com/subscribe#api

## License

[MIT](LICENSE) © Goalie IP Inc. This repository documents how to connect to the hosted Goalie IP
trademark MCP server; the server and the underlying data are operated by Goalie IP.
