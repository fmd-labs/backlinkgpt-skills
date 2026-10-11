# Outreach brief template

The routine reads this at the start of every run. BacklinkGPT holds the data; this file holds the rules only the user can set. Keep it under two pages. Update it whenever the user corrects a draft or a decision.

Copy the block below into the place the routine reads (a file in the project, ChatGPT project instructions, the agent's memory) and fill it in with the user.

```markdown
# Link building brief: <site>

## Site
- Workspace: <site domain> (BacklinkGPT, sending <active|off>, credits <yes|no>)
- Stages the agent does (off in BacklinkGPT): <find sites | judge pages | write outreach | suggest replies>; BacklinkGPT does the rest
- What it is: <one or two sentences, as written with update_site_positioning>
- What it is not: <the categories outreach must stay away from>
- Audience: <who buys or uses it>
- Target pages: <url> (<language>), <url> (<language>)
- Markets and languages we pitch in: <e.g. en-US, de-DE>

## Money
- Most we pay for one link: <$0 | about $75 | about $150 | the fair price, decided per link>
- Never pay before the link is live unless the site has DR 50+ and 10k+ organic visits a month.
- Most we owe at once for links not live yet: <e.g. $300>
- Money commitments need the user's yes: <always | above $X>
- Link swaps: <allowed only when both links are live | never>

## Sender
- Name on the emails: <full name>
- Usual sign-off, exactly as written:
  <Best,
  Jane
  Founder, Trackly>
- Language register: <e.g. German: du with startups and marketers, Sie with publishers and institutions>

## Voice samples (written by the user, not by an AI; for tone only, never copy)
1. <a real email they sent>
2. <...>
3. <...>
Measured habits: median length <n> words, opens with <...>, ends with <...>, never uses <...>.

## Facts we may state (each with its exact meaning)
- <e.g. "Trackly has 4,000 paying freelancers" = paying accounts in September 2026>
- <a real customer we may name>
- <what the product does not do, so we never claim it>

## Never say
- <competitor names we do not mention, promises we do not make>

## Limits
- New conversations per working day: <e.g. 10>
- Credit budget per daily run: <e.g. 40>; per week: <e.g. 200>
- Follow-ups per cold thread: <1 or 2>
- Minimum domain rating for prospects (optional): <e.g. none, or 10>

## Needs the user's yes
- Any money commitment above the limits above
- Universities, schools, libraries, hospitals, government bodies, charities (never offer money there)
- Ask-first competitors' pages (trade only, never money)
- Anything outside this brief

## Never contact
- <domains, people, companies>

## State (the routine updates this)
- list_events cursor: <cursor>
- Last daily run: <date>, queued <n>, credits used <n>
- Sources that produced replies or wins lately: <e.g. ai_citation 3 replies, competitor_intersection 2>
```

## Notes for filling it in

- **Voice samples**: three to five, chosen for variety rather than topic, from the same channel (cold or warm business email). Older emails written before they used AI tools are the cleanest. If the user has none, write two drafts, let them edit, and keep their edited versions as samples.
- **Facts**: every first-person claim in a pitch must come from this list. One fact per message at most, and only when it helps the reader.
- **Money**: the fair price table in `handle-link-replies` already caps what a link is worth. The brief's ceiling only lowers it.
- **Edits**: when the user edits a draft, compare the two. A deletion is a candidate for "Never say", an insertion a candidate for the voice habits.
