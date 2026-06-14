# spacex-daily-briefing

A daily, investor-grade briefing on **SpaceX** — now public as **SPCX** after the June 12, 2026 IPO, and merged with xAI.

Built for Grok Build (native **X Search** + **Web Search**) and any Claude-compatible agent. It pulls live data, applies a **price-moving-signal** lens, and returns a tiered brief — not a wall of news.

- Skill: `skills/spacex-daily-briefing/SKILL.md`
- Command: `/spacex-brief`
- References: live source list, the X/YouTube query set, the entity map, and dated catalysts in `skills/spacex-daily-briefing/references/`

**Scope (post-merger):** launch & Starship, Starlink, Starshield, Dragon, the Terafab chip effort, plus xAI/Grok and X.

**Output:** one-line read (≤18 words) → ≤5-bullet TL;DR → scannable key metrics → what moved today → segment watch (Starlink + xAI/Grok lead) → social & video pulse → catalysts & risks → inline sources. Tight enough to read in ~2 minutes; reads yesterday's brief and reports only the delta.

**Sources:** Nasdaq + Yahoo Finance (analyst targets), SEC EDGAR (8-K / Form 4 / lockup), Kalshi (prediction-market odds), X, YouTube, spacex.com, Wikipedia.

**Guardrails:** states only numbers it sourced that run; separates confirmed facts from chatter; never invents a quote. Informational only — **not financial advice**. Discloses that Grok/xAI now sits inside the SPCX entity.

## Install (Grok Build)

Drop the folder into `~/.grok/skills/` (Grok also reads `~/.claude/skills/`), or add this repo from the Marketplace. Then run `/spacex-daily-briefing`. Headless, once a day:

```bash
grok -p "/spacex-daily-briefing" > briefs/$(date +%F).md
```

Built by [Solid State](https://solidstate.cc). Sourced, licensed, pinned. Read the skill before you run it.

MIT.
