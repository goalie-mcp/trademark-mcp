# Goalie IP Trademark MCP (USPTO)

[![MCP Badge](https://lobehub.com/badge/mcp-full/goalieip-trademark-mcp?theme=light)](https://lobehub.com/mcp/goalieip-trademark-mcp)

Give Claude, Cursor, or any MCP client one endpoint for searching the U.S. federal trademark
register, screening a proposed name, retrieving full records, and checking upcoming deadlines.

Goalie IP searches its own daily-refreshed copy of 14M+ USPTO applications and registrations. The
hosted server is ready to use: no package to install, integration code to write, or database to run.

- **Screen a proposed name in one call.** Find exact matches, spelling variants, sound-alikes
  (`KWIK` vs. `QUICK`), and the USPTO's own pseudo-mark readings (`EZ` as `EASY`).
- **Search the register, not just known serial numbers.** Filter by mark, owner, goods/services,
  class, status, dates, attorney, and more.
- **Get deadline answers from a purpose-built tool.** Check one serial number or upcoming dates
  across an owner's marks.
- **Let agents work without babysitting.** All five tools are declared read-only.

> **Connect:** `https://www.goalieip.com/api/mcp`<br>
> **[Start free](https://www.goalieip.com/signup)** · **[Setup guide](https://www.goalieip.com/docs#mcp)** · **[Example prompts](examples/prompts.md)**

---

## More than a TSDR lookup

TSDR returns a record when you already know its serial or registration number. Goalie IP lets an
agent begin with the question the user actually has: *Is this name taken? What has this owner filed?
Which marks cover these goods? What is due next?*

| Capability | TSDR-based MCP | Goalie IP MCP |
|---|:---:|:---:|
| Retrieve a known record | ✅ | ✅ |
| Search and filter the full U.S. federal register | ❌ | ✅ |
| Screen a proposed name for similar marks and sound-alikes | ❌ | ✅ |
| Search by owner, goods/services, class, status, and dates | ❌ | ✅ |
| Compute upcoming U.S. federal trademark deadlines | ❌ | ✅ |

---

## Connect in a minute

### Claude Code

```bash
claude mcp add --transport http goalieip https://www.goalieip.com/api/mcp
```

Claude opens a browser for OAuth sign-in. No API key is required.

### Claude Desktop and claude.ai

Open **Settings → Connectors → Add custom connector**, then paste:

```
https://www.goalieip.com/api/mcp
```

For Cursor, scripts, CI, and clients without OAuth, create an API key in the
[Goalie IP portal](https://www.goalieip.com/portal/api-keys). Drop-in configurations are in
[`examples/`](examples/), with complete setup and troubleshooting at
[goalieip.com/docs#mcp](https://www.goalieip.com/docs#mcp).

Always use the `www` host. Some clients drop authorization headers when following the redirect from
`goalieip.com` to `www.goalieip.com`.

---

## Five read-only tools

| Tool | What it does |
|---|---|
| `find_similar_marks` | Screen a proposed name for exact matches, spelling variants, sound-alikes, and USPTO pseudo-mark readings, ranked by match strength and related classes. |
| `search_trademarks` | Search and filter the U.S. federal register by mark, owner, goods/services, class, status, dates, attorney, and more. Its `fuzzy` mode is a typo-tolerant spelling lookup, not a sound-alike search. |
| `get_trademark` | Retrieve the full record for one USPTO serial number. |
| `get_deadlines` | Compute U.S. federal trademark deadlines for one serial number or upcoming deadlines across an owner's marks. Dates should be confirmed with the USPTO. |
| `get_account` | Check the connected account, plan, authentication method, usage, and reset date. This tool is never billed. |

All five tools are annotated `readOnlyHint: true`. The four trademark-data tools are metered;
`get_account` is not. For complete descriptions and input schemas, see the
[live server card](https://www.goalieip.com/.well-known/mcp/server-card.json).

`find_similar_marks` is a screening aid, not a clearance search or legal opinion. It returns strong
matches—more when the field is crowded—with dead marks listed separately. For register research by
owner, goods, dates, or other fields, use `search_trademarks`.

---

## Try asking

```text
Is LILAH taken for coffee in class 30?

Find every U.S. federal trademark owned by Acme Example Corp.

Show live class 9 marks whose goods mention password management.

What is the next deadline for serial number [USPTO serial number]?

Which deadlines are coming up across Example Company's marks this year?
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
| **Coverage** | U.S. federal trademark applications and registrations |
| **Data source** | Goalie IP's daily-refreshed copy of the USPTO register |

The server is published under a `goalieip.com`-verified namespace in the
[official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark).

---

## Pricing

MCP access is included with every Goalie IP API plan and uses the same monthly allowance. The free
tier includes 200 calls per month with no credit card. [Compare plans](https://www.goalieip.com/subscribe#api).

---

## Coverage and trust

- **U.S. federal records only.** The tools cover USPTO applications and registrations. They do not
  search state registrations, unregistered common-law use, foreign trademark offices, the web,
  marketplaces, social media, or domain names.
- **Goalie IP's copy of the register.** Data is refreshed daily and can occasionally be a day behind.
  This is not a live connection to USPTO systems. Goalie IP is not affiliated with or endorsed by
  the USPTO.
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
- [Official MCP Registry listing](https://registry.modelcontextprotocol.io/v0/servers?search=com.goalieip/trademark)
- [API key portal](https://www.goalieip.com/portal/api-keys)
- [Support](mailto:reid@goalieip.com)

## License

[MIT](LICENSE) © Goalie IP Inc. This repository documents how to connect to the hosted Goalie IP
trademark MCP server; the server and underlying data are operated by Goalie IP.
