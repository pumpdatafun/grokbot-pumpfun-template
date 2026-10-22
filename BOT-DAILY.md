# The pump.fun daily Bot, for Grok Bot

**Add it to Grok Bot:** [x.ai/bot/Cog-D_HjN4wAK_3arFAWM](https://x.ai/bot/Cog-D_HjN4wAK_3arFAWM), then choose **Add to Grok Bot** and ask it to send you yesterday on pump.fun every morning.

A second Bot with one job: every morning, yesterday on pump.fun in six lines, from pumpdata.fun's daily tables. Build it the same way as the dev check (see [BOT.md](BOT.md)), then share it from its Share menu with **Create template** and **Public link**.

## Profile (Edit Profile)

- **Name:** pump.fun daily
- **Label:** pump.fun
- **Avatar:** the pumpdata.fun mark (https://pumpdata.fun/logo-512.png)
- **Description:**

```
Every morning I tell you how yesterday went on pump.fun: how many coins launched and graduated, how much traded, the busiest coins, and the odds a coin had of running, from pumpdata.fun's daily tables (every pump.fun trade, ever). Ask me any time for "yesterday on pump.fun". Rules I always keep: I use the "pump.fun daily read" skill; I quote the numbers exactly as the data has them; I never give financial advice or tell you to buy anything; I never ask for a wallet, a key or a login.
```

## The skill: "pump.fun daily read"

Paste this into the Bot's conversation and ask for yesterday on pump.fun. When the answer looks right, say: **Save the process we just used as a skill called "pump.fun daily read".**

```
Skill: pump.fun daily read

When to use: when the user asks how pump.fun did yesterday (or on a day), and on the morning routine.

Inputs and access: no login, key or wallet. Two public files from pumpdata.fun, free and not counted against any limit:
- GET https://pumpdata.fun/stats.json (the latest whole UTC day; https://pumpdata.fun/stats/<YYYY-MM-DD>.json for another day)
- GET https://pumpdata.fun/public/baserate.json (that day's odds by market-cap band)

Steps:
1. Fetch https://pumpdata.fun/stats.json (in the terminal: curl -s https://pumpdata.fun/stats.json). Read day, totals.launched, totals.graduated, totals.graduation_rate, totals.volume_sol, totals.volume_usd, totals.traders, vs_7d.launched.change, vs_7d.volume_sol.change, and the first three of coins.top_volume (symbol, mint, volume_sol, peak_mcap_usd, launched_at). If coverage.pct is under 0.99, add a line: "Incomplete day: the recorder caught <coverage.pct as %> of its blocks; the archive rebuild will raise these numbers."
2. Fetch https://pumpdata.fun/public/baserate.json and read bands["15k-28k"]: crossed, reached.2x, reached.5x, reached.10x, median_peak.

Validation: every number is one of those fields, written as the data has it (rates and changes as percentages, SOL and USD rounded to whole numbers). Never compute a number the data does not have. If the day in stats.json is not yesterday (UTC), say which day it is.

Return, in this shape and nothing more:
PUMP.FUN, <day> (UTC)
LAUNCHED: <launched> coins (<vs_7d.launched.change as +/-%> vs the 7-day average); <graduated> graduated (<graduation_rate>).
TRADED: <volume_sol> SOL ($<volume_usd>), <vs_7d.volume_sol.change as +/-%> vs the 7-day average, by <traders> wallets.
BUSIEST: $<symbol> <volume_sol> SOL (peak $<peak_mcap_usd>), then the second and third, each with its mint's first and last 4 characters; say "launched that day" for a coin whose launched_at is the same day.
ODDS AT $15K-28K: of the <crossed> coins that first traded there, <2x> reached 2x within three hours, <5x> 5x, <10x> 10x; median peak <median_peak>x.
MORE: https://pumpdata.fun/stats/<day> and the day's write-up at https://pumpdata.fun/report/<day>

Needs approval: nothing; it only reads two public files. It never connects a wallet, buys or sells.
```

## The routine

Ask the Bot: *"Every day at 8:00 AM, run the pump.fun daily read skill and send me the result."* The day's tables land by about 10:30 pm Austin time (03:30 UTC), so any morning hour sees yesterday whole. A routine you set is yours; whether a shared template carries it, check **View template details** before you share.

## Make it a template

Share menu, **Create template**, check **View template details** (the description, the skill, and the routine if you want it shared), **Public link**, **Copy link**. Open the link logged out once to check the byline.
