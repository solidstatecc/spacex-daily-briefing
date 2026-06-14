---
name: spacex-daily-briefing
description: >-
  Generate a daily, investor-grade intelligence briefing on SpaceX — now public
  as SPCX on Nasdaq (IPO June 12, 2026) and merged with xAI. Use this skill
  WHENEVER the user asks for a SpaceX or SPCX update, a daily or morning
  briefing, an investor briefing, "what's happening with SpaceX", an SPCX
  stock/price check, Starlink / Starship / Starshield / Dragon news, SpaceX IPO
  or lockup / quiet-period watch, #SpaceX or #SpaceXIPO chatter, Elon-Musk
  headline risk on SPCX, or any recurring SpaceX market-intelligence digest —
  even if they don't name the skill. It pulls LIVE data via X Search and Web
  Search, applies a price-moving-signal lens, and outputs a tiered brief
  (one-line read → TL;DR → sections → sources). Prefer this skill over a plain
  web search for anything SpaceX/SPCX investor-facing.
---

# SpaceX Daily Briefing

Produce one tight, **investor-grade daily briefing** on SpaceX / SPCX. The reader
is a shareholder who wants the **price-moving signal first** and the strategic
context second. Bias every choice toward "what could move SPCX or change the
thesis in the next few days," not encyclopedic completeness.

This skill works in Grok Build (native **X Search** + **Web Search**) and in any
Claude-compatible agent. It is designed to be run once a day — as a slash command
(`/spacex-daily-briefing`) or headlessly on a schedule (see *Daily automation*).

**Quick start:** drop this folder in `~/.grok/skills/`, then run
`/spacex-daily-briefing` (or `grok -p "/spacex-daily-briefing"`). The first run reads
`references/` for the source list, live query set, and entity map.

## What's in scope (the combined entity)

Since the **February 2026 SpaceX × xAI merger**, the SPCX-listed company contains
more than rockets. Track all of it, weighted by stock impact:

- **Launch & vehicles** — Falcon 9 / Heavy cadence, Starship test/flight progress, Dragon (crew/cargo).
- **Starlink** — the profit engine (~60%+ of revenue): subscriber adds, pricing, capacity, direct-to-cell, regulatory wins/losses.
- **Starshield** — classified/government tier; defense contracts and awards.
- **Terafab** — the nascent semiconductor effort; treat as optionality, flag any concrete news.
- **xAI / Grok (the AI segment — the second pillar of the thesis)** — the Feb 2026 merger folded xAI into SPCX (xAI acquired by SpaceX at ~$250B; combined entity ~$1.25T pre-IPO). Track: Grok model releases and benchmark standing (Grok 4.1 led LMArena), usage/MAU, the **Colossus** supercomputer buildout (Memphis — hundreds of thousands of GPUs, Colossus 2 online), enterprise/compute deals — and critically the **AI-segment burn**: ~$3.2B AI revenue in 2025 against a multi-billion operating loss, the main drag pulling the consolidated company to a net loss. This segment is both the bull case (frontier AI + orbital compute) and the bear case (losses).
- **X (the platform — owned by xAI since Mar 2025, so inside SPCX)** — ad/subscription revenue, MAU, and regulatory items that flow into the consolidated entity.
- **The entity is now rockets + AI.** 2025 combined revenue ~$18.7B with a net loss (~$4.9B) driven by the AI segment. Starlink's profit and xAI's burn are the two numbers that move the thesis — verify both live.
- **Elon headline risk** — Musk posts/politics/legal that historically swing the stock.

Full source list, the entity map with as-of figures, and the live query set live in
`references/sources-and-queries.md` — read it at the start of each run.

## Tools (use the live ones every run)

In **Grok Build**, map each task to the native tool — these are the real tool names:

- `web_search` — news, the live SPCX quote, YouTube fallback. Pass a focused `query`; favor recent results.
- `browse_page` — summarize one page (spacex.com, Wikipedia) with a tight *summarizer instruction*. Good for background; **not** reliable for live stock numbers.
- `x_keyword_search` — the daily X scrape. Supports `mode="Latest"`, `limit`, and operators (`since:`, `min_faves:`, `from:`, `filter:has_engagement`).
- `x_semantic_search` — meaning-based X pass for sentiment/themes; supports `from_date`.
- `render_inline_citation` — attach a source to a specific claim.

In a non-Grok (Claude-compatible) agent, substitute your web search for `web_search`/`browse_page` and search `x.com` via the web for the X passes.

## Daily workflow

Do these in order. Fire independent searches in parallel where the tool allows.

1. **Set the window.** Get today's date. Look for the most recent prior brief in
   `./briefs/`. If one exists, your job is the **delta since then** — surface what's
   *new*, don't re-report yesterday. If none exists, use a rolling **48-hour** window.

2. **SPCX stock check.** Use `web_search` (e.g. `query="SPCX stock price today"`,
   `query="SPCX analyst price target OR rating"`). Capture latest price, daily % move,
   volume, market cap / implied valuation, and any analyst initiations or rating
   changes. Anchor to the verified IPO facts in the reference file ($135 IPO price,
   ~$161 first-day close, ~$1.77T implied valuation) — and treat round numbers like
   "$2T" or a specific Musk stake % as **claims to verify**, not facts. **Only state
   numbers you sourced this run.** A stale or invented quote is worse than "last
   confirmed close $X as of <date>; intraday unverified." `browse_page` on a JS quote
   page is fine for context, but don't trust a hard number it can't cleanly parse —
   corroborate with a finance source or mark it unconfirmed. Also scan prediction-market
   odds (Kalshi — kalshi.com/search?q=spacex) for a forward-looking read on near-term
   SpaceX/Starship/SPCX events; report these as market-implied probabilities, not fact.

