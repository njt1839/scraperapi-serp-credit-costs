# Google SERP API: How Much Does One Search Cost, Which ScraperAPI Plan Fits, and What 25 Credits per Request Really Buys

ScraperAPI charges 25 credits for a Google or Bing SERP request, and the rate holds for every subdomain and regional domain. That one number settles most of the cost question before you open a plan grid: the free tier's 1,000 monthly credits cover 40 Google searches, and the 7-day trial's 5,000 credits cover 200.

Whether that is a good deal depends on volume. A rank tracker checking 200 keywords once a day burns 5,000 credits daily, roughly 150,000 a month, which is a very different purchase from a content team spot-checking 30 queries a week. ScraperAPI makes sense when you want structured SERP JSON from the same account and billing line you already use for page scraping, and when a flat per-request rate is easier to forecast than a price that shifts with every parameter you add. It is a weak fit if you need 100 organic results from a single HTTP call, or if your only optimization target is the lowest possible cost per thousand searches.

> **Quick pick:** use the 7-day trial first. It includes 5,000 credits with no card required, enough to measure your real burn at 25 credits per search rather than estimating it. Then pick the smallest tier whose monthly allowance covers that measured number, and turn on pay-as-you-go only if your volume spikes without warning. 👉 [Start the ScraperAPI trial and compare current plan allowances](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Every ScraperAPI plan, and what each tier actually changes

Plan prices and credit allowances are quoted per tier on the pricing page and are the part of this product that changes most often. What the documentation fixes is the billing mechanics, and those decide whether a tier survives contact with a production job.

| Plan | Monthly credits & concurrency | What happens when credits run out | Best for | Plan page |
| --- | --- | --- | --- | --- |
| Free | 1,000 credits, 5 concurrent threads | Resets at renewal | ~40 Google SERP requests a month; evaluating the API | [Free plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | Tier-set allowance, tier-set concurrency | Scraping pauses until credits renew unless auto-upgrade is enabled | One project, low daily keyword count | [Hobby pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | Tier-set allowance, tier-set concurrency | Same pause behavior; auto-upgrade optional | A single rank tracker with steady daily runs | [Startup pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | Tier-set allowance, tier-set concurrency | Same pause behavior; auto-upgrade optional | Several sites or markets tracked in parallel | [Business pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | Tier-set allowance, tier-set concurrency | Auto-upgrade available, plus pay-as-you-go up to a monthly cap you set | Agency workloads with seasonal peaks | [Scaling pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | Tier-set allowance, tier-set concurrency | Auto-upgrade and pay-as-you-go both available | Continuous data pipelines | [Professional pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | Tier-set allowance, tier-set concurrency | Pay-as-you-go available; no auto-upgrade, since this is the highest self-serve tier | High-volume SERP collection without a sales call | [Advanced pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Contract terms | Committed multi-million-credit volume | [Enterprise and custom plans](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Two mechanics in that table matter more than the tier names. First, auto-upgrade is opt-in and works on Hobby, Startup, Business, Scaling and Professional. Second, pay-as-you-go exists only on Scaling, Professional, Advanced and Enterprise. If your nightly rank check must never miss a night, a tier without one of those two options will simply stop when credits hit zero, and there is no penalty charge for that, only a gap in your data. Annual plans can also renew early to pull a fresh batch of credits.

Concurrency is a hard ceiling in the other direction. Exceeding your thread limit returns a 429 and the request is refused rather than queued, with no extra charge. For SERP work that means your effective throughput is capped by threads, not credits, once you run large keyword lists in parallel.

## The credit math: 25 per search, and where it grows

The credit table is short enough to memorize. A standard page is 1 credit. Amazon, Walmart and eBay are 5. LinkedIn is 30. Google and Bing, subdomains included, are 25. Bypassing Cloudflare, Cloudflare Turnstile, DataDome, PerimeterX/HUMAN or Akamai adds 10. So does `render=true`, `premium=true` or `screenshot=true`, and `ultra_premium=true` adds 30, with premium plus rendering at 25 and ultra premium plus rendering at 75.

Google searches at a flat 25 credits are the predictable part of that list. What people underestimate is depth. ScraperAPI's Google SERP endpoint documents `START` as a supported parameter and does not list `num` among them, and the sample response paginates in increments of 10. Pulling 100 organic results therefore means 10 requests, or 250 credits, not one request at 25. Budget for that before you promise a client "top 100 tracking."

There is a second-order effect worth knowing about even if you never use another vendor. Several SERP APIs price by multiplier rather than by flat rate. DataForSEO publishes a formula in which a keyword containing a search operator such as `site:` multiplies the base price by 5, `calculate_rectangles` or `load_async_ai_overview` doubles it, and a `depth` above the default adds its own factor. That is where the "$X per 1,000 requests" figures in comparison posts come from: they describe the cheapest possible call, not the call most people end up making. A flat 25 credits is duller and easier to defend in a budget meeting.

Three tools keep the burn visible: every response carries an `sa-credit-cost` header showing what that request cost, the dashboard's Domain Cost Estimator prices any URL before you scrape it, and `max_cost` caps a single request. There is also an `/account/urlcost` endpoint that accepts the same parameters you plan to send, so you can price a whole job programmatically.

## Structured SERP JSON or raw HTML: the 1-credit versus 25-credit decision

ScraperAPI's Google SERP endpoint returns parsed, structured results. The docs list companion structured endpoints for Google News, Google Shopping, Google Jobs and Google AI Mode, each with an async variant for batch work. That parsing is most of what the 25 credits buy, compared with 1 credit for a plain page request that hands you HTML.

The choice between them is a parsing-budget question. If you need position numbers, titles, links and snippets as fields, the structured endpoint removes an entire extraction layer you would otherwise maintain through every Google layout change. If you need the page as a browser would render it, including elements the parser does not expose, the 1-credit request plus your own parser is cheaper per call and more work per month.

Vendors differ on how much structure they will hand over in one tier. DataForSEO splits its output three ways: a Regular function that returns top 10 organic and paid results with no complete overview of featured snippets and other extra elements, an Advanced function that covers the full SERP, and an HTML function for raw markup. Anyone comparing headline prices across SERP APIs should check which of those three they are being quoted for.

## Whose Google results are you actually getting?

Any SERP API returns a snapshot keyed to the keyword, location, language and device you specified, not the page a logged-in user sees. DataForSEO states this outright: user preferences, search history and other personalized factors are ignored by their system and will not appear in returned results, and they suggest opening the returned `check_url` in incognito mode to confirm relevance.

That has practical consequences for rank reporting. If a client searches from a signed-in browser with local history and sees their page at position 4 while your dashboard says 7, both numbers can be correct. Building the report around "position for a de-personalized query from this city on this device" keeps the conversation honest, and it is worth writing that definition into whatever you send clients. The same caution applies to AI-generated summaries: whether an AI Overview appears at all varies by query, so a monitoring job that samples once will produce noisy presence rates.

## What a managed SERP API replaces

Building your own Google scraper runs into a fixed list of obstacles: CAPTCHAs, IP blocks, rate limiting tied to request patterns, geotargeting differences across locations and devices, and honeypot elements planted to catch automated access. Those five are the ones Bright Data names in its own documentation, and they are a fair description of the problem any self-hosted collector meets within the first week of production traffic.

A hosted API absorbs that layer and bills you per request instead. What you still own is the schedule, the keyword list, the storage, and the decision about how fresh the data needs to be. ScraperAPI's async SERP endpoint exists for the case where you would rather submit a batch and collect results later than hold a connection open.

## How the alternatives differ on the same job

Three other vendors come up repeatedly in this comparison, and their published mechanics differ in ways that matter more than their marketing.

| Vendor | Verified pricing model | Notable detail |
| --- | --- | --- |
| DataForSEO | Base price per method and priority, multiplied by paid parameters | Separate Live and Standard methods; Standard is cheaper but requires POST then GET; 10,000 POST+GET calls per minute combined; Regular, Advanced and HTML output functions |
| Bright Data | Pay per successful request | Its Google Search page advertises 5K requests per month free; city-level targeting, multi-engine coverage, async results stored for 24 hours; a query flag raises the chance of receiving AI Overviews |
| HasData | Per-request API with an npm client | Parameters include `q`, `location` as a canonical place name, `domain` and a two-letter `gl`; response objects vary, and its docs warn that not every property appears in every response |

Bright Data's free allowance is worth stating plainly, because 5,000 requests a month is far more SERP volume than ScraperAPI's 1,000 credits, which buy 40 Google searches. If you are evaluating on free tier alone, that comparison is not close. ScraperAPI's advantage sits elsewhere: one credit system across page scraping and SERP parsing, a flat 25-credit rate you can multiply in your head, and the cost-inspection tools described above.

Two claims from vendor pages should be treated as claims. Bright Data's own Google Search page states both "under one second" and "under 5 sec" for response time in different sections, so no single figure is reliable. Its note that AI Overviews appear in roughly 15-20%+ of results comes with no published methodology.

## When ScraperAPI is the wrong buy

Skip it if your monthly Google volume is small and fixed, since 40 free searches will not cover a serious tracker and you would be paying for a scraping platform to do one job. Skip it if you need a single call returning 100 organic results, given the `START`-based pagination. Skip it if the cheapest per-request rate in the market is your primary criterion and you are willing to manage tiered output functions and parameter multipliers to get there.

Choose it when three things are true: you already send page-scraping traffic through ScraperAPI, you want parsed SERP fields rather than HTML, and your keyword volume is predictable enough that credits divided by 25 gives you a number you can plan against.

Sizing runs from your own data, not from tier names. Multiply keywords by 25, multiply by runs per day, multiply by 30. That is your monthly credit requirement, and it should be compared against the allowance printed on each tier rather than against the plan's marketing label. A 300-keyword daily check is 7,500 credits a day, about 225,000 a month, which puts you well past the small tiers regardless of what they are called.

Before you pay, confirm three things on the plan page: that credits reset monthly rather than rolling over, which of auto-upgrade or pay-as-you-go your tier supports, and what the cancellation terms are. Then start the trial, run your real keyword list for a few days, and read the `sa-credit-cost` header on the responses. If the measured burn lands inside a tier with room to spare, that tier is the answer.

👉 [Compare ScraperAPI plans and start with 5,000 trial credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)
