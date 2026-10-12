---
name: backlinkgpt-setup
description: Set up BacklinkGPT so an agent can run link building for a site. Use when the user says "set me up", "connect BacklinkGPT", "start link building for my site", "get BacklinkGPT working with Claude/ChatGPT/Codex/Hermes", or when get_agent_status shows missing pieces (no target pages, no sender, no routine). Connects the MCP server, settles which autopilot stages the agent takes over (get_autopilot_stages), connects Search Console and Bing, defines target pages, competitors and exclusions, writes the site profile, deal profile and outreach brief, picks a model, and schedules the recurring routine.
---

# BacklinkGPT setup

BacklinkGPT runs link building in four autopilot stages, each on by default (`get_autopilot_stages`): **find sites** (it hunts prospect pages every night), **judge pages** (it reads and judges each new page), **write outreach** (it picks a contact, starts the outreach and drafts every message) and **suggest replies** (it drafts an answer to each reply). A stage that is off is yours: BacklinkGPT stops doing it and you do it over MCP. All four off is full MCP mode. Whatever the stages, BacklinkGPT keeps the memory (prospects, contacts, messages, deals), sends with safety rails, detects replies, checks pages and won links, and reports events.

Every BacklinkGPT skill does only the stages that are off, or what the user asked for by name. Never switch a stage yourself without the user's word (`set_autopilot_stages`).

Labels used in all BacklinkGPT skills: **(credits)** is a metered tool that spends the workspace's credits, **(sending)** needs a connected sender (`list_senders`: a Gmail, Outlook or IMAP inbox, or a LinkedIn account, connected in the app). A workspace without credits left refuses the credit tools, and one without a working sender refuses the sending tools: then you take your own tools or the manual path (below). Everything else is free: memory, contact policies, `check_draft`, logging, events. Every tool needs the workspace to have an active plan; without one each call is refused and nothing is charged. A tool these skills name that is not in your tool list: use your own equivalent and say so in the summary.

**Without credits** the workflow stays the same, only the source of the numbers changes. Read pages with your own web fetch, search with your own web search, and take backlinks, link gaps, traffic and domain rating from an SEO data source the user already has (an Ahrefs or Semrush MCP server, open-seo, an export). Save everything into BacklinkGPT memory as usual. Never invent a number you could not read: an unmeasured site is scored as unmeasured.

Work through the steps in order. Skip a step that `get_agent_status` shows is already done. Ask the user only what you cannot read yourself, and confirm before writing anything they did not dictate.

## 1. Connect

Call `get_agent_status`. If the BacklinkGPT tools are missing, connect first: see `references/connect-clients.md` for Claude, ChatGPT, Claude Code, Codex, Hermes and headless agents.

- MCP URL: `https://backlinkgpt.com/api/mcp`. OAuth by default. One connection is bound to one workspace, and one workspace is one site.
- Headless agents (scheduled jobs, Hermes, CI) use an API key from https://backlinkgpt.com/app/api/keys as `Authorization: Bearer <key>` or `x-api-key`. Owners and admins create keys. Store the key in the runtime's secret store, never in a prompt, file in a repo, or chat.
- No account yet, or no browser on your side: follow https://backlinkgpt.com/auth.md. With the user's OK you register with their email and get a link and a 6-digit code; the user opens the link, signs in or creates the account, enters the code and picks the workspace (or creates it by typing the site's domain), and you receive a token for the MCP server. That token lasts 60 minutes with no refresh (exchange the identity assertion for a new one) and the assertion 30 days (then the user claims again), so a scheduled routine switches to an API key once the workspace is set up. Finishing the workspace's setup and connecting an inbox stay with the user, in the app; a new workspace answers the tools only once its setup is finished.

Read the status back to the user in two lines: which autopilot stages are on (`stages`), credits left, senders and their state (`list_senders`), send window.

## 2. Who does what

Show the user the four stages and what each means for them (`get_autopilot_stages` explains each). Ask which ones you take over. Common setups:

- **All on**: BacklinkGPT runs the loop; you supervise, answer what it escalates, and report.
- **Write outreach and suggest replies off**: BacklinkGPT finds and judges pages; you write every message and answer every reply.
- **All off** (full MCP mode): you run the whole loop with these skills.

