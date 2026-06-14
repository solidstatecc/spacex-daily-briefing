# Sources, queries & entity map

Read this at the start of every run. It is the canonical source list, the live
query set, the verified facts to anchor on, and the dated catalysts to watch.
Everything here is a **starting point to verify live** — figures move daily.

## Source list

### Official / company (use `browse_page` with a tight summarizer instruction)
| Source | URL | Pull |
|---|---|---|
| SpaceX site | https://www.spacex.com/ | Launch manifest, Starship/Falcon/Dragon status, Starlink milestones |
| SpaceX (Wikipedia) | https://en.wikipedia.org/wiki/SpaceX | History, segments, leadership, post-IPO context |
| Starbase (Wikipedia) | https://en.wikipedia.org/wiki/SpaceX_Starbase | Facility role, launch capability, status |
| Starlink (official) | https://www.starlink.com/ (`browse_page`) | coverage, direct-to-cell, pricing, availability — the profit engine |
| X / official accounts | x.com · x.com/SpaceX · x.com/Starlink · x.com/xai · x.com/grok | first-party posts (the daily X scrape follows these via `from:`); X itself is owned by xAI, inside SPCX |

### Stock & market (use `web_search`; `browse_page` for context only)
| Source | URL | Pull |
|---|---|---|
| Nasdaq SPCX | https://www.nasdaq.com/market-activity/stocks/spcx | Price, volume, market cap (JS page — corroborate the number) |
| TradingView SPCX | https://www.tradingview.com/symbols/NASDAQ-SPCX/ | Chart, range, intraday |
| Yahoo Finance | https://finance.yahoo.com/quote/SPCX and `/analysis` | Analyst targets/ratings, key stats |
| Kalshi (prediction markets) | https://kalshi.com/search?q=spacex&order_by=querymatch (`browse_page`) | crowd odds on SpaceX / Starship / SPCX events — a forward-looking sentiment signal; report as market-implied probability, not fact |

For the live quote, prefer a fresh `web_search` ("SPCX stock price today") and a
reputable finance/news source over a single scraped JS widget.

### News (use `web_search`)
- General: `query="SpaceX OR SPCX OR \"SpaceX IPO\" OR Starlink OR Starship news"`
- AP backgrounder (Musk wealth / valuation / employee equity): https://apnews.com/article/musk-spacex-tesla-ipo-trillionaire-billionaire-worth-rockets-7723f82b6063a9a17c194e25982cd66d

### SEC filings (high-signal for a new IPO — use `web_search` or EDGAR full-text search)
- EDGAR: https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&company=space+exploration  (confirm the CIK once, then watch it)
- Watch: **8-K** (material events), **S-1/424B** (prospectus — has the exact **lockup** terms), **Form 4** (insider buys/sells — a real overhang signal), first **10-Q**.

### Social — X (Grok's edge; `x_keyword_search` + `x_semantic_search`)
See query set below.

### Video — YouTube (use `browse_page`, fall back to `web_search`)
- https://www.youtube.com/hashtag/spacexipo
- https://www.youtube.com/hashtag/spacex
- https://www.youtube.com/hashtag/elonmusk
- https://www.youtube.com/hashtag/nasdaq
- Fallback: `query="SpaceX IPO OR SPCX site:youtube.com"`

### xAI / Grok + X — added because the merger folded these into SPCX
| Source | How |
|---|---|
| xAI / Grok news | `web_search query="xAI OR Grok news"` — model releases, compute deals, revenue |
| X (platform) | `web_search query="X platform revenue OR advertising OR regulatory"` |
| xAI on X | `x_keyword_search "(xAI OR Grok) min_faves:25 filter:has_engagement" mode="Latest"` |
| xAI official (company / news / Colossus) | https://x.ai/company · https://x.ai/news · https://x.ai/colossus (`browse_page`) | model releases, Colossus buildout, official metrics |

## Live X query set

Keep these fresh with `since:<yesterday's date>`. Raise the `min_faves:` floor when
volume is high — you want signal, not every reply.

