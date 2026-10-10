# Verdict rubric

The full rules behind `judge-link-targets`. This is the BacklinkGPT verdict pipeline, written for an agent to apply by hand. Thresholds were tuned on a hand-labeled gold set of real prospect pages (October 2026): keep precision about 92%, recall of pages a human would keep about 91%.

## 1. Gate order

The order matters where two gates disagree.

1. **URL gates** (no read, no cost): community platform, excluded domain, off-limits competitor (`no_contact`), the user's own site.
2. **Fetch outcome**: gone (404, 410, a page that says not found, a redirect loop, a redirect to the homepage) drops. Unreadable (403, bot challenge, timeout) is evaluate, never drop: nothing about the page is known yet.
3. **Our followed link on an indexed page**: drop. The link exists; there is nothing to pitch. It is not a win either (nobody pitched it).
4. **Our link with an unreadable rel**: evaluate.
5. **Thin**: under about 200 characters of content.
6. **Duplicate**: see section 5. Judge the original instead when the copy names it.
7. **Spam**: see section 6.
8. **Language**: see section 7.
9. **Authority floor**: the minimum DR in the brief, if the user set one. Separately, a platform junk tier: DR 9 or lower AND under 100 organic visits a month goes to evaluate. A DR 1 blog with 4,000 visits survives on traffic alone.

Open gates pass facts to the judgment instead of a verdict:

- our nofollow, sponsored or ugc link: ask becomes follow_upgrade
- an unlinked mention of the brand: ask becomes link_existing_mention
- our followed link on a noindex page: it does not count until the page is indexed
- noindex: decided after the score (section 4)

## 2. Judgment

### Page types

list_or_roundup, comparison_or_alternatives, guide_or_howto, resource_page, statistics_page, directory, review, news_or_opinion, case_study_or_report, product_or_service_page, homepage_or_category, job_or_listing, forum_or_ugc, other.

Judge the page, not the site. A hub, homepage, category index, product or pricing page has no place for a third party.

### Publisher types

independent_blog, agency_or_consultant, adjacent_vendor, direct_competitor, media_publisher, big_brand, institution, community_platform, affiliate_or_review_site, engagement_seller, content_farm, other.

- Independent blogs, agencies, consultants and adjacent vendors (same buyer, different job) are normal, good targets.
- A direct competitor's main product does what one target page offers, for the same audience. Name it honestly; whether it may be contacted is the user's competitor policy, not your call.
- A sister brand or subsidiary of a listed competitor is the user's call too, until its domain is on a list.

### Audiences

A product can serve several audiences and each target page serves one. A page written for the second audience (creators, when the buyer is brands) fits a target page for that audience as well as a brand page fits a brand-facing one. Never call a page off-topic because its audience is not the main buyer.

### Editorial quality

- **high**: substantive, original, maintained by a real editor or expert
- **medium**: useful but generic; most AI-assisted company blog posts
- **low**: thin, dated or careless
- **spam**: junk you would not want a link from

### Quality flags (site or whole page, only when clearly true)

- **engagement_seller**: sells followers, likes, views, or a growth service for them
- **ai_content_farm**: a site that exists to mass-publish AI text with no business or editor behind it, or a page with leftover prompt text
- **paid_placement_or_advertorial**: the page itself is a labeled sponsored, partner or advertorial post
- **guest_post_farm**: a site whose business is hundreds of unrelated contributed posts
- **programmatic_page**: generated from a template at scale
- **link_farm**
- **parasite_hosting**: content on an open publishing platform borrowing its authority
- **gambling_or_adult**

Not a flag: a normal company blog post that is AI-assisted, generic, or carries one sponsored link. Rate that with editorial quality.

### Target fit

- **strong**: the page's topic is what the target page is about
- **moderate**: a related angle readers would welcome
- **weak**: an honest but minor angle; one sentence somewhere would fit (a TikTok tips post citing a views benchmark, a creator income guide naming a jobs board)
- **none**: mentioning any target page would be off-topic. A loose keyword overlap is not an angle.

### Win likelihood

