# Example prompts and real results

Two unedited-in-substance sessions against the live server, run from **Claude Desktop** in July 2026.
They show the thing that separates a search tool from a lookup tool: in neither case did the user know
a serial or registration number going in.

Transcripts are abridged only to remove repeated client UI noise (the "Used goalieip integration"
lines that Claude Desktop prints on each tool call); elisions are marked `[…]`. Editorial corrections
are in `[square brackets]`. No results were changed.

> **Results drift.** The USPTO register updates daily, so the specific marks, statuses, and dates
> below are a snapshot as of **July 2026** and will not match what you get today. They are here to
> show the shape of an answer, not as current data.
>
> **Not legal advice.** These are register search results. They do not assess registrability or
> likelihood of confusion. See [Scope & safety](../README.md#scope--safety).

---

## Sample 1 — Naming shortlist screened against the live register

**Model:** Opus 4.8 · **Tool calls:** ~10 · **Why it matters:** ten candidate marks, zero known serial
numbers. A TSDR-style lookup tool cannot start this task at all.

### Prompt

```
I want to pursue a trademark for a software business. I want it to have a dog theme.
"Smart Labs" or "Pointer development" Something like that. come up with 10 possibilities.
Use the goalie mcp to check for similar marks. Create a table with the proposed marks and
the 2 or 3 closest live trademarks for each proposed mark.
```

### Response

> I'll research this properly. Let me load the tools I need and start searching trademarks.
>
> Quick note before I dig in: trademark clearance ultimately depends on the specific goods/services
> class, and a real conflict analysis (likelihood of confusion) is a legal judgment. I'm going to
> surface the closest live marks so you can eyeball them, but this isn't a legal opinion — you'll
> want a proper clearance search before filing.
>
> My 10 candidates, then searching each in the software classes (Class 009 software / Class 042 SaaS).
>
> […]
>
> "Pointer" is crowded (POINTER TECHNOLOGIES is live-registered in Class 042 for software services).
> Continuing with the rest.
>
> […]
>
> Kelpie is notably clear. Two more: Setter and Wolfhound.
>
> […]
>
> Here are 10 dog-themed candidates, each searched against the live USPTO register in the two classes
> a software business usually files in — Class 009 (downloadable software) and Class 042 (SaaS). The
> "closest live marks" column shows live registrations/applications in those classes only.

| # | Proposed mark | Closest LIVE marks in Cls 009/042 (owner — class — status) | Read on availability |
|---|---|---|---|
| 1 | Smart Labs | SMART LABOR — John Bean Technologies — 007/009/041/042 — Registered · SMART LABOR — The Estimating Edge — 009 — Registered (the exact "SMART LABS" filings are all dead/abandoned) | Weak/descriptive; exact term is dead but "Smart ___" is heavily crowded |
| 2 | Pointer | POINTER TECHNOLOGIES — Pointer Technologies Inc. — 042 — Registered · POINTER — Pointer Group Inc. — 009 — Pending · POINTERPRO — Objective7 — 042 — Registered | Crowded / high risk |
| 3 | Fetch | FETCH — Fetch CRM LLC — 042 — Registered (SaaS) · FETCH — Fetch Rewards LLC — 009/035/036 — Registered · FETCH — Caber Group LLC — 009 — Registered | Very crowded / high risk |
| 4 | Retriever | RETRIEVER — Lever Technology — 042 — Registered (SaaS) · CLASS RETRIEVER — Smart Data Consultants — 009 — Notice of Allowance · PROFIT RETRIEVER — Sweetfish Ltd — 009/035/042 — Registered | Crowded / risky |
| 5 | Bloodhound | BLOODHOUND — Apps Associates — 042 — Registered (SaaS) · BLOODHOUND ENTERPRISE — SpecterOps — 009/042 — Registered · BLOODHOUND — ChampionX — 009 — Registered | Crowded, esp. security software / high risk |
| 6 | Beagle | BEAGLE — Discover Beagle Inc. — 042 — Pending (legal SaaS) · BEAGLEY — Beagleboard.org — 009 — Registered · STANDARD BEAGLE — Standard Beagle Studio — 035/042 — Pending | Moderate–high risk |
| 7 | Husky | HUSKY — Home Depot Product Authority — 009 — Registered (multiple) · HUSKY COMPLETE — Husky Injection Molding — 007/009/037/042 — Registered · POLICYHUSKY — DirectDrive Tech — 036/042 — Pending | Crowded, strong senior owners / high risk |
| 8 | Kelpie | KELPIE — Novosound Ltd — 009/010/042 — Registered (ultrasonic imaging) (the only other, a Cls 009 "application software" filing, is dead) | Relatively open — only 1 live mark, niche goods |
| 9 | Wolfhound | WOLFHOUND — Bit Kid Inc. — 009 — Pending (video-game software) (all other Wolfhound filings are dead) | Fairly open — one live mark, in games |
| 10 | Setter | (No live marks — the exact word "SETTER" in Cls 009/042 is held by 3 filings, all dead/cancelled) | Most open of the set for the exact word |

> The cleaner-looking names here are Setter, Kelpie, and Wolfhound. Fetch, Pointer, Retriever,
> Bloodhound, and Husky are genuinely crowded with live software marks and I'd treat them as
> high-risk. "Smart Labs" is also weak for a separate reason — it's fairly descriptive/laudatory,
> which makes it both hard to clear and hard to protect.
>
> Caveats I want to be honest about: this covers only the US federal register in Classes 009 and 042.
> It does not cover other relevant classes, state registrations, common-law (unregistered) use, or
> domain/social-handle availability. I searched literal word matches, so phonetic equivalents and
> design marks may be missed. And "closest" here is my judgment from the mark text plus class —
> actual likelihood-of-confusion is a legal analysis that weighs the specific goods/services,
> channels, and mark strength. Before filing on any of these, get a full clearance search and a
> trademark attorney's opinion.

**What the agent did:** roughly ten `search_trademarks` calls — each candidate word filtered to
classes 009 and 042 and to live statuses — then composed the results into a comparison. Note that the
model volunteered its own scope limits without being asked; the tool returns records, and the model
is responsible for not overclaiming from them.

---

## Sample 2 — Owner portfolio, then full records for deadline math

**Model:** Sonnet 5 · **Tool calls:** 6 · **Why it matters:** demonstrates the two-tool pattern —
`search_trademarks` by owner to find the portfolio, then `get_trademark` on each serial number for the
status history that drives the answer.

### Prompt

```
use the goalie mcp to find all trademarks owned by Ross Brandborg. it should be 5 to 10.
then determine the next maintenance deadline for each.
```

### Response

> Found all 5 trademarks owned by Ross Brandborg, and pulled full USPTO records for each to check
> maintenance status. Here's what matters most first: JACOBITE's Section 8 declaration is due
> 2026-08-18 — about 4 weeks from now — and the last data update (2026-06-20) shows it has not yet
> been filed. Worth verifying that immediately on USPTO TSDR since [Goalie IP] data may lag
> real-time filings.
>
> Full picture, based on standard USPTO post-registration maintenance rules (15 U.S.C. §§1058–1059: a
> Section 8 Declaration of Use is due between the 5th and 6th anniversary of registration, with a
> 6-month grace period at added cost; a combined Section 8 & 9 renewal is due between the 9th and
> 10th anniversary, same grace period):
>
> **FULL REVOLT** — Reg. No. 7121102, Class 025 (shirts), registered 2023-07-25. No Section 8 filed
> yet. Next deadline: Section 8 declaration, window opens 2028-07-25, due by 2029-07-25 (grace period
> to 2030-01-25).
>
> **CLANDESTINE** — Reg. No. 6708316, Class 005 (dietary supplements), registered 2022-04-19. No
> Section 8 filed yet. Next deadline: Section 8 declaration, window opens 2027-04-19, due by
> 2028-04-19 (grace period to 2028-10-19).
>
> **JACOBITE** — Reg. No. 6129460, Class 025 (shirts), registered 2020-08-18. A courtesy reminder for
> the 6-year Section 8 was emailed 2025-08-18; as of the record's last update, no ES8R/acceptance
> entry appears, meaning it looks unfiled. Next deadline: Section 8 declaration due 2026-08-18 (grace
> period to 2027-02-18 with surcharge). This is the urgent one.
>
> **FIRECASTLE** — Reg. No. 5595476, Class 045 (legal services), registered 2018-10-30. Section 8
> (6-year) was already filed and accepted (2024-01-18). Next deadline: combined Section 8 & 9
> renewal, window opens 2027-10-30, due by 2028-10-30 (grace period to 2029-04-30).
>
> **CLANDESTINE (second one)** — Serial 90375359, Class 025, never registered — abandoned 2021-11-03
> for failure to respond to an office action. No maintenance obligation; it's dead.
>
> I am not a substitute for pulling live TSDR status, especially for JACOBITE given the tight window —
> that one's worth confirming today rather than relying solely on this snapshot.

**What the agent did:** one `search_trademarks` call filtered by `ownerName`, then a `get_trademark`
call per serial number. The deadline calculations come from the model applying statutory rules to the
registration dates and prosecution history in those full records — the tools supply the facts, not
the arithmetic.

---

## What these illustrate

- **Search, not lookup.** Both sessions began with no serial or registration number. Sample 1 could
  not be started at all with a TSDR-wrapper tool.
- **Two tools, two jobs.** `search_trademarks` narrows; `get_trademark` supplies the full record when
  the answer depends on status history or goods/services text.
- **Budget for multiple calls.** Sample 1 used about ten tool calls for one question. MCP usage draws
  on the same quota as the REST API, so agentic sessions consume it faster than scripted lookups —
  see [Pricing](../README.md#pricing).
- **Narrow your filters.** Every search needs at least one text filter (`markLiteral`, `ownerName`,
  `goodsAndServices`, or similar). Class, status, and date filters alone match too much of the
  register and are rejected — the error names the parameter to change.