Switch the stages the user names with `set_autopilot_stages`, and write the split into the brief (step 9). With write outreach off, every live engagement becomes yours (`followUpMode: "agent"`); drafts already waiting in review stay for the user to approve or reject.

## 3. Search Console and Bing

The user connects both in the app at https://backlinkgpt.com/app/connected-accounts. Both are free and both matter: they show which pages are close to ranking (where links help most) and later whether won links moved anything. BacklinkGPT reads them itself. When your tool list has Search Console or Bing tools, use them in sourcing and in the report; otherwise ask the user for an export when you need the numbers.

The app picks the property that matches the site on its own. When the connected account holds several (a domain property and a URL-prefix one, or a www and a non-www site), `set_search_property` picks one; its refusal lists the ones it can read (`availableSites`). Switching to a different property deletes the history stored for the old one, so ask the user first, and never switch an established property just to tidy up.

## 4. Site profile

`refresh_site_positioning` reads the homepage and writes BacklinkGPT's own profile; or read the homepage and two or three main pages yourself (your own web fetch, or `read_page`, credits, 1 per page) and write it with `update_site_positioning`:

- **What it is**: one or two sentences, first principles, what it does and for whom. Name the category precisely ("time tracker for freelancers", not "productivity software").
- **What it is not**: two to six broad categories a lazy reader would file it under. This is the exclusion filter that keeps outreach off the wrong pages.
- **Audience**: who buys or uses it, ten words or fewer.
- **Value props**: up to five, short, factual.

Detect the real content language and market from the copy, TLD, currency and imprint. Do not default to English or the US. Show the profile to the user and fix what they correct. Copy the final profile into the brief (step 9), so every run has it.

Also judge linkability honestly and tell the user: **strong** (guides, data, tools, a real blog), **workable** (credible product pages, thin editorial: list additions and resource pages still work), **weak** (placeholder or thin pages nobody would cite). For a weak site, name the one asset worth publishing first.

## 5. Target pages

The pages links should point to. Call `list_target_pages`, then choose one to four:

- Young or small site: the homepage first.
- Commercial landing or category pages, and genuinely citeable resources (a study, a tool, a definitive guide).
- Not legal pages, login or app pages, tag or archive pages, or ordinary blog posts.
- One target per language when the site serves several. A German page gets a German target.
- With Search Console data: pages with queries in positions 5 to 20 and real impressions gain the most from links.

Promote crawled pages with `set_target_page`; add a new URL with `add_target_page`.

## 6. Competitors

Call `get_competitor_policy`, then classify with `set_competitor_role`:

- `no_contact` (off-limits): direct challengers, the same product for the same buyer. Never contacted on any channel.
- `seed_only` (ask-first): bigger, older or adjacent vendors in the space. Their pages may be pitched, but they trade links rather than sell them, and you never offer them money.
- `fair_game`: listed by mistake or not a competitor after all.

Only vendors are competitors. Blogs, magazines, agencies and communities are not. Every listed competitor's backlinks can be mined whatever its role. When unsure between off-limits and ask-first, pick ask-first and say so. `update_competitor` fixes names and domains; `delete_competitor` removes an entry.

## 7. Exclusions

Ask the user once: "Which sites should we never contact?" Typical answers: clients, partners, investors, their own other sites, publishers they had a bad experience with. Add each with `add_domain_exclusion`. Show the built-in list with `list_exclusions`; `set_global_exclusion` opts one entry in or out.

## 8. Deal profile

`update_deal_profile` holds the company and billing facts you may share when a publisher asks how to invoice: legal name, address, VAT id, billing email. Ask the user for them. Never invent one. If they skip it, you will tell publishers the details follow, and ask the user then.

## 9. The brief

BacklinkGPT stores data, not your judgment rules. Write a short brief the routine reads at the start of every run: which stages are yours, money rules, voice, facts you may claim, caps, credit budget, what needs the user. Use `references/brief-template.md`. Ask for:

- The most they would pay for one link: $0, about $75, about $150, or "decide each one at the fair price". Plus a cap on money paid before a link is live.
- Who signs the emails, and their usual sign-off exactly as they write it.
- Three to five emails they wrote themselves (voice samples) and the first-hand facts you may state. Never invent a claim, client, number or credential.
- New conversations per day, and a credit budget per run.
- What always needs their yes (default: any money commitment, anything to a university, hospital, government body or charity, and anything outside the brief).