- **high**: independent operator, small agency, adjacent SaaS blog with a visible editor
- **medium**: established blog or mid-size company
- **low**: giant brand, big media house, university, government, or no contact route

A news story or opinion piece with no list or resource section is a one-off: low odds, usually drop.

### Ask

| Ask | When |
| --- | --- |
| add_to_list | the page, or a section of it, lists several third-party tools, platforms or places (a real list) |
| resource_link | the page's job is to collect links: a resource page, links page, directory |
| editorial_mention_link | an article or guide without a list where one sentence fits; a statistics page that would cite the user's data |
| guest_contribution | only when the site visibly runs contributed posts and nothing else fits |
| follow_upgrade | the page already links to the user with nofollow, sponsored or ugc |
| link_existing_mention | the page names the user without a link |
| fix_indexing | a valuable page that is noindex (rare; a person decides) |

Corrections that always apply:

- add_to_list on a page that is not a real list becomes resource_link on a resource page or directory, else editorial_mention_link.
- resource_link on a page that is not a resource page or directory becomes editorial_mention_link. A guide that cites sources is pitched as a mention.
- "none" becomes add_to_list on a real list, else editorial_mention_link.
- A warm ask decided by a gate always wins.

## 3. Disqualifiers and decision order

1. Any quality flag, spam quality, publisher engagement_seller or content_farm: drop. On a warm ask, only engagement_seller drops (the editor already knows the user; a job board or AI-assisted post can still upgrade a link).
2. Self-promotional: drop.
3. Direct competitor not on the user's lists and not cleared: evaluate; ask the user to classify it.
4. No target page fits: drop.
5. forum_or_ugc: drop.
6. product_or_service_page or homepage_or_category without a real list: drop.
7. news_or_opinion, no real list, low odds: drop.
8. Noindex: value 3 or more is evaluate with ask fix_indexing; else drop.
9. Low odds, value 4 or more, authority 0.9 or more (about 250k visits a month): evaluate.
10. Expected value 0 or more: keep.
11. Value 3 or more: evaluate.
12. Else drop, with the numbers in the reason ("value 1.2 of 5, P(win) 8%").

## 4. Score

```
authority      = traffic score of the domain's organic visits (x 0.5 if inflated); 0.35 when unmeasured
page strength  = max(page traffic score with full marks at 10k visits, URL rating / 50); 0 when unmeasured
traffic score  = log10(visits + 1) / 6, capped at 1   (1k visits = 0.5, 1M visits = 1)

value          = 5 x relevance x quality x placement x (0.30 + 0.45 x authority + 0.25 x page strength), capped at 5
P(win)         = base x modifiers, capped at 0.60
expected value = value x P(win) - 0.05   (0.05 = the cost of one outreach sequence)
```

| Relevance (target fit) | | Quality | |
| --- | --- | --- | --- |
| strong | 1.00 | high | 1.00 |
| moderate | 0.65 | medium | 0.80 |
| weak | 0.30 | low | 0.45 |
| none | 0 | spam | 0 |

Placement, from the page's outbound links:

| Situation | Factor |
| --- | --- |
| Gives followed links to other sites, or the ask is follow_upgrade | 1.00 |
| Links out, but every link is nofollow or sponsored | 0.45 |
| Links to nobody (a mention-only list) | 0.70 |
| 60 or more followed outbound links | x 0.8 |
| 150 or more followed outbound links | x 0.6 instead |
| Each competitor it links with a followed link (up to 3) | + 0.10 |

Win odds:

| Base (win likelihood) | | Modifiers | |
| --- | --- | --- | --- |
| high | 0.30 | real list | x 1.25 |
| medium | 0.15 | links to a competitor (followed) | x 1.20 |
| low | 0.05 | warm ask (follow_upgrade, link_existing_mention) | x 1.60 |
| | | ask-first competitor's page | x 0.50 |
| | | news or opinion | x 0.60 |

