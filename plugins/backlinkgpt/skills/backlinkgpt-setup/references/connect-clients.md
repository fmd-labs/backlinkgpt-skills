# Connecting an agent to BacklinkGPT

MCP URL: `https://backlinkgpt.com/api/mcp` (streamable HTTP).

Three ways to connect:

- **OAuth** (default). The user signs in, picks the workspace on the consent screen, and approves. No key is pasted anywhere. Dynamic client registration is supported, so client ID and secret stay empty.
- **auth.md** (to sign the user up, or reach a workspace, without a browser on your side). Follow https://backlinkgpt.com/auth.md: you register with the user's email, the user opens the link you show, signs in or creates the account, enters your 6-digit code and picks or creates the workspace, and you receive a token for the MCP server. Its access token lasts 60 minutes and there is no refresh token: exchange the identity assertion auth.md returns for a fresh one. The assertion itself expires after 30 days, and then the user claims the connection again with a new code. For anything that runs on a schedule, switch to an API key once the workspace is set up.
- **API key**. For agents that run unattended or must switch between several workspaces (one key per workspace). Owners and admins create keys at https://backlinkgpt.com/app/api/keys. Send it as `Authorization: Bearer <key>` or `x-api-key: <key>`. The key acts as the member who created it.

Whichever you use, the workspace needs an active plan before the tools answer.

The same key works for the REST API at `https://backlinkgpt.com/api/v1` (`x-api-key` header, reference at https://backlinkgpt.com/api/v1/docs).

After connecting, call `get_agent_status`. If it answers, the connection works.

## Claude (web and desktop)

Settings, Connectors, Add custom connector. Name: BacklinkGPT. URL: the MCP URL. Leave the advanced OAuth fields empty. Click Connect and approve in the browser.

Skills: the agent can fetch them with `list_skills` and `get_skill`, or the user uploads a skill folder as a zip under Settings, Capabilities, Skills.

## ChatGPT

Install the BacklinkGPT app from the ChatGPT apps directory once it is listed. Until then: Settings, Apps and Connectors, turn on Developer mode under Advanced settings, Create, paste the MCP URL, authentication OAuth, client ID and secret empty.

Skills: ask ChatGPT to load them with `get_skill` ("use the BacklinkGPT setup skill").

## Claude Code

```bash
claude mcp add --transport http backlinkgpt https://backlinkgpt.com/api/mcp
```

Run `/mcp`, pick `backlinkgpt`, sign in. Or install the plugin, which brings the skills and the server config:

```bash
claude plugin marketplace add https://github.com/fmd-labs/backlinkgpt-skills
claude plugin install backlinkgpt@backlinkgpt-skills
```

Headless (routines on a server, CI): add the server with a header instead of OAuth.

```bash
claude mcp add --transport http backlinkgpt https://backlinkgpt.com/api/mcp \
  --header "x-api-key: $BACKLINKGPT_API_KEY"
```

## Codex

The plugin brings the skills and the server config:

```bash
codex plugin marketplace add https://github.com/fmd-labs/backlinkgpt-skills
codex plugin add backlinkgpt@backlinkgpt-skills
```

Start a new session, then `codex mcp login backlinkgpt`. Or add only the server:

```toml
# ~/.codex/config.toml
[mcp_servers.backlinkgpt]
url = "https://backlinkgpt.com/api/mcp"
```

```bash
codex mcp login backlinkgpt
```

Headless: replace the login with a key from the environment.

```toml
[mcp_servers.backlinkgpt]
url = "https://backlinkgpt.com/api/mcp"
bearer_token_env_var = "BACKLINKGPT_API_KEY"
```

Skills: `npx skills add fmd-labs/backlinkgpt-skills -a codex`.

## Hermes and other self-hosted agents

Use an API key: it keeps working until the user pauses or deletes it (or until the expiry date they gave it), which is what a scheduled agent needs. Add an HTTP MCP server named `backlinkgpt` with the MCP URL and the header `Authorization: Bearer <key>` in the agent's MCP config (Hermes: the `mcp_servers` section of its config). When the user has no account yet (or cannot create a key), sign in through https://backlinkgpt.com/auth.md first: give the user the link and the 6-digit code, and keep the identity assertion in the agent's secret store. An auth.md token does not belong in a static header: it lasts 60 minutes with no refresh, so exchange the assertion for a fresh token before each run, and the assertion expires after 30 days (then the user claims again). Once the workspace is set up, ask the user for an API key and switch to it. Load the credential from the agent's secret store. Copy the skill folders into the agent's skills directory. Restart the agent so it picks up the server, then call `get_agent_status`.

## Cursor, VS Code, others

Any client that supports remote HTTP MCP servers with OAuth works with the MCP URL. Clients without OAuth support can bridge with `npx mcp-remote https://backlinkgpt.com/api/mcp`, or send the API key as a header if the client allows custom headers.

## Revoking

OAuth clients and agents connected through auth.md: https://backlinkgpt.com/app/settings/security. API keys: pause or delete at https://backlinkgpt.com/app/api/keys when an agent retires.
