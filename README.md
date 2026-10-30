# Goalie IP Trademark MCP Server (USPTO + Canada)

[![MCP Badge](https://lobehub.com/badge/mcp-full/goalieip-trademark-mcp?theme=light)](https://lobehub.com/mcp/goalieip-trademark-mcp)

A hosted trademark MCP server for Claude, ChatGPT, Cursor, or any MCP client. One endpoint lets an
agent search the U.S. and Canadian federal trademark registers, screen a proposed name, retrieve
full records, check upcoming deadlines, and find acceptable goods and services wording.

Goalie IP searches its own copies of the registers: a daily-refreshed copy of 14M+ USPTO
applications and registrations, and a weekly-refreshed copy of 2.2M+ Canadian (CIPO) filings. The
server is ready to use: no package to install, integration code to write, or database to run.

- **Screen a proposed name in one call.** Find exact matches, spelling variants, sound-alikes
  (`KWIK` vs. `QUICK`), and the USPTO's own pseudo-mark readings (`EZ` as `EASY`), in the U.S. or
  Canada.
- **Search the register, not just known serial numbers.** Filter by mark, owner, goods/services,
  class, status, dates, attorney, and more.
- **Get deadline answers from a purpose-built tool.** Check one serial number or upcoming dates
  across an owner's marks or an attorney's docket.
- **Draft the goods and services.** Look up pre-approved wording and classes in the USPTO ID Manual.
- **Let agents work without babysitting.** All seven tools are declared read-only.

> **Connect:** `https://www.goalieip.com/api/mcp`<br>
> **[Start free](https://www.goalieip.com/signup)** · **[Setup guide](https://www.goalieip.com/docs#mcp)** · **[Example prompts](examples/prompts.md)**

---

## More than a TSDR lookup

TSDR returns a record when you already know its serial or registration number. Goalie IP lets an
agent begin with the question the user actually has: *Is this name taken? What has this owner filed?
Which marks cover these goods? What is due next? How should I describe my goods?*

| Capability | TSDR-based MCP | Goalie IP MCP |
|---|:---:|:---:|
| Retrieve a known record | ✅ | ✅ |
| Search and filter the full U.S. federal register | ❌ | ✅ |
| Screen a proposed name for similar marks and sound-alikes | ❌ | ✅ |
| Search by owner, goods/services, class, status, and dates | ❌ | ✅ |
| Compute upcoming U.S. federal trademark deadlines | ❌ | ✅ |
| Search Canada's federal (CIPO) register | ❌ | ✅ |
| Look up acceptable goods/services wording (USPTO ID Manual) | ❌ | ✅ |

---

## Connect in a minute

### Claude and ChatGPT directories

Goalie IP is listed in the [Claude Connectors Directory](https://claude.ai/directory/goalie-trademark-search)
and the [ChatGPT plugins directory](https://chatgpt.com/plugins) (search for **Goalie**). Install it
there and sign in with your Goalie IP account.

### Claude Code

```bash
claude mcp add --transport http goalieip https://www.goalieip.com/api/mcp
```

Claude opens a browser for OAuth sign-in. No API key is required.

### Claude Desktop and claude.ai (custom connector)

Open **Settings → Connectors → Add custom connector**, then paste:

```
https://www.goalieip.com/api/mcp
```

### Cursor, scripts, and CI

For clients without OAuth, create an API key in the
[Goalie IP portal](https://www.goalieip.com/portal/api-keys). Drop-in configurations are in
[`examples/`](examples/), with complete setup and troubleshooting at
[goalieip.com/docs#mcp](https://www.goalieip.com/docs#mcp).

Always use the `www` host. Some clients drop authorization headers when following the redirect from
`goalieip.com` to `www.goalieip.com`.

---

## Seven read-only tools

| Tool | What it does |
|---|---|
| `find_similar_marks` | Screen a proposed name for exact matches, spelling variants, sound-alikes, and USPTO pseudo-mark readings, ranked by match strength and related classes. Set `register: "CA"` to screen Canada. |
| `search_trademarks` | Search and filter the U.S. federal register by mark, owner, goods/services, class, status, dates, attorney, and more, or Canada's with `register: "CA"`. Its `fuzzy` mode is a typo-tolerant spelling lookup, not a sound-alike search. |
| `get_trademark` | Retrieve the full record for one USPTO serial number or CIPO application number. |
| `get_deadlines` | Compute U.S. federal trademark deadlines for one serial number, or upcoming deadlines across an owner's marks or an attorney's docket. Dates should be confirmed with the USPTO. |
| `search_id_manual` | Search the USPTO Trademark ID Manual (~72,000 active entries) for pre-approved goods and services wording and its class. |
| `get_data_status` | Report how current and complete the data is: latest file loaded, missing days, verification results, and known limitations. Never billed. |
| `get_account` | Check the connected account, plan, authentication method, usage, and reset date. Never billed. |

All seven tools are annotated `readOnlyHint: true`. The five data tools are metered;
`get_data_status` and `get_account` are not. For complete descriptions and input schemas, see the
[live server card](https://www.goalieip.com/.well-known/mcp/server-card.json).

`find_similar_marks` is a screening aid, not a clearance search or legal opinion. It returns strong
matches—more when the field is crowded—with dead marks listed separately. For register research by
owner, goods, dates, or other fields, use `search_trademarks`.

---

## Try asking

```text
Is LILAH taken for coffee in class 30?

Is LILAH taken for coffee in Canada?

Find every U.S. federal trademark owned by Acme Example Corp.

Show live class 9 marks whose goods mention password management.

What is the next deadline for serial number [USPTO serial number]?

Which deadlines are coming up across Example Company's marks this year?

What class are scented candles in, and how should I word them?
```

See [`examples/prompts.md`](examples/prompts.md) for longer examples and the shape of the results.

---

## Server details

| | |
|---|---|
| **Endpoint** | `https://www.goalieip.com/api/mcp` |
| **Official MCP Registry name** | `com.goalieip/trademark` |
| **Transport** | Streamable HTTP |
| **Authentication** | OAuth 2.1 or Bearer API key |
| **Coverage** | U.S. (USPTO) and Canadian (CIPO) federal trademark applications and registrations; USPTO ID Manual |
| **Data source** | Goalie IP's own copies of the registers: USPTO refreshed daily, CIPO refreshed weekly |
| **Data health** | [goalieip.com/data-status](https://www.goalieip.com/data-status) |

The server is published under a `goalieip.com`-verified namespace in the
[official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark).

---

## Pricing

MCP access is included with every Goalie IP API plan and uses the same monthly allowance. The free
tier includes 200 calls per month with no credit card. [Compare plans](https://www.goalieip.com/trademark-api#pricing).

---

## Coverage and trust

- **U.S. and Canadian federal records only.** The tools cover USPTO and CIPO applications and
  registrations, one register per call. They do not search state or provincial registrations,
  unregistered common-law use, other trademark offices, the web, marketplaces, social media, or
  domain names. Deadlines and the ID Manual are U.S. only.
- **Goalie IP's copy of the registers.** U.S. data is refreshed daily and Canadian data weekly, so a
  very recent filing may not appear yet. This is not a live connection to USPTO or CIPO systems.
  Goalie IP is not affiliated with or endorsed by the USPTO or CIPO.
- **Canadian attribution.** Canadian results carry CIPO's attribution statement: *Contains data
  licensed by Her Majesty the Queen in right of Canada, as represented by the Minister of Industry,
  the minister responsible for the administration of the Canadian Intellectual Property Office.*
- **Data, not legal advice.** Results do not decide registrability, likelihood of confusion, or
  infringement, and using the tools does not create an attorney-client relationship.
- **Public-record text is untrusted input.** Mark text, owner names, and goods/services descriptions
  come from public filings. Tool responses fence and label record content, but builders should apply
  their own checks before allowing it to drive privileged actions.

For an attorney-led opinion or enforcement work, [contact the Goalie IP team](https://www.goalieip.com/contact).

---

## Privacy

The server records one usage row per metered call—the credential identifier, tool, status code, and
timestamp—for allowance enforcement, billing, and abuse detection. **Query contents are not stored,**
and conversation prompts and files do not reach the server. OAuth tokens are stored only as hashes
and can be revoked from the account portal.

Read the full [Privacy Policy](https://www.goalieip.com/legal/privacy).

---

## Links

- [Product overview](https://www.goalieip.com/mcp)
- [Setup and troubleshooting](https://www.goalieip.com/docs#mcp)
- [Example prompts](examples/prompts.md)
- [Live tool descriptions and schemas](https://www.goalieip.com/.well-known/mcp/server-card.json)
- [Data status](https://www.goalieip.com/data-status)
- [Claude Connectors Directory listing](https://claude.ai/directory/goalie-trademark-search)
- [Official MCP Registry listing](https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark)
- [API key portal](https://www.goalieip.com/portal/api-keys)
- [Support](mailto:reid@goalieip.com)

## License

[MIT](LICENSE) © Goalie IP Inc. This repository documents how to connect to the hosted Goalie IP
trademark MCP server; the server and underlying data are operated by Goalie IP.