Worked example: a real list of time trackers on an independent blog, strong fit, medium quality, followed outbound links including two competitors, domain at 8k visits (authority about 0.65), page at 900 visits (page strength about 0.74). Value = 5 x 1 x 0.8 x 1.2 x (0.30 + 0.29 + 0.18) = about 3.7. P(win) = 0.30 x 1.25 x 1.20 = 0.45. Expected value = about 1.6. Keep.

## 5. Duplicates

A copy is never pitched: its editor cannot change it, or the publisher prefers another URL.

- **Staging or preview host**: a subdomain label staging, stage, dev, preview, test or beta (dev.to and test.io are real sites; only a subdomain counts), or hosts on webflow.io, wpengine.com, wpenginepowered.com, pantheonsite.io, kinsta.cloud. App hosts like vercel.app or netlify.app are fine.
- **Canonical on another domain**: a syndicated copy. Pitch the canonical URL.
- **Syndication notice** near the top or bottom: "originally published on/at/in <publication>", "republished from", "syndicated from", "this article first appeared on". "Originally published on March 3" is a date line on the original, not a notice.
- **Regional copy**: the page lists itself under one region (en-gb) and the same article under the user's market region (en-us). Pitch the market version.
- **Canonical to another URL on the same site**: pitch that URL. A canonical pointing a deep page at the homepage is a broken theme, ignore it.

## 6. Spam signals in the page itself

Only signals with no innocent reading:

- **Fake engagement seller**: three or more distinct "buy <platform> <followers|likes|views|subscribers...>" products in the visible text. An article that mentions buying followers once is not a seller.
- **Leaked AI text**, not in quotes: "as an AI language model", "here is the revised/updated/final version", "regenerate response", "certainly! here's the article". An article about AI writing quotes these; a page nobody edited does not.
- **Gambling or adult density**: five or more terms like online casino, sports betting, free spins, porn, escort service, and more than 0.4% of the words. A single mention of a casino client is fine.

Third-party spam scores (surface signals like TLD, thin pages, link ratios) missed the cases that cost money in practice. Read the page.

## 7. Language routing

- The page's language: the content first. `<html lang="en">` on a German translation is common on WordPress. Then metadata, then `<html lang>`.
- Accepted languages: those of the live target pages, plus the product's language when a target page's language is unknown.
- Aliases sites use: gr = el (Greek), jp = ja, cn = zh, kr = ko, se = sv, dk = da, cz = cs, ua = uk, nb and nn = no, iw = he. Danish, Norwegian and Swedish read each other.
- No match: pitch the hreflang alternate in an accepted language, preferring the user's market region, then the bare language, then any regional variant. Else the x-default alternate (if it is an article, not a homepage). Else the same path without its language prefix (`/da/foo` becomes `/foo`). Each alternative is judged like a new page. None: drop, or evaluate when the user added the page.

## 8. Site quality tiers

| Tier | Rule | Meaning |
| --- | --- | --- |
| no audience | under 50 organic visits a month and under 25 ranking keywords | worth a free ask only |
| inflated | DR 25 or more, under 2,000 visits, under 0.3 visits per referring domain | typical of link sellers; free ask only |
| thin | under 300 visits | small but real |
| healthy | everything else | |
| unknown | no traffic number | treat as mid-size, never as proof of quality |

In BacklinkGPT's outreach data the two weak tiers held about a fifth of contacted sites and none of the wins. They are never worth money (see `handle-link-replies`).

A clean DR 45 with real editorial mentions beats a noisy DR 56 built on agency footer credits. When a keep looks too good, sample its backlinks: "Website by" footer credits, many DR 0 to 5 sources, link-selling anchors and Blogspot networks mean inflated.

## 9. Notes that make the pitch easy

Write notes the writer can quote without re-reading the page:

- `List: "12 Best Time Tracking Apps for Freelancers (2025)", 12 numbered entries, Toggl Track to Clockify. Fits as #13.`
- `Section "Tools I use" in a guide by Maya Lin (byline). Competitors on page: Harvest (followed).`
- `Links to us nofollow from "time tracking" anchor in the third paragraph.`
- `Evaluate: big media, value 4.3 but low odds. Worth a personal pitch from the founder?`
