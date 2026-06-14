# Grok tool-call reference (exact syntax)

Copy-ready tool calls for Grok Build. Always set `since:` / `from_date` to *yesterday*
so the scrape stays daily-fresh. Raise `min_faves:` when volume is high — chase signal,
not every reply. Run independent calls in parallel.

## browse_page — background & official (not for live quotes)

```xml
<browse_page>
  <url>https://www.spacex.com/</url>
  <instructions>Summarize current/upcoming launch manifest, Starship/Falcon/Dragon status, Starlink constellation milestones, and any investor-relevant updates or company highlights.</instructions>
</browse_page>
```

```xml
<browse_page>
  <url>https://en.wikipedia.org/wiki/SpaceX</url>
  <instructions>Investor-focused summary: segments/subsidiaries and their focus, leadership, post-IPO status, valuation context, and recent developments. List key figures (launches, Starlink subs, revenue) with their stated dates so I can verify freshness.</instructions>
</browse_page>
```

```xml
<browse_page>
  <url>https://www.youtube.com/hashtag/spacexipo</url>
  <instructions>List up to 8-10 recent/high-engagement videos in the feed. For each: title, channel, relative upload time, and watch URL. Prioritize IPO reaction, SPCX price analysis, Starship/Starlink. Do not invent view counts you can't see.</instructions>
</browse_page>
```

> The Nasdaq SPCX page is JS-heavy. `browse_page` it for context only — get the actual
> price number from `web_search` + a finance/news source, or mark it unconfirmed.

Yahoo Finance's analysis page is the cleaner source for **analyst targets/ratings**:

```xml
<browse_page>
  <url>https://finance.yahoo.com/quote/SPCX/analysis</url>
  <instructions>Extract the analyst ratings breakdown (buy/hold/sell counts), average/high/low 12-month price targets, number of covering analysts, and the date of the latest rating change or initiation.</instructions>
</browse_page>
```

## web_search — live quote, news, video fallback

```xml
<web_search><query>SPCX stock price today</query><num_results>6</num_results></web_search>
<web_search><query>SPCX analyst price target OR rating OR initiation</query><num_results>6</num_results></web_search>
<web_search><query>SpaceX OR SPCX OR "SpaceX IPO" OR Starlink OR Starship news</query><num_results>8</num_results></web_search>
<web_search><query>xAI OR Grok OR "X platform" revenue OR deal news</query><num_results>5</num_results></web_search>
<web_search><query>SpaceX IPO OR SPCX site:youtube.com</query><num_results>5</num_results></web_search>
```

## x_keyword_search — the daily scrape (Grok's edge)

```xml
<x_keyword_search>
  <query>#spacex OR #SpaceXIPO OR #SPCX OR "SpaceX IPO" OR "SPCX stock" min_faves:10 filter:has_engagement since:2026-06-14</query>
  <mode>Latest</mode>
  <limit>10</limit>
</x_keyword_search>
```

```xml
<x_keyword_search>
  <query>$SPCX min_faves:10 since:2026-06-14</query>
  <mode>Latest</mode>
  <limit>10</limit>
</x_keyword_search>
```

```xml
<x_keyword_search>
  <query>from:elonmusk (SpaceX OR Starlink OR Starship OR IPO OR SPCX OR xAI OR Grok) since:2026-06-14</query>
  <mode>Latest</mode>
  <limit>5</limit>
</x_keyword_search>
```

## x_semantic_search — sentiment & themes

```xml
<x_semantic_search>
  <query>SpaceX IPO stock reaction, Starlink updates, Starship tests, investor sentiment on SPCX</query>
  <limit>8</limit>
  <from_date>2026-06-14</from_date>
</x_semantic_search>
```

## render_inline_citation

Attach the source to any specific number or material claim in the brief, so the
investor can click through and the figure is auditable.
