---
name: backlinkgpt-setup
description: Set up BacklinkGPT so an agent can run link building for a site. Use when the user says "set me up", "connect BacklinkGPT", "start link building for my site", "get BacklinkGPT working with Claude/ChatGPT/Codex/Hermes", or when get_workspace_status shows missing pieces (no target pages, no Search Console, no routine). Connects the MCP server, Search Console and Bing, defines target pages, competitors and exclusions, writes the site profile, deal profile and outreach brief, picks a model, and schedules the recurring routine.
---

# BacklinkGPT setup

BacklinkGPT is the rails, you are the agent. It stores prospects, contacts, messages and deals, runs metered SEO data tools, syncs Search Console and Bing, sends on a schedule with safety rails (when scheduled sending is on), detects replies and reports events. It runs no AI. You source, judge, write, reply and negotiate, on a schedule the user owns.

Labels used in all BacklinkGPT skills: **(credits)** is a metered data tool that spends data credits, **(sending)** needs scheduled sending (`plan.scheduledSending` in `get_workspace_status`). A workspace without data credits refuses the data tools, and one without scheduled sending refuses the sending tools: then you take your own tools or the manual path (below). Everything else is free: memory, contact policies, Search Console and Bing, `check_draft`, logging, events.

**Without data credits** the workflow stays the same, only the source of the numbers changes. Read pages with your own web fetch, search with your own web search, and take backlinks, link gaps, traffic and domain rating from an SEO data source the user already has (an Ahrefs or Semrush MCP server, open-seo, an export). Save everything into BacklinkGPT memory as usual. Never invent a number you could not read: an unmeasured site is scored as unmeasured.

Work through the steps in order. Skip a step that `get_workspace_status` shows is already done. Ask the user only what you cannot read yourself, and confirm before writing anything they did not dictate.

## 1. Connect

Call `get_workspace_status`. If the BacklinkGPT tools are missing, connect first: see `references/connect-clients.md` for Claude, ChatGPT, Claude Code, Codex, Hermes and headless agents.

- MCP URL: `https://backlinkgpt.com/api/mcp`. OAuth by default. One connection is bound to one workspace, and one workspace is one site.
- Headless agents (scheduled jobs, Hermes, CI) use an API key from https://backlinkgpt.com/app/api/keys as `Authorization: Bearer <key>` or `x-api-key`. Owners and admins create keys. Store the key in the runtime's secret store, never in a prompt, file in a repo, or chat.
- No account yet: the user signs up at https://backlinkgpt.com and creates the workspace for their site in the app. You do not create accounts for them.

Read the status back to the user in two lines: whether scheduled sending is on, data credits left (or none), senders, Search Console and Bing connected or not, send window.

## 2. Search Console and Bing

The user connects both in the app at https://backlinkgpt.com/app/connected-accounts. Both are free and both matter: they show which pages are close to ranking (where links help most) and later whether won links moved anything.

Verify with `get_search_console_overview` and `get_bing_overview`. If either is empty, say so and continue; the routine works without them, the report gets thinner.

## 3. Site profile

Read the homepage and two or three main pages. Use your own web fetch if you have one (free), else `read_page` (credits, 1 per page).

Write the profile with `update_site_positioning`:

- **What it is**: one or two sentences, first principles, what it does and for whom. Name the category precisely ("time tracker for freelancers", not "productivity software").
- **What it is not**: two to six broad categories a lazy reader would file it under. This is the exclusion filter that keeps outreach off the wrong pages.
- **Audience**: who buys or uses it, ten words or fewer.
- **Value props**: up to five, short, factual.

Detect the real content language and market from the copy, TLD, currency and imprint. Do not default to English or the US. Show the profile to the user and fix what they correct. Copy the final profile into the brief (step 8), so every run has it.

Also judge linkability honestly and tell the user: **strong** (guides, data, tools, a real blog), **workable** (credible product pages, thin editorial: list additions and resource pages still work), **weak** (placeholder or thin pages nobody would cite). For a weak site, name the one asset worth publishing first.

## 4. Target pages

The pages links should point to. Call `list_target_pages`, then choose one to four:

- Young or small site: the homepage first.
- Commercial landing or category pages, and genuinely citeable resources (a study, a tool, a definitive guide).
- Not legal pages, login or app pages, tag or archive pages, or ordinary blog posts.
- One target per language when the site serves several. A German page gets a German target.
- With Search Console: pages with queries in positions 5 to 20 and real impressions gain the most from links (`list_search_console_pages`, `list_search_console_queries`).

Promote crawled pages with `set_target_page`; add a new URL with `add_target_page`.

## 5. Competitors

Call `get_competitor_policy`, then classify with `set_competitor_role`:

- `no_contact` (off-limits): direct challengers, the same product for the same buyer. Never contacted on any channel.
- `seed_only` (ask-first): bigger, older or adjacent vendors in the space. Their pages may be pitched, but they trade links rather than sell them, and you never offer them money.
- `fair_game`: listed by mistake or not a competitor after all.