- Broad buzz: `#spacex OR #SpaceXIPO OR #SPCX OR spacexipo OR "SpaceX IPO" OR "SPCX stock" min_faves:10 filter:has_engagement` — `mode="Latest"`, `limit=10`
- Elon, company-scoped: `from:elonmusk (SpaceX OR Starlink OR Starship OR IPO OR SPCX OR xAI OR Grok)` — `mode="Latest"`, `limit=5`
- Official accounts: `(from:xai OR from:grok OR from:SpaceX OR from:Starlink) since:<yesterday>` — `mode="Latest"`, `limit=8` — first-party announcements (x.com/xai, x.com/grok, x.com/SpaceX, x.com/Starlink)
- Cashtag / trader view: `$SPCX min_faves:10` — `mode="Latest"`, `limit=10`
- Semantic (sentiment/themes): `"SpaceX IPO stock reaction, Starlink updates, Starship tests, investor sentiment on SPCX"` via `x_semantic_search`, `from_date=<yesterday>`

## Verified anchor facts (as of 2026-06-15 — re-verify each run)

These are confirmed from reporting around the IPO. Use them to sanity-check live
data and to flag inflated round numbers.

- **IPO:** priced at **$135**/share, **June 12, 2026**, on Nasdaq as **SPCX**. Opened ~$150, **closed first day ~$161 (+19%)**. Raised **~$75B** — the largest IPO on record.
- **Implied valuation:** **~$1.77T** at debut. Treat "$2T+" as a claim to verify, not a baseline.
- **Structure:** the listed entity is the **combined SpaceX × xAI** company (all-stock merger Feb 2026; xAI acquired at ~$250B, combined entity ~$1.25T pre-IPO). Grok and X sit inside it. Grok is an xAI product, so a Grok-authored brief has a disclosed interest — flag it neutrally and report straight (including the losses).
- **AI segment (xAI + X):** xAI develops **Grok** (Grok 4.1 topped LMArena; tens of millions of MAU) and runs the **Colossus** supercomputer (Memphis; ~555K NVIDIA GPUs, ~2GW target, Colossus 2 online). **X** (acquired by xAI Mar 2025) sits under xAI. AI-segment 2025 revenue ~**$3.2B** against a ~**$6.4B operating loss** — the primary driver of the consolidated **net loss (~$4.9B on ~$18.7B revenue, 2025)**. The burn is the central bear-case number; frontier AI + orbital compute is the bull case.
- **Starlink:** **>10M** active customers (Feb 2026), ~160 markets; ~**$11.4B** 2025 revenue (~60%+ of total). The profit engine.
- **Starshield:** classified/government tier; held a **$537M** defense contract (through 2027) — watch for new awards.
- **Terafab:** early semiconductor effort — optionality, not yet material; flag concrete news.
- **Musk stake:** widely reported but **percentage varies by source — verify before quoting any specific %.**

Sources for the above: CNBC (2026-06-11, 2026-06-12, 2026-05-21), Nasdaq newsroom, CNN Business (2026-06-12), Yahoo Finance live blog. Re-pull live each run.

## Key metrics to watch (check every run)

A fast checklist so each brief stays decision-oriented, not just descriptive:

- SPCX price + daily % move + volume vs. the short post-IPO average
- Market cap / implied valuation, and distance from the $135 IPO price
- Analyst targets/ratings (any new initiation, upgrade, or downgrade)
- Prediction-market odds (Kalshi) on near-term SpaceX / Starship / SPCX events — a forward-looking crowd signal (label as market-implied probability)
- Starlink: subscriber count, ARPU, direct-to-cell / international expansion
- xAI / Grok: a Grok model release or benchmark move, Colossus / compute deals, X ad revenue, and the AI-segment burn rate (the bear-case number)
- Next Starship flight window; Falcon 9 cadence
- Starshield / government award activity
- Insider Form 4 activity and proximity to the lockup-expiry date
- Elon Musk post sentiment (SpaceX/xAI/X) + overall X & YouTube buzz level

## Dated catalysts to watch (price-moving lens — verify exact dates live)

- **Analyst quiet period ends** ~25 days after IPO (early **July 2026**): expect a wave of initiations/price targets — a known volatility event.
- **IPO lockup expiry** ~180 days after pricing (~early **December 2026**): insider supply can pressure the stock; confirm the exact date from the S-1/prospectus.
- **First earnings as a public company:** date TBD — watch for the announcement.
- **Operational catalysts:** next Starship flight, major Starlink/Starshield awards, direct-to-cell rollouts, any secondary offering.
