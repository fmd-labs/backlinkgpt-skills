---
name: write-link-pitch
description: Write the first outreach message and follow-ups for a backlink request, check them (claims, style, check_draft), and queue or send them with BacklinkGPT. Use when drafting a link pitch, an "add us to your list" email, a link insertion or resource page request, a nofollow-to-dofollow ask, a follow-up sequence, or as the writing step of a link-building routine. Research-led, short, plain text, one ask, no links in the first touch, no AI tells. With sending active it queues with start_outreach and followUpSteps; without it the user sends from their own mailbox and you log with record_manual_touch. While the workspace's write outreach stage is on, BacklinkGPT drafts its own outreach: use this skill when that stage is off, for an engagement you run (followUpMode agent), or when the user asks you to write one.
---

# Write a link pitch

The message that gets answered names something true and specific about their page, offers one thing that fits it, asks for one thing, and stops. In our own campaigns, leading with what we actually saw on their page beat "I came across your site". In BacklinkGPT's outreach data, drafts with one concrete page detail, a plain paid line and short copy got replies from 14 to 27% of recipients, while generic drafts got almost none.

**Who writes.** `get_autopilot_stages`: with `writeOutreach` on, BacklinkGPT picks the contacts, starts the outreach and drafts every message (the user reviews them in the inbox or they autosend), so you write only when the user asks, and with `followUps: "agent"` so its drafter stays away from that engagement (always with your own `firstMessage`: a start nobody would write the first message of is refused). With it off, every message is yours and every live engagement reads `followUpMode: "agent"`: `start_outreach` needs your own `firstMessage`, and with `followUpSteps` (or `followUps: "agent"`) the engagement stays yours if the user turns write outreach back on; one started without either goes back to BacklinkGPT then.

Email first. Email won 9 of every 10 links we got. Prospects contacted on LinkedIn first won a link 0.2% of the time, against 2.0% for email. Use LinkedIn only when there is no email route.

## 1. Gather

- `get_prospect_verdict`: verdict, rationale, ask, and the notes saved when it was judged. The notes should already hold what to quote.
- Their page text: `read_page` (1 credit) or your own fetch, if the notes are thin or older than a few weeks. You may only state what the page shows. `check_draft` tests your claims against the read BacklinkGPT stored when it imported the page.
- Your target page: the site profile and the page itself. Describe it only from what it shows.
- The contact: `get_contact`. Are they the byline author? Is the first name a real name?
- History with this person: earlier threads in `list_inbox` and `get_conversation_thread` (with sending active also `get_outreach_history`).
- The brief: sender name and exact sign-off, voice samples, facts you may state, money rules.
- Optional: the target page's Search Console queries (a Search Console tool if your tool list has one). Queries it already ranks for are good wording for the entry. Never quote the numbers.

**Skip gate.** No specific, checkable observation about their page, no message. Leave the page out, list it in the summary as evaluate with the reason, and move to the next one. A generic pitch costs the sender's reputation and gets nothing.

## 2. Write the first touch

Write like the sender writes, using the voice samples in the brief. One person writing to another.

**Opener**: one checkable fact about their page, phrased so they could check it in five seconds. Pick one:

- the entries their list gives: how many, and two by name exactly as the page writes them
- the section where the user would fit, quoting its heading exactly
- a dated detail: the year in its title, its last-updated date

Then the **offer** in their format, after one short lead-in tied to their page ("If you refresh it for 2026:"):

- On a list: brand name, then one factual sentence on what it does and for whom, shaped like their other entries.
- On an article: one or two ready-to-paste sentences in the article's style that name the brand.
- Every fact in the offer comes from the target page or the brief's facts. Never "the best", never "a great fit".

Then **one ask**: whether they want the link, whether they look after updates to that page, or whether a short blurb would help. One question, or none when the offer is the ask.

