# cache-palace

Keeps Claude Code's one-hour prompt cache warm during breaks, stops a cold send once with its estimated rewrite price, and opens a pane that shows the cache's state at a glance. A cache is the "mind palace" of a session: everything Claude has already read, stored so it does not have to be learned again.

**This is a modified copy of [cache-tax](https://github.com/karanb192/claude-code-mods) by Karan Bansal**, used under its MIT license (see [LICENSE](LICENSE)). The warming, the guard, the pricing and the tests are his. What is added here is the `/palace` pane and a short ping history that feeds it. The plugin, its commands and its messages were renamed so it does not clash with the original.

## What it does

- **Keepwarm.** `/keepwarm` arms a window. After 50 idle minutes a timer sends one tool-less fork over the session's transcript, which reads the cache and keeps it alive. It stops if a ping reads nothing, or writes a tenth or more of what it read.
- **Guard.** When you come back after the cache went cold and the context is at least 50,000 tokens, your message is dropped once and the price of rewriting is shown. Send again to pay it, or `/clear` and start from a note.
- **The pane.** `/palace` opens a pane with:
  - the cache state (warm with a countdown to cold, cold since when, unknown, or keepwarm stopped, and why),
  - context size, the price of a cold rewrite against a warm turn, and the break-even number of pings,
  - the keepwarm window and next ping, with a button to start or stop it, and a button to switch the guard between "refuse once" and "warn only",
  - the last pings: when, tokens read, tokens written, cost, and whether the cache was warm,
  - the cold writes paid this session, with tokens and cost.
- **The row above the prompt**, while keepwarm is armed, and the `/cache-palace` text card, work as in cache-tax.

## Commands

    /palace                     open the pane
    /keepwarm                   keep warm for six hours
    /keepwarm 90m               a window of your own (also 2h30m, 6h)
    /keepwarm always            keep every session warm until six hours after its last turn
    /keepwarm status            the warming window, next ping and last receipt
    /keepwarm off               stop, and turn always off
    /cache-palace               the text card
    /cache-palace guard warn    show the price and send
    /cache-palace guard refuse  drop a cold send once (the default)

## Install

Uninstall or turn off the original `cache-tax` first. Both register `/keepwarm` and both guard cold sends, so running them together means two refusals and a command clash. Run `/keepwarm off` before removing it so no stray ping fires.

From a terminal session:

```
/plugin install cache-palace --marketplace leonwcl95/cache-palace
```

From a shell:

```
claude plugin install cache-palace --marketplace leonwcl95/cache-palace
```

In the desktop app: click **+** next to the prompt box, then **Plugins**, **Add marketplace** with `leonwcl95/cache-palace`, then **Add plugin**.

Then run `/reload-plugins`. It needs Claude Code 2.1.287 or later, a one-hour cache and Claude Code left running. Subscription usage defaults to a one-hour cache. API-key, usage-credit and cloud-provider sessions default to five minutes, so set `promptCacheTtl` to `"1h"` for those, because a 50-minute timer cannot protect a five-minute cache.

## Read this first

- **Pings cost tokens.** Each one re-reads the whole context at the cache-read rate and bills whatever the model answers. How a cache read counts against subscription limits is not documented. Watch your plan percentage during the first window.
- **Warm or cold is an estimate.** It comes from a one-hour clock, not from a check of the server's cache.
- **Dollar figures are API list prices.** On a subscription they are a yardstick, not your bill.
- The limits listed in the original's README apply here too. See [LIMITATIONS.md](LIMITATIONS.md) for what is and is not tested in this copy.

## Credit

cache-tax, Copyright (c) 2026 Karan Bansal, MIT. Changes in this copy, Copyright (c) 2026 leonwcl95, MIT.

## License

[MIT](LICENSE)