Store it where the routine will find it: a file in the project for Claude Code or Codex, the project instructions or task prompt for ChatGPT, the agent's memory or skills folder for Hermes.

## 10. Sending

Check `list_senders`. Senders (Gmail, Outlook or IMAP inboxes, and LinkedIn accounts) are connected by the user in the app at https://backlinkgpt.com/app/connected-accounts.

- **With a sender connected**: read `get_cadence_policy` and set the send window and daily caps with `update_cadence_policy`. A new or rarely used mailbox ramps up: see `set_sender_warmup`. With several senders, `update_sender` makes one the default for its channel. It also sets the name prospects see (`displayName`) and, when the user sends from an alias such as hello@ their domain, the send-as address: the first call sends a real test email from the alias to itself, and a second call about 10 seconds later saves it once the test arrived as the alias. A From address on another domain restarts the email warm-up, so say so before switching.
- **No sender connected**: BacklinkGPT cannot send. The routine drafts, the user (or your mail connector, with their yes) sends from their own mailbox, and you log each message with `record_manual_touch`. Connecting a sender later switches the routine to queueing.

## 11. Events

Pick how the routine learns what happened:

- **Polling** (works everywhere): `list_events` with the cursor saved from the last run. Keep the cursor in the brief or the routine's state file.
- **Webhooks** (needs a public HTTPS endpoint): `create_event_subscription` with the URL and event types, for example `engagement.reply_received` to wake a reply check. Verify the signatures. `list_event_subscriptions`, `update_event_subscription` (change event types or filters, pause or resume, `send_test` to check your receiver, `redeliver_failed` after your endpoint was down) and `delete_event_subscription` manage them.

Replies decide links. Answers that slipped from same day to two days cost real links in our own campaigns, so the reply check runs at least twice per working day, or on the webhook.

## 12. Model

Recommend the strongest model the user's client offers, with extended thinking on where available (for example the current Claude Opus or Sonnet, or the GPT model with thinking in ChatGPT). Judging a page, writing a message that does not read as a template, and negotiating a price are reasoning work. Small fast models produce generic pitches that do not get answers.

## 13. The routine

Set up the schedule in whatever the user already runs. Details and prompt templates: `references/routines.md`.

| Job          | When                                       | What it does                                                               |
| ------------ | ------------------------------------------ | -------------------------------------------------------------------------- |
| Daily run    | Working days, morning in the user's zone   | Replies first, then the stages that are yours (source, judge, write), queue within caps, short summary |
| Reply check  | Two or three times per working day, or on the reply webhook | `handle-link-replies` only                                 |
| Weekly report | Monday morning                            | `link-building-report`                                                     |

Schedulers: Claude Code routines (`/schedule`) or `/loop` in an open session; scheduled tasks in the Claude app; ChatGPT scheduled tasks; Hermes cron; any machine with cron running `claude -p` or `codex exec` with an API key. When `list_skills` shows `outreach-routine` available, `get_skill("outreach-routine")` returns the full routine prompt.

## 14. First run with the user watching

Before anything runs unattended:

1. Run the daily routine once, live, for the stages that are yours: for example find about ten pages, judge them, write pitches for the best three to five.
2. Show the drafts. Do not queue or send yet. Fix the voice with the user's edits and add each correction to the brief.
3. When the user approves a batch, queue it, and turn the schedule on.
4. For the first week, the daily summary lists every message queued so the user can stop anything that reads wrong.

## Done when

- [ ] `get_agent_status` shows the right site, credits and stages, and `list_senders` the expected senders
- [ ] The stages the agent takes over are switched off, and the brief says which they are
- [ ] Search Console and Bing connected, or the user chose not to
- [ ] Site profile written and confirmed
- [ ] One to four target pages, one per language
- [ ] Competitors classified, exclusions added
- [ ] Deal profile filled, or the user knows it is empty
- [ ] Brief written and stored where the routine reads it
- [ ] Sender and caps set (with a sender connected), or the manual send-and-log path explained (without one)
- [ ] Events: cursor saved or webhook subscribed
- [ ] Daily run, reply checks and weekly report scheduled
- [ ] First batch reviewed by the user
