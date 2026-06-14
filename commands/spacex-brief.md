---
description: Generate today's SpaceX / SPCX investor briefing — live X + web search, price-moving lens, tiered output. Optional argument narrows the focus (e.g. "Starlink", "stock only", "risks").
---

Run the `spacex-daily-briefing` skill and produce today's brief. Optional focus: $ARGUMENTS

Follow the skill's daily workflow exactly: set the window (delta since the last brief in `./briefs/`, else 48h), pull the SPCX quote and analyst targets via web search, run the X scrape (`x_keyword_search` / `x_semantic_search`) for the price-moving pass, scan news and the YouTube hashtag feeds, then synthesize the tiered template — headline, TL;DR, key metrics, what moved, segment watch (Starlink leads), social & video pulse, catalysts & risks, and linked sources.

Honor the guardrails: state only numbers you sourced this run, separate confirmed facts from chatter, cite everything, and close with the not-financial-advice line. If $ARGUMENTS names a focus, weight the brief toward it but keep the key-metrics block and sources intact.
