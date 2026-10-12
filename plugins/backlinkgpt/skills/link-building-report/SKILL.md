---
name: link-building-report
description: Write a weekly (or monthly) link-building report from BacklinkGPT data. Use when the user asks "how is link building going", "weekly report", "what did we win", "did our backlinks help rankings", "reply rate this month", or as the Monday job of a link-building routine. Pulls get_agent_status, get_outreach_kpis, get_source_breakdown, list_backlinks, get_authority_growth, check_backlink_live, deal and reply status, and Search Console and Bing movement for target pages when the user has it, then compares sources and sets next week's plan. Covers what BacklinkGPT's autopilot did and what the agent did, by stage.
---

# Link-building report

A report answers four questions in this order: what did we win, is the pipeline healthy, did any of it move search, and what changes next week. One screen, numbers first, no filler.

Default window: the last 7 days against the 7 before. Monthly: the last 28 days against the 28 before (Search Console works best on 28-day windows).

## 1. Gather

Reading your own data costs no credits.

- `get_agent_status`: the autopilot stages (who did what this week), credits left this period, the prospect pool, links won.
- `get_outreach_kpis` twice, once per window: messages sent, prospects contacted, replies, reply rate, bounces, engagements started, links won. LinkedIn adds invites sent and accepted; quote the accept rate as given.
- `list_backlinks` with `view: "won"` and `sinceDays`: links won in the window; with `view: "won"` alone, every won link for the totals (DR, dofollow share, target page, price where recorded). Without a view it is the whole backlink profile, links the crawler found included.
- `get_authority_growth`: domain rating, referring domains and organic traffic over the window, with the days won links went live.
- `check_backlink_live` on links won in the window and on links not checked in the last 30 days. Report each as live, lost, or unreachable. Unreachable means the page could not be read (blocked); it is not a lost link.
- `get_source_breakdown`: which sourcing channels BacklinkGPT's own hunt kept, contacted and won from.
- `list_inbox` with the `inbox` view: anything still unanswered, with how long it has waited.
- Deals: the threads that moved this week and their stage (negotiating, agreed, invoice received, paid, awaiting placement, live), from the routine's notes and `get_conversation_thread`.
- With a sender connected: `list_engagements` for live sequences, and `get_outreach_history` for what went out.
- Search Console and Bing, from the tools below (or an export the user shares when they aren't connected). Without either, leave the Search section out and say why.
- The Search Console and Bing tools (free). Only the site as a whole compares two windows: `get_search_console_overview` and `get_bing_overview` with `days` set to the window return it and the window before (the default window ends on the last day Search Console has finalized, `finalThrough`, or on Bing's newest stored day, since both run a few days behind; a Search Console site without recent impressions reads zeros, not an old busy period, and when the Bing overview has a `note`, pass it on: Bing has reported nothing for weeks). Pages come as their current numbers only: `list_search_console_pages` with `pageUrl` per target page (the stored last 28 days) and `list_bing_pages`. `list_search_console_queries` and `list_bing_queries` are the site's queries, not split by page: never present them as a target page's queries or a page's trend. The one per-page before and after is `get_link_impact`, for what each won link did to its target page. Quote its `reading` as given: `measured` and `baseline_clipped` (a shorter baseline that won't fill in) are settled, `partial` will still move, and `low_volume`, `not_enough_data`, `no_search_data`, `before_history`, `not_tracked` and `too_early` are not results. Not connected: they say so in `connection.message` or `note`; pass it on.
- The brief's state: which source each prospect came from, credits spent per run.

## 2. Read the numbers honestly

- **Reply rate** is per prospect contacted, counted once however many messages follow. Compare windows of equal length.
- **Search impact** lags. A new link usually shows in Search Console after 2 to 8 weeks, if at all. Report movement on the pages that won links, never claim a link caused it, and name other likely causes (a content update, seasonality, a Google update) when you know them.
- **One week is noisy.** With fewer than 20 prospects contacted, say the sample is small instead of drawing conclusions.
- **Lost links**: a link that disappears within weeks of a paid placement is a deal problem, not an SEO one. Flag it for the user with the thread, and whether payment went out.

## 3. Compare sources

Group the prospects contacted in the last 30 days by their source (`ai_citation`, `competitor_intersection`, `serp`, `resource_search`, `unlinked_mention`, `nofollow_upgrade`, `search_console`, `user`). For each: contacted, replied, won, credits spent. The source with the most replies and wins per credit gets more of next week's budget; a source with 20+ contacts and no replies gets less. Write the new mix into the brief's state.

## 4. Write the report

```markdown
# Link building, <site>: week of <date>

**Won**: <n> links (<n> dofollow), <n> live in total. **Replies**: <n> (<rate>% of <n> contacted, <up/down> from <rate>%). **Credits**: <used>, <left> left.

## Wins
| Page | DR | Target page | Dofollow | Price |
| ... |

## Pipeline
- Contacted: <n> new prospects, <n> follow-ups sent, <n> bounced
- Replies waiting: <n> (oldest <hours> h)
- Deals: <n> negotiating, <n> agreed, <n> awaiting placement, <n> paid and not live

## Search
Site: <clicks> clicks (<vs prev>), <impressions> impressions, average position <position>, <dates> (Search Console); Bing <clicks> clicks (<vs prev>).

| Target page | Clicks, last 28 days | Impressions | Avg position | Links won (all time) | Link impact (reading) |
| ... |
<one sentence on what moved, with the lag caveat when relevant>

## What worked
<the source comparison in two lines>

## Needs you
- <payments to make, institutions or fees to decide, lost paid links, competitors to classify>

## Next week
- <sourcing mix, target pages to push, anything to change in the brief>
```

Keep it under a page. Leave a section out when it has nothing in it. Numbers as they come from the tools, rounded sensibly; no estimates presented as data.

## 5. Deliver

In the chat, or by email to the user through their mail connector (only to the user, never to a third party). In a routine, the report is the run's summary. Update the brief's state with the source mix and the date of the report.