Only vendors are competitors. Blogs, magazines, agencies and communities are not. Every listed competitor's backlinks can be mined whatever its role. When unsure between off-limits and ask-first, pick ask-first and say so. `update_competitor` fixes names and domains; `delete_competitor` removes an entry.

## 6. Exclusions

Ask the user once: "Which sites should we never contact?" Typical answers: clients, partners, investors, their own other sites, publishers they had a bad experience with. Add each with `add_domain_exclusion`. Show the built-in list with `list_exclusions`; `set_global_exclusion` opts one entry in or out.

## 7. Deal profile

`update_deal_profile` holds the company and billing facts you may share when a publisher asks how to invoice: legal name, address, VAT id, billing email. Ask the user for them. Never invent one. If they skip it, you will tell publishers the details follow, and ask the user then.

## 8. The brief

BacklinkGPT stores data, not your judgment rules. Write a short brief the routine reads at the start of every run: money rules, voice, facts you may claim, caps, credit budget, what needs the user. Use `references/brief-template.md`. Ask for:

- The most they would pay for one link: $0, about $75, about $150, or "decide each one at the fair price". Plus a cap on money paid before a link is live.
- Who signs the emails, and their usual sign-off exactly as they write it.
- Three to five emails they wrote themselves (voice samples) and the first-hand facts you may state. Never invent a claim, client, number or credential.
- New conversations per day, and a credit budget per run when the workspace has data credits.
- What always needs their yes (default: any money commitment, anything to a university, hospital, government body or charity, and anything outside the brief).

Store it where the routine will find it: a file in the project for Claude Code or Codex, the project instructions or task prompt for ChatGPT, the agent's memory or skills folder for Hermes.

## 9. Sending

- **With scheduled sending**: check `list_senders`. Senders (Gmail, Outlook or IMAP inboxes, and LinkedIn accounts) are connected in the app at https://backlinkgpt.com/app/connected-accounts. Read `get_cadence_policy` and set the send window and daily caps with `update_cadence_policy`. A new or rarely used mailbox ramps up: see `set_sender_warmup`.
- **Without scheduled sending**: BacklinkGPT does not send. The routine drafts, the user (or your mail connector, with their yes) sends from their own mailbox, and you log each message with `record_manual_touch`.

## 10. Events

Pick how the routine learns what happened:

- **Polling** (works everywhere): `list_events` with the cursor saved from the last run. Keep the cursor in the brief or the routine's state file.
- **Webhooks** (needs a public HTTPS endpoint): `create_event_subscription` with the URL and event types, for example `engagement.reply_received` to wake a reply check. Verify the signatures. `list_event_subscriptions` and `delete_event_subscription` manage them.

Replies decide links. Answers that slipped from same day to two days cost real links in our own campaigns, so the reply check runs at least twice per working day, or on the webhook.

## 11. Model

Recommend the strongest model the user's client offers, with extended thinking on where available (for example the current Claude Opus or Sonnet, or the GPT model with thinking in ChatGPT). Judging a page, writing a message that does not read as a template, and negotiating a price are reasoning work. Small fast models produce generic pitches that do not get answers.

## 12. The routine

Set up the schedule in whatever the user already runs. Details and prompt templates: `references/routines.md`.

| Job          | When                                       | What it does                                                               |
| ------------ | ------------------------------------------ | -------------------------------------------------------------------------- |
| Daily run    | Working days, morning in the user's zone   | Replies first, then source, judge, write, queue within caps, short summary |
| Reply check  | Two or three times per working day, or on the reply webhook | `handle-link-replies` only                                 |
| Weekly report | Monday morning                            | `link-building-report`                                                     |

Schedulers: Claude Code routines (`/schedule`) or `/loop` in an open session; scheduled tasks in the Claude app; ChatGPT scheduled tasks; Hermes cron; any machine with cron running `claude -p` or `codex exec` with an API key. When `list_skills` shows `outreach-routine` available, `get_skill("outreach-routine")` returns the full routine prompt.

## 13. First run with the user watching

Before anything runs unattended:

1. Run the daily routine once, live: find about ten pages, judge them, write pitches for the best three to five.
2. Show the drafts. Do not queue or send yet. Fix the voice with the user's edits and add each correction to the brief.
3. When the user approves a batch, queue it, and turn the schedule on.
4. For the first week, the daily summary lists every message queued so the user can stop anything that reads wrong.

## Done when

- [ ] `get_workspace_status` shows the right site, scheduled sending, data credits and senders as expected
- [ ] Search Console and Bing connected, or the user chose not to
- [ ] Site profile written and confirmed
- [ ] One to four target pages, one per language
- [ ] Competitors classified, exclusions added
- [ ] Deal profile filled, or the user knows it is empty
- [ ] Brief written and stored where the routine reads it
- [ ] Sender and caps set (with scheduled sending), or the manual send-and-log path explained (without it)
- [ ] Events: cursor saved or webhook subscribed
- [ ] Daily run, reply checks and weekly report scheduled
- [ ] First batch reviewed by the user