**Paid line** (only when the brief's per-link budget is above $0, and the site is not a university, school, library, hospital, government body or charity, not an ask-first competitor, and not a no-audience or inflated site): one plain conditional sentence, its own line, for example "If additions to the page are paid, we're open to that." Never an amount, never "happy to cover a fee".

**Shape**:

- 25 to 80 words in the body, 120 at most. Short paragraphs with blank lines between them. Plain text: no bullets, bold, images or tracking.
- No links in the first touch, not even to the target page. Name the brand; send the URL when they answer or in the follow-up.
- Greet by first name. No usable name: greet the site's team by name ("Hi Freelance Weekly team").
- Sign off exactly as the brief says. Translate a title literally (Founder: Gründer, Fundador), never invent one.
- Subject: plain and specific, naming their list or section the way the page names it, with the brand: `Trackly for "Best time tracking apps"`. No question hook, no emoji, never starting with "Quick".

**Language**: the page's language, written the way professionals in that market write, not translated. Choose one register and keep it in every sentence (German du for startups and marketers, Sie for conservative companies, journalists and institutions; French usually vous). Use the language's own punctuation.

**Who wrote it**: say "your article" only to the byline author. Anyone else gets "the guide on <site>" or "your team's article".

### Variants by ask

- **add_to_list**: the shape above.
- **resource_link**: name the section of their resource page; the offer is one line on what the resource gives their readers.
- **editorial_mention_link**: quote or name the passage where one sentence would fit; offer that sentence.
- **follow_upgrade** (they already link to the user with nofollow): a warm note, shorter than a pitch. Thank them for the specific mention, ask once whether it could be a normal followed link ("without the nofollow attribute" is plain enough), offer a small give-back (an updated screenshot, a quote, a fact check of what they wrote). Say the thanks stands either way. No paid line.
- **link_existing_mention** (they name the user without a link): quote the mention as their page shows it and ask whether the name could link to the site. No second entry.
- **guest_contribution**: only when the site visibly publishes contributed posts. Propose one specific title that fits their readers and the target page.
- **Referral** ("talk to my colleague"): open with who sent you.
- **Earlier thread with this person**: acknowledge it in a clause, never re-pitch what they declined, honor what they said (prices, policies, formats), and keep it shorter. Never thank them for connecting again.

Examples for each: `references/pitch-examples.md`.

## 3. Check before anything is queued

**Claim check.** List every statement the message makes about their page, subject included. Each must be on the page you read:

- No absence claims at all: never say their page is missing, skips, lacks, leaves out or doesn't include something, in any language. In an audit of BacklinkGPT drafts, 4 of 7 such claims that could be checked were false, and the owner knows their page.
- A named section must exist as a heading on the page. A tag or archive page is not a curated "overview".
- Do not join two things the page says separately, and do not credit the page with a view or emphasis it does not state.
- Facts about the target page must match the target page.

If a claim fails, rewrite once with nothing about their page beyond its title and topic. If it still fails, do not queue it; hold it for the user with the sentence that failed.

**Style check.** Now load `references/style-check.md` and check the draft against it (keep that list out of your writing step; reading banned phrases while writing primes them). If the user has a tell checker such as the human-voice `ai_tells.py`, run it. Fix the exact problems found and check again, up to three rounds.

**Queue check.** Run `check_draft` (free; nothing is sent, stored or charged) on the first message and the follow-ups, with `prospectUrl`, `targetPageUrl` and `contactEmail`, or the `engagementId` or `conversationId` of an existing thread. Errors are what the queue would refuse (follow-up steps) or hold for review (a first message, `add_touch`): a link to a site other than the user's, the prospect's or one given in the thread, an outside email address, text that reads like instructions to an AI, payment details, a placeholder, a missing subject, a LinkedIn note that is too long. Warnings include absence claims tested against the stored read of their page, and stock phrases. Fix every error, weigh the warnings. `asQueued` is the text exactly as it would go out. Run it on copy the user sends by hand too.

## 4. Follow-ups

Write them now, so they ship as one sequence on the same thread. A human reply stops the sequence automatically: every pending step is cancelled, and you answer the reply instead.

- Default one follow-up, 4 working days after the first message. A second, very short one, 7 days later, only for high-value pages.
- Each brings one new element: a sharper reason it fits their page, a different angle on the value, the target URL (links are fine now), or the paid line if the first lacked it and the brief allows it.
- Mention the first email only in passing, in your own words. Never open with a stock follow-up line.
- Shorter than the first message. Same register, same sign-off.

## 5. Queue or send

### With sending active: `start_outreach`

Pass `prospectUrl`, `targetPageUrl`, the contact (`contactId` from `add_contact`, or the email), `firstMessage` (channel email, subject, body, optional send time) and `followUpSteps` (each step a channel, a body, `delayDays` after the previous touch or after a LinkedIn acceptance, and for email whether it replies in the thread; the tool schema has the exact fields). Steps make the engagement yours (`followUps: "agent"`): BacklinkGPT sends each one as written when it comes due and never drafts for it. Add a `note` for referral context.

- Queued means approved, and it sends inside the send window. In a chat, show the exact text and get the user's yes first; the tool is marked destructive so the client asks. In an unattended routine, the brief's limits are the approval, and the run summary lists every message queued.
- Stay within the brief's daily limit of new conversations.
- A first message to an ask-first competitor, to a site that already links to the user, or one whose copy the check flags waits in the review queue instead (`status: pending_review`, the reason in `note`); the user approves it in the inbox.
- A refusal is an answer, not an error: do-not-contact, competitor policy, already engaged, no sender, cap reached, steps that do not fit the touch gap or max touches, or copy that fails the check (`findings`). Read the note and move on.
- Later changes: `set_follow_up_steps` to rewrite pending follow-ups (on an engagement you run), `add_touch` for one extra message, `reschedule_attempt`, `cancel_attempt`, `stop_engagement`. `send_attempt_now` skips the queue and needs the user's explicit yes. `get_engagement` shows the steps and what each became.

### Without sending: send from the user's mailbox

1. Create the draft in the user's mailbox if your mail connector can, or give them the text to paste. The user presses send.
2. Once it is sent, log it with `record_manual_touch` (channel email, direction outbound, the contact's email, subject, body, when it was sent). The contact must be in memory first (`add_contact`). Logging keeps memory truthful and stops anyone pitching the same person twice.
3. Without sending BacklinkGPT does not send follow-ups. Put the follow-up date in the brief's state and draft it in that day's run.

### LinkedIn (only without an email route)

A connection note gets the connection, nothing else: at most 200 characters on a free LinkedIn account (300 on Premium), one specific reaction to their page, no pitch, no link, no ask. The pitch comes in the first message after they accept.

## Output

Per prospect: the subject, the body, the follow-ups with their delays, the claim check result, and whether it was queued, held, or handed to the user to send.