3. **X scrape — the price-moving pass.** Grok's edge; lean on it via
   `x_keyword_search` (`mode="Latest"`). Run the query set in the reference file
   (`#SpaceX`, `#SpaceXIPO`, `$SPCX`, "SPCX stock", Starship/Starlink terms) plus a
   second pass `from:elonmusk` scoped to the company. Add `since:<yesterday>` and a
   `min_faves:` floor to stay fresh and signal-rich; optionally add an
   `x_semantic_search` pass for sentiment/themes. Capture: net sentiment, the 2–3 most
   viral or consequential posts (handles + links), and any breaking chatter not yet in
   mainstream news — labeled **unconfirmed** until corroborated.

4. **News pass.** `web_search` for SpaceX / Starlink / Starship / Starshield / xAI / X
   developments in the window: contracts and awards, launches and anomalies, earnings
   or revenue leaks, regulatory (FCC/FAA/SEC) actions, lockup / quiet-period milestones,
   index-inclusion talk, governance news.

5. **Video pass.** Check the YouTube hashtag feeds (`#SpaceXIPO`, `#SpaceX`,
   `#ElonMusk`, `#nasdaq`); if the hashtag page is thin, `web_search` with
   `query="SpaceX IPO OR SPCX site:youtube.com"`. List a handful with title, channel,
   and link. You're surfacing what's circulating — don't invent view counts.

6. **Synthesize** into the template below, ranked by **potential SPCX impact**.

## Output format — tiered brief

Fill `assets/brief-template.md`. Structure, in order:

1. **Headline (one line).** The net read: *Bullish / Bearish / Neutral on SPCX today, because <single reason>.*
2. **TL;DR (≤5 bullets).** Only the price-moving items, hardest signal first. Each bullet ends with a parenthetical impact tag: *(catalyst / risk / sentiment / noise)*.
3. **Key metrics (scannable snapshot).** A tight, fixed block so the brief stays decision-grade: SPCX price + daily % move · volume vs. the short post-IPO average · market cap / implied valuation · distance from the $135 IPO price · analyst consensus + any new target · Starlink subs/constellation milestone · next Starship window · X buzz level. Mark anything you couldn't source this run as **unverified**.
4. **What moved today.** The 2–4 developments most likely to affect the stock, each with a one-line "why it matters."
5. **Segment watch.** Two standing leads — a line each every day even if "no change": **Starlink** (the profit engine — subs, cadence, ARPU, direct-to-cell) and **xAI / Grok** (the AI segment — Grok releases/benchmarks, Colossus/compute deals, X, and the AI-segment burn that drives consolidated losses). Then the others only when there's news: Launch/Starship · Starshield/gov · Terafab. Don't pad the quiet ones.
6. **Social & video pulse.** X sentiment summary + top posts (links), then notable YouTube videos (links). Separate **confirmed** from **chatter**.
7. **Catalysts & risks.** The dated events ahead (quiet-period end, lockup expiry, next Starship flight, first earnings, regulatory/contract milestones) and the near-term overhangs to watch.
8. **Sources.** Every claim's link, grouped or inline. A brief without sources is not usable for investing.

Keep it skimmable: the headline + TL;DR must stand alone for a reader who reads nothing else.

## Guardrails — read these, they matter

Investor decisions ride on this output, so accuracy beats completeness every time.

- **Live data only.** Treat your training knowledge as out-of-date. Prices, subs,
  contracts, and valuations must come from this run's searches.
- **Cite or don't claim.** Every number and material fact needs a source you saw
  today. If you can't source it, say so explicitly rather than guessing.
- **Separate fact from rumor.** Label unconfirmed X/forum chatter as such. Don't let
  a viral post become a stated fact.
- **Timestamp everything.** Lead the brief with the generation time and the lookback
  window so the reader knows how fresh it is.
- **Flag the conflict of interest once.** Grok is an xAI product and xAI is now inside
  SPCX. Note this neutrally where xAI/Grok items appear; report them straight.
- **Not financial advice.** Close with a one-line disclaimer. Present signal and let
  the investor decide — no buy/sell calls.

## Daily automation

This skill is built to repeat. Two ways to run it daily:

- **Slash command:** in an interactive session, `/spacex-daily-briefing`.
- **Headless / scheduled:** `grok -p "/spacex-daily-briefing" --output-format text > briefs/$(date +%F).md`
  wired to cron or a CI schedule. Writing each brief into `./briefs/` is what lets
  the next run compute the *delta since yesterday* (step 1). Create that folder if missing.

## Files

- `references/sources-and-queries.md` — canonical sources, the live X/YouTube query set, the entity map with as-of figures, the daily key-metrics checklist, and dated catalysts to verify. Read at the start of every run.
- `references/grok-tool-calls.md` — exact, copy-ready Grok tool-call syntax (`browse_page`, `web_search`, `x_keyword_search`, `x_semantic_search`).
- `assets/brief-template.md` — the fill-in tiered template.

## Version history

- **v1.2** (2026-06-15) — deepened the **xAI / Grok** coverage: AI now a co-lead segment (Grok benchmarks, Colossus, X, and the AI-segment burn that drives consolidated losses); entity reframed as rockets + AI with 2025 revenue/loss anchors; xAI official sources + metrics added.
- **v1.1** (2026-06-15) — multi-source stock (Nasdaq + Yahoo Finance analyst targets), explicit scannable key-metrics block, Starlink as the standing lead segment, catalysts-and-risks framing, `$SPCX` cashtag + higher engagement floor, SEC EDGAR (8-K / Form 4 / lockup) watch.
- **v1.0** (2026-06-15) — initial: tiered price-moving brief, full combined-entity scope (incl. xAI/Grok + X), anti-fabrication/citation rules, delta-since-yesterday logic.
