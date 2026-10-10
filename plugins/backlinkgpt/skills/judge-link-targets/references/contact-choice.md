# Contact choice

Which person at a publisher gets the pitch. From BacklinkGPT's outreach data (engagements started June to September 2026; reply rate = share of contacted engagements that got a human reply).

## Route first

| Route | Emailed | Bounced | Replied |
| --- | --- | --- | --- |
| Byline author of the page, with an email | 142 | 2% | 27% |
| Address the publisher lists itself, a role inbox, or a verified address | 297 | 0% | 14% |
| Named person from a lead database, not verified | 341 | 21% | 6% |
| LinkedIn only (no email) | 78 touched | | 12% |

Two lessons:

1. The free hunt on the publisher's own site beats the paid lookup. On a sample of 80 pages that had a paid lookup, the site's own pages had a usable address on 67 of them. Look on the site first; pay for `find_contacts` only when it comes up empty.
2. An unverified named address bounces one time in five. Verify it before using it (`verify_email`, or the user's own verifier). Bounces hurt the sender's reputation for every later message.

## Role second

Inside a route, best first:

1. Byline author of the page
2. Editorial: editor, writer, author, journalist, content, blogger, press (German: Redakteur, Redaktion, Autor, Herausgeber)
3. Marketing, SEO, growth, communication, PR, partnerships, outreach, brand
4. Founder, co-founder, owner (German: Gründer, Inhaber, Geschäftsführer). At a small publisher this is the editor.
5. Other executives: chief, CEO, CTO, COO, CFO, CMO, president, VP, director, head of
6. Everyone else

A contact who probably left (their profile shows a current role elsewhere) sorts below everyone reachable. A contact who left is never picked.

## Where to look for free

- The byline and author bio on the page itself
- The site's contact, about, team, impressum (German sites must have one), and "write for us" or "contribute" pages
- The author's own profile page on the site

## Role inboxes

Shared inboxes a publisher lists for this kind of mail are trusted routes. Recognize them by the local part: info, contact, hello, hi, mail, office, team, editor, editors, editorial, newsroom, news, press, presse, pr, media, content, blog, tips, submissions, writeforus, guestpost, marketing, partner, partnerships, collab, sponsor, sponsorship, ads, advertising, business, redaktion, kontakt, verlag. Ranked by what the name says: editor@ is editorial, marketing@ is marketing, info@ is anyone.

`hi.jane@` is a person, not a role inbox.

## Names

- A role word in the first-name field ("Redaktion", "Team", "Editor", "Support", "Info", "Kontakt", "Office", "Webmaster") is not a name. Greet the site or team by name: "Hi Freelance Weekly team". Never a bare "Hi Team", never "Hi Redaktion" in an English email.
- Say "your article" only when the contact's first and last name appear in order inside one byline name (a middle name between them is fine). Two-author bylines are common; a bag-of-words match credits the wrong person. Otherwise refer to "the guide on <site>" or "your team's article".

## Never

- Do-not-contact, opted out, or on a suppression list
- An address that bounced or failed verification
- Someone who left the company
- A second live thread with the same person
- A new automated pitch to someone who replied to an earlier thread in the last 90 days, or got a full sequence in the last 180 days, unless the user starts it themselves. When the user does, the message must acknowledge the earlier exchange.
