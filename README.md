# Grok bot template for pump.fun

A Grok bot template: a prompt you paste into Grok once. Then send it any pump.fun contract address, and in a few seconds it tells you:

- the dev's record: launches, graduations, best earlier peak, launches in the last day, how its recent coins went
- the first 10 seconds: how many wallets bought, and how much of the supply they took
- who holds it now: the dev and those first wallets, side wallets included
- how launches that looked like this one went in their first day
- which of your skip rules fire

It's the manual "copy the CA, open the creator, check the dev" routine, done from every pump.fun trade since pump.fun's first day. No API key, no wallet connected, no signup. 30 checks an hour free.

The data comes from [pumpdata.fun](https://pumpdata.fun)'s free calls. The full guide is at **[pumpdata.fun/pump-fun-grok-check](https://pumpdata.fun/pump-fun-grok-check)**.

## Use it

**Grokbot app, Grok app or grok.com:** start a new chat, paste everything in [TEMPLATE.md](TEMPLATE.md), then send a contract address. To make it a standing Grok bot, put the template in a Grok project's instructions: every chat in that project is the bot, and you just send addresses.

**On X:** reply under a coin's post and tag Grok, with the address in place of `<CA>`:

```
@grok check this pump.fun coin: open https://pumpdata.fun/free/launch/<CA> and tell me the dev's launches and graduations, how much of the supply went in the first 10 seconds, and how launches that looked like it went
```

**ChatGPT or Claude:** the same template works. Paste it into a new chat or a project's instructions.

**In code:** the same calls as a Python or TypeScript function are on [pumpdata.fun/pump-fun-trading-agent](https://pumpdata.fun/pump-fun-trading-agent).

## What an answer looks like

$Kimchi (`CSocnzcRpgSCr1naKcNTieWZUxb6rABe45j844m7pump`), Oct 21's busiest coin, checked on Oct 22:

```
DEV: 35 launches, 3 graduated, best earlier peak $7,980.09, 5 launched in the last day. Of its last-90-day launches, 52.4% were dead the next day.
FIRST 10 SECONDS: 2 wallets bought 79.3% of the supply. Insiders (dev + those wallets) hold 74.9% now.
LOOK-ALIKES: 122 launches looked like this one at the same age; 120 of them were dead by day one; their median peak was 1x.
FLAGS:
- NEVER RAN: 35 launches, best earlier peak $7,980.09.
- INSIDERS: 74.9% held by the dev and the first-10-second wallets.
- LOOK-ALIKES DIE: 120 of 122 dead by day one.
- (note) BOUGHT OUT AT LAUNCH: 79.3% in the first 10 seconds, migrated 1 second after the create.
RULES SAY: skip. This is a check of what the chain shows, not advice.
Can't see: who funded the dev wallet, or the dev's X account.
```

It peaked at $27.7M that day anyway. The rules don't predict price. They tell you who you're trading against: here, two wallets that bought 79.3% of the supply in the launch block.

## The rules

They're plain text in the template. Change the numbers to your own.

| Rule | Fires when |
|---|---|
| Serial launcher | under 1,000 launches, and 20+ launches with under 5% graduated, or 10+ launched in the last day |
| Never ran | 5+ launches and best earlier peak under $10,000 |
| Bundled | 50%+ of the supply bought in the first 10 seconds, unless it was bought out at launch |
| Insiders | the dev and the first-10-second wallets hold 50%+ now |
| Look-alikes die | 20+ look-alike launches and 90%+ of them dead by day one |
| Dev's coins die | 5+ of the dev's last-90-day launches measured and 80%+ dead the next day |

Two notes, not flags:
- **Launch service:** 1,000+ launches usually means a launch service or bot account, not one dev.
- **Bought out at launch:** 75%+ bought in the first 10 seconds and migrated within a minute means the whole curve sold in the launch block.

## What it can't see

- who funded the dev wallet
- the dev's X account
- a fresh wallet's history: a dev with one launch proves nothing either way

For a fresh wallet, the first 10 seconds and the look-alike launches are the parts that don't depend on the dev's past.

## Limits

30 free calls an hour per IP under `/free/`. A check uses two. If Grok reports a 429, the hour is used up. For more, the same paths without `/free/` take a free API key (100 an hour, [pumpdata.fun/keys](https://pumpdata.fun/keys)) or pay per call in USDC over x402, with no account.

Numbers from every pump.fun trade, ever. Not advice, and never a verdict on a coin. Not affiliated with pump.fun.

MIT licensed.
