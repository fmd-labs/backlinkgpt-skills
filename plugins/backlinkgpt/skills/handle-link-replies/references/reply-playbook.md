# Reply playbook

The reply desk's moves, the order to apply them in, and example replies. Same fictional product as the pitch examples: Trackly (time tracking and invoicing for freelancers), sender Jane.

## First, extract

From their latest message, before writing anything:

- **Disposition**: their stance only. A yes, a quoted price or an accepted connection is positive even when you will not pay that price. Your own stance never changes it.
- **Terms**: price exactly as written, one-time or recurring, what is included, link attribute (dofollow, nofollow, sponsored, ugc, unspecified), permanence (permanent, expiring, unspecified), which site and page.
- **Complete?** Only when a price, dofollow, and duration are all confirmed by them, for the page we pitched, and the fee is not recurring.
- **Their questions**, close to verbatim, and **what they asked us for**.
- **Deal status**: agreed (both sides accepted the same terms), negotiating, or none.
- **Payment requested**: they sent an invoice, a payment link, or bank or PayPal details.
- **Link placed**: only when they say it is live now. "We'll add it next week" is not placed.

## Moves

Pick the smallest set that fits.

0. **Several ways in** (free mention, swap, paid tiers): ask for the cheapest that still gets a link, and say which.
1. **Answer their questions** with facts from the thread, the brief and the deal profile only.
2. **Provide what they asked for** when you have it. Otherwise say it follows shortly. Never invent assets, stats or links.
3. **Link details** when asked, or when the deal is agreed and that is all that is left: URL, one anchor, the section, a one or two sentence snippet in their style.
3b. **A site worth $0** (no real audience, or the page does not show in Google) and they want a fee you never agreed: never accept it. If our earlier messages already offered to pay, make one low one-time offer of at most $50, justified by what comparable links cost. If we never offered, ask whether a free editorial mention works instead. Terms already agreed (an accepted price, an invoice for it, a live link) stand.
4. **Below market, or a price we countered and they accepted**, with dofollow and duration confirmed: take the deal. Acknowledge the exact terms in one clause and hand over everything they need. Do not re-ask what they stated.
5. **Fair price**: no counter at about $100 or less; above that one light counter about 15% lower, "at that number we have a deal". Held firm before: take it.
6. **Above market**: counter at the counter number with one clause of reason. Trade something non-monetary where natural (permanent, dofollow, a ready snippet, a quick yes). Direct and warm, never apologetic.
7. **Incomplete terms**: ask only for what is missing, from: price, dofollow or sponsored, permanent or for how long, placement and anchor, roughly what traffic the page gets. Skip anything already answered.
8. **Declined**: thank them in one line, no pushback.
8b. **We declined earlier** and they only accept that or offer other things: no reply needed, or one closing line. Never re-pitch, never ask for rates.
9. **Opted out**: one line confirming removal. No pitch, no door left open.

## Counter math

| Their quote vs fair price | Our opening number |
| --- | --- |
| More than 10% above fair (high) | 40 to 50% of their ask, capped at fair |
| Up to 10% above fair, ask about $100 or less | none: accept |
| Up to 10% above fair, ask above $100 | about 15% below their ask, once |
| Under half of fair (low) | none: accept quickly |
| Recurring fee | one-time price for a permanent link, compared against one year of their fee |
| Link worth $0 | none (move 3b allows one token offer only if we already offered to pay) |

Round to a clean number: nearest $5 under $100, nearest $10 above. Write it as one payment in their currency: "$120 one-time", "100 € einmalig".

## Example replies

### They quote a price above fair

Their site: about 3k organic visits a month (fair: $120). They wrote: "Our fee for a link insertion is $300. Dofollow, permanent."

```text
Hi Maya,

Thanks, dofollow and permanent works for us. For an insertion in an existing article we usually pay around $130 one-time, which is what comparable links cost. If that works, I'll send the snippet for the "Billing" section.

Jane
```

Opening at 43% of the ask. Never "our budget is", never their traffic.

### They want a monthly fee

They wrote: "Sponsored listings are $80 per month."

```text
Hi Maya,

Thanks for the details. A monthly listing isn't something we'd pay for, but a permanent dofollow entry in the list for $120 one-time would work for us. Is that possible?

Jane
```

One payment, a permanent link, no policy talk ("we only...", "our policy is").

### Terms incomplete

They wrote: "We can add you for $100."

```text
Hi Maya,

$100 sounds fair. Two quick checks before we go ahead: would the link be dofollow, and would it stay up permanently?

Jane
```

"$100 sounds fair" is not a yes to pay; the yes waits for dofollow and duration.

### Sponsored or nofollow offered

They wrote: "We can do a sponsored post, rel=sponsored, $150."

```text
Hi Maya,

Thanks for the offer. A sponsored link isn't something we'd pay for. Is a normal dofollow link in an existing article possible, or a free mention in the 2026 update?

Jane
```

### Agreed, they ask for details

They wrote: "OK, $120 one-time, dofollow, permanent. Send me the link and anchor."

```text
Hi Maya,

Great, $120 one-time for a permanent dofollow link.

URL: https://trackly.example/freelancers
Anchor: Trackly time tracking

In the "Getting paid faster" section, after the paragraph on invoicing tools, this would fit: "Trackly time tracking turns tracked hours into an invoice, so billing takes minutes."

Once it's live, send me the URL and the invoice, and the payment follows.

Jane
```

Then note the deal as agreed in the routine's state. "The payment follows" names no date.

### Invoice before the link is live (small site)

```text
Hi Maya,

Thanks for the invoice. Would it work for you if we pay once the link is live? Send me the URL when it's up and I'll get it paid.

Jane
```

The summary tells the user: invoice from Maya, $120, pay after the link is verified.

### Institution asks for a fee

A university library page asks for a "listing fee".

Do not answer with money. Draft:

```text
Hi Dr. Patel,

Thank you for getting back to me. Would you consider listing Trackly without a fee, as a free tool for students who freelance?

Jane
```

and tell the user: "Institution asked for a fee. Paying is your call."

### Link is live

They wrote: "It's up: https://example.com/time-tracking-apps".

1. Read the page: link present, points to the target URL, no nofollow, sponsored or ugc, page not noindex.
2. `mark_link_acquired`.
3. Reply:

```text
Hi Maya,

Just checked, it's live and looks right. Thank you.

Jane
```

Only write "just checked" after you checked. If the check fails, say what you see ("the link still has a nofollow attribute") and ask, before any payment.

### Silence after a price was agreed (day 10, no link)

```text
Hi Maya,

Checking in on the Trackly link we agreed on October 2 ($120, permanent, dofollow, "Getting paid faster" section). Is it still planned?

Jane
```

## Scheduling

You cannot see the user's calendar or send invites. When they offer times, never pick one: write "[FILL IN: which of their times works]" and tell the user. When they ask for times, never invent any: "[FILL IN: two or three times that work for you]". A message with a placeholder is never sent until the user fills it.
