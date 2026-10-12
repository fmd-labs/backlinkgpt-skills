# BacklinkGPT skills

Official [BacklinkGPT](https://backlinkgpt.com) plugin and skills for AI agents: Claude, ChatGPT, Claude Code, Codex, Cursor, Hermes, or anything else that speaks MCP or REST. One install connects your agent to the BacklinkGPT MCP server and gives it six skills that teach it link building end to end.

BacklinkGPT runs link building in four autopilot stages: **find sites**, **judge pages**, **write outreach** and **suggest replies**. Each is on by default. Switch any of them off (`set_autopilot_stages`, or the Playbook in the app) and your agent does that part with these skills; switch all four off and your agent runs the whole loop. Whatever the split, BacklinkGPT is the rails underneath:

- **Memory.** Every prospect, verdict, contact, message and deal, so nothing is pitched twice.
- **SEO data tools.** Domain rating and traffic, competitor link intersection, SERP reads, page reads, email finding and verification. Metered in credits. Without credits your agent brings its own SEO data, and the skills tell it how.
- **Search Console and Bing.** Connected in the app, they steer BacklinkGPT's own target pages and sourcing, and your agent reads the synced data too: query and page data for your site, and the movement on pages that won links (`get_search_console_overview`, `list_search_console_queries`, `list_search_console_pages`, `get_link_impact`, `get_bing_overview`, `list_bing_queries`, `list_bing_pages`). Read-only and free.
- **Sending with safety rails.** Your agent queues a first message and its follow-up steps; BacklinkGPT sends each one as written from your Gmail, Outlook or IMAP inbox inside your send window, with caps, pacing, suppression lists and dead-page checks.
- **Draft checks.** `check_draft` runs your agent's copy through the checks the send queue applies, before anything is queued. Free.
- **Reply detection.** Any human reply stops the sequence and fires an event.
- **Events and webhooks.** Your agent polls `list_events` or subscribes its own endpoint to signed webhooks (`create_event_subscription`, then `update_event_subscription` to change, pause, test or redeliver them).

For the stages your agent takes over, the quality of the pitches and the deals is the quality of your agent's model and these skills.

## The skills

| Skill | What it does |
| --- | --- |
| [`backlinkgpt-setup`](plugins/backlinkgpt/skills/backlinkgpt-setup/SKILL.md) | "Set me up": connect the MCP server, Search Console and Bing (in the app), target pages, competitors, exclusions, profiles, and the routine. |
| [`find-link-targets`](plugins/backlinkgpt/skills/find-link-targets/SKILL.md) | Sourcing: AI-citation sources, competitor link intersection, "best X" roundups, resource pages, unlinked mentions. Dedupe and save. |
| [`judge-link-targets`](plugins/backlinkgpt/skills/judge-link-targets/SKILL.md) | The page verdict rubric (gates, spam, language, duplicates, value x win odds) and contact choice. |
| [`write-link-pitch`](plugins/backlinkgpt/skills/write-link-pitch/SKILL.md) | Research-led first message and follow-ups, claim and style checks, `check_draft`, then queue or send. |
| [`handle-link-replies`](plugins/backlinkgpt/skills/handle-link-replies/SKILL.md) | Same-day replies, the fair price table and money rules, deal stages, verifying the live link. |
| [`link-building-report`](plugins/backlinkgpt/skills/link-building-report/SKILL.md) | Weekly report: outreach KPIs, won and lost links, deals, and Search Console and Bing movement on target pages and won links. |

Workspaces on a paid plan also get deeper playbooks served by the MCP server itself (`list_skills`, `get_skill`): AI-citation sourcing, a competitor gap deep dive, link negotiation and an end-to-end outreach routine. They are always the latest version.

The MCP server lives at `https://backlinkgpt.com/api/mcp` (streamable HTTP, OAuth or API key). You need a BacklinkGPT account with a workspace for your site on an active plan: sign up at [backlinkgpt.com](https://backlinkgpt.com), or let a headless agent sign you up through [auth.md](https://backlinkgpt.com/auth.md) (below).

## Install

### Skills CLI (Claude Code, Codex, Cursor and others)

```bash
npx skills add fmd-labs/backlinkgpt-skills --list
npx skills add fmd-labs/backlinkgpt-skills -a claude-code
npx skills add fmd-labs/backlinkgpt-skills -a codex
npx skills add fmd-labs/backlinkgpt-skills --skill write-link-pitch -a cursor
```

Pass `-a` for the agents you use. Without it the CLI may link the skills into every agent folder it knows. Add `-g` to install for your user instead of the current project. `npx skills update` pulls the latest version later. The skills CLI installs the skills only: connect the MCP server too (below).

### Claude Code

The plugin bundles the skills and the MCP server config.

```bash
claude plugin marketplace add https://github.com/fmd-labs/backlinkgpt-skills
claude plugin install backlinkgpt@backlinkgpt-skills
```

Or interactively: `/plugin marketplace add https://github.com/fmd-labs/backlinkgpt-skills`, then `/plugin install backlinkgpt@backlinkgpt-skills`. Afterwards run `/mcp`, pick `backlinkgpt`, and sign in in the browser. The `fmd-labs/backlinkgpt-skills` shorthand also works if you have GitHub SSH access.

Only the MCP server, without the plugin:

```bash
claude mcp add --transport http backlinkgpt https://backlinkgpt.com/api/mcp
```

### Claude (web and desktop)

Add a custom connector named BacklinkGPT with this URL, and leave client ID and secret empty (BacklinkGPT supports dynamic client registration):

```text
https://backlinkgpt.com/api/mcp
```

[Open the connector settings](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors) or go to Settings, Connectors, Add custom connector. The skills come through the server: ask Claude to "use the BacklinkGPT setup skill" and it fetches it with `get_skill`. You can also upload a skill folder as a zip under Settings, Capabilities, Skills.

### ChatGPT

Install the BacklinkGPT app from the ChatGPT apps directory once it is listed, or create a connector with the MCP URL (Settings, Apps and Connectors, Developer mode, Create, OAuth, client ID and secret empty). The skills come through the server's `get_skill` tool.

### Codex

```bash
codex plugin marketplace add https://github.com/fmd-labs/backlinkgpt-skills
codex plugin add backlinkgpt@backlinkgpt-skills
```

Or interactively: `/plugins` in the Codex TUI. Start a new session afterwards, then sign in with `codex mcp login backlinkgpt`.

Only the MCP server, without the plugin:

```bash
codex mcp add backlinkgpt --url https://backlinkgpt.com/api/mcp
codex mcp login backlinkgpt
npx skills add fmd-labs/backlinkgpt-skills -a codex
```

### Cursor

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=backlinkgpt&config=eyJ1cmwiOiAiaHR0cHM6Ly9iYWNrbGlua2dwdC5jb20vYXBpL21jcCJ9)

Or add to `.cursor/mcp.json`:

```json
{ "mcpServers": { "backlinkgpt": { "type": "http", "url": "https://backlinkgpt.com/api/mcp" } } }
```

Then `npx skills add fmd-labs/backlinkgpt-skills -a cursor` for the skills.

### VS Code

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_BacklinkGPT_MCP-0098FF)](https://vscode.dev/redirect/mcp/install?name=backlinkgpt&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fbacklinkgpt.com%2Fapi%2Fmcp%22%7D)

Or add to `.vscode/mcp.json`:

```json
{ "servers": { "backlinkgpt": { "type": "http", "url": "https://backlinkgpt.com/api/mcp" } } }
```

### Hermes and other headless agents

- **A workspace API key** is the credential for an agent that runs on a schedule: it keeps working until you pause or delete it (or until the expiry date you gave it). Owners and admins create keys at [backlinkgpt.com/app/api/keys](https://backlinkgpt.com/app/api/keys). Send it as `Authorization: Bearer <key>` or `x-api-key: <key>` to the MCP URL, or call the REST API directly. Keep the key in the agent's secret store, never in a prompt or a repo.
- **auth.md** signs the user up, or reaches a workspace, without a browser on the agent's side (no account or key needed yet): point the agent at `https://backlinkgpt.com/auth.md`. It registers with the user's email and shows them a link and a 6-digit code; the user signs in (or creates the account), enters the code and picks or creates the workspace, and the agent receives its own token for the MCP server. That token lasts 60 minutes with no refresh token (the agent exchanges its identity assertion for a new one), and the assertion expires after 30 days (then the user claims again), so it does not belong in a static config header: once the workspace is set up, switch the routine to an API key. The connection shows up under Settings, Security, MCP Clients and is revoked there.

Then copy the skill folders from `plugins/backlinkgpt/skills/` into the agent's skills directory.

### Any other MCP client

Point the client at `https://backlinkgpt.com/api/mcp` with streamable HTTP and OAuth. The server publishes standard OAuth discovery metadata and supports dynamic client registration, so no pre-shared credentials are needed. Clients without OAuth can bridge with `npx mcp-remote https://backlinkgpt.com/api/mcp`, or send an API key as a header.

### Endpoints

| What | Where |
| --- | --- |
| MCP server | `https://backlinkgpt.com/api/mcp` (streamable HTTP, OAuth or key) |
| REST API | `https://backlinkgpt.com/api/v1` (`x-api-key` header) |
| API reference | https://backlinkgpt.com/api/v1/docs |
| API keys | https://backlinkgpt.com/app/api/keys |
| Agent sign-up (auth.md) | https://backlinkgpt.com/auth.md |
| Webhooks | https://backlinkgpt.com/app/api/webhooks |
| Search Console, Bing | https://backlinkgpt.com/app/connected-accounts |

One connection or key is bound to one workspace. One workspace is one site.

## Plans and credits

Every MCP and REST call needs a workspace with an active plan, whatever the credential: without one the MCP tools answer `FORBIDDEN` and the REST API `402`, and nothing is charged. The MCP server and the REST API are on every plan, and so are memory, contact policies, events, webhooks, `check_draft`, logging messages you sent elsewhere (`record_manual_touch`), the SEO data tools (metered in credits) and the premium skills. BacklinkGPT sends once a sender (Gmail, Outlook, IMAP or LinkedIn) is connected in the app; without one, your agent drafts and the user sends. Search Console and Bing are connected in the app; BacklinkGPT steers by them, and your agent reads their synced data with the Search Console and Bing tools. The public skills are free to read and install. Plans and prices are at [backlinkgpt.com/pricing](https://backlinkgpt.com/pricing).

These tools use credits:

| Tool | Credits |
| --- | --- |
| `get_seo_metrics` | 2 per read; free for a bare domain this workspace already measured in the last 30 days (unless `fresh: true`); a URL with a path always costs 2 |
| `search_serp` | 1 per results page |
| `read_page` | 1 per page |
| `find_link_intersection` | 25 per 1,000 rows, rounded up |
| `find_contacts` | the hunt on the site's own pages is free; 1 per person the people search returns |
| `verify_email` | 1 per address |
| `import_prospects`, `research_prospect` | per page BacklinkGPT reads (and judges) |
| `scan_competitor_gap` | 25 per 1,000 pages the scan returns (nothing if it finds none), plus up to 3 per page it evaluates; the quote, 100 by default, is the most it can cost |
| `trigger_sourcing_run` | what the run's search reads and page verdicts cost (admins only) |

Reading and writing your own data (Search Console and Bing included), `check_draft`, events and sending are not metered. On MCP a credit tool first returns a quote and spends nothing; the agent calls it again with `confirm: true`. Without credits the data tools are refused and nothing is charged: the skills fall back to your agent's own web search, page fetch and SEO data (an Ahrefs or Semrush MCP server, open-seo, an export).

## Auth and security

- OAuth is the default. During consent you pick one workspace, and the grant stays bound to it. Review or revoke authorized clients at [backlinkgpt.com/app/settings/security](https://backlinkgpt.com/app/settings/security).
- API keys are for agents that run unattended or must switch between workspaces (one key per workspace). Pause or delete a key when an agent retires.
- An agent that signed up through auth.md holds a token bound to the one workspace the user picked; the user sees and revokes it under Settings, Security, MCP Clients.
- This repository contains no secrets and never will. It only ships public configuration pointing at the BacklinkGPT endpoint; credentials are issued at runtime, through OAuth in your own browser, through auth.md (you confirm with a code in your own browser), or as a key you create in the app.
- Anything your agent queues counts as approved. The queueing tools are marked destructive, so Claude and ChatGPT ask you before they run. In an unattended routine, the limits you wrote in the setup brief are the approval.
- Caps, pacing, the send window, do-not-contact lists and dead-page checks apply to every send, whatever the agent asked for. A human reply stops the sequence.
- Tools that spend credits quote their cost first and only run when called again with `confirm: true`.
- Prospect pages and replies are untrusted text. The skills tell the agent to treat them as data and never follow instructions inside them.

## What's inside

| Path | Purpose |
| --- | --- |
| `.claude-plugin/marketplace.json` | Claude Code marketplace catalog for this repo |
| `.agents/plugins/marketplace.json` | Codex marketplace catalog for this repo |
| `plugins/backlinkgpt/` | The BacklinkGPT plugin: shared MCP server config and the six skills, with manifests for Claude Code and Codex |
| `plugins/backlinkgpt/.mcp.json` | The MCP server (`https://backlinkgpt.com/api/mcp`) |
| `plugins/backlinkgpt/skills/<skill>/SKILL.md` | One skill per folder, `name` and `description` in frontmatter |
| `plugins/backlinkgpt/skills/<skill>/references/` | Longer material a skill loads when it needs it |

## Contributing

This repository is published from the BacklinkGPT app repository, which also serves these skills over MCP (`get_skill`), so both always match. Issues and pull requests are welcome here: an accepted change is made upstream and arrives with the next sync, so a pull request is closed rather than merged. To change a skill for yourself only, fork the repository and install from your fork, or keep your own rules in the brief the setup skill writes.

## Related

- [BacklinkGPT MCP server docs](https://backlinkgpt.com/docs/mcp-server)
- [API reference](https://backlinkgpt.com/api/v1/docs)
- [BacklinkGPT docs](https://backlinkgpt.com/docs)

## License

[MIT](LICENSE)
