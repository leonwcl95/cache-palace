# Limitations of this copy

cache-palace is cache-tax 2.3.1 plus a pane. Everything below is about what was added; the original's limits (cache lifetime, pings costing tokens, managed plugins, resume edge cases) still apply, and its README lists them.

## What was checked

- The original's 160 tests pass in this copy after the rename and the additions (`claude plugin test .`).
- `claude plugin validate` passes.
- `/palace` opens the pane in the desktop app, and `/keepwarm` answers as `cache-palace` once the original `cache-tax` is off. Seen by the author once.
- A command that cannot be registered (the original still holds `/keepwarm`) now logs a line and lets the rest of startup run; before that, one clash skipped the whole startup and `/palace` never appeared.
- The startup runs again only when the mod reloads (an edit, or a restart). `/reload-plugins` alone did not register `/palace` after the original was turned off, so after installing, restart the session once.

## What was not checked

- **The pane has no automated tests.** It was written against the same state the original's tests cover, but nothing draws it in a test.
- **The pane has been opened once**, in the desktop app only. The terminal and VS Code layouts have not been seen.
- **The ping history** is kept in memory only (the last 10 pings) and empties with the session or on `/clear`. It is not saved.
- **The refresh timer** redraws the open pane every 30 seconds for 20 minutes after `/palace`, then stops. Reopen `/palace` to restart it. A countdown can be up to 30 seconds old.
- **The buttons** (start, stop, guard) call the same functions as the commands. They have not been pressed in a live session.
- **Installing from the marketplace** has not been tried.

## Sessions and the cap

- **The cap is a change of behavior from cache-tax**, which has none. The default is 5 sessions keeping warm at once; `/keepwarm cap off` removes it.
- **It is not atomic.** The check reads the shared store and then writes, so two sessions arming at the same instant can both pass. Each recounts after writing and before every ping, and ends if it ranks below the cap, so the excess lasts at most until the next recount, not for the whole window.
- **Stopping another session works by removing its keys.** That session notices only when its next ping is due (up to 50 minutes later) and then stops without sending it. Until then its status line still shows a window.
- **`always` brings a stopped session back** at its next turn if a slot is free.
- **A crashed session counts until its window ends**, which can be hours. **Stop** clears it.
- **A lowered cap ends windows at once**, the ones with the least time left, by removing their keys. Those sessions stop at their next ping like any session stopped from the list.
- **The name** shown for each session is its project: the last part of its working directory, the repo for a `.claude/worktrees` checkout, and "No folder" for the desktop app's scratch folder. A name stored by an older version is shortened the same way when shown. A session that armed a window before the names existed shows `(no folder)`.
- **Pings per hour is an estimate** from each session's interval. The other sessions' context sizes are not known, so no cost is shown for them.
- **Tested:** the cap, refusal, `cap off` and bad input, ended windows not counting, and a removed window stopping before its ping , a lowered cap ending the extra windows, and the project-name rules (8 new tests, 168 in all, all passing). **Not tested:** the recount after a simultaneous open, the check before a ping, the Sessions tab itself (search, paging, the cap buttons, the Stop buttons) and the tab switch.

## What changed from cache-tax

- The plugin is named `cache-palace`; the text card is `/cache-palace` (was `/cache-tax`); messages say `cache-palace`. `/keepwarm` keeps its name, which is why the original must be removed first.
- New: `/palace` and the pane (Cache and Sessions tabs), `history` in the session state, a keepwarm cap, a 30-second pane refresh.
- Removed from the repository: the original's website, demo video and screenshots, and its maintainer tooling. The status-bar cube icons are kept.
- Unchanged: the guard, the pings, the pricing table, the store keys' meaning (the store is per plugin, so a window armed in cache-tax is not carried over).
