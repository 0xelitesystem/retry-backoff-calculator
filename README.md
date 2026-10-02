# Retry Backoff Calculator

Visualize exponential backoff with jitter for API retry logic: per-attempt delay table, worst-case wait, bar chart, and a copyable JavaScript snippet.

**Live demo:** https://0xelitesystem.github.io/retry-backoff-calculator/

## Use

1. Set max attempts, base delay, multiplier and max delay cap.
2. Pick a jitter strategy: none, full, equal or decorrelated.
3. Read the per-attempt delay table, the summary tiles and the sleep-range chart. They update on every change.
4. Click Copy to take the JavaScript snippet built from your exact numbers.

## Features

- Inputs for max attempts (1 to 20), base delay, multiplier, max delay cap, and jitter strategy (none, full, equal, decorrelated)
- Per-attempt table with min sleep, expected (mean) sleep, max sleep, and cumulative worst-case wait
- Summary tiles: total worst-case wait, total expected wait, and final attempt max delay, formatted human-readable alongside raw milliseconds
- Horizontal bar chart of each attempt's sleep range, with the guaranteed minimum shown as a solid segment and the jitter range striped
- Copyable plain-JavaScript snippet implementing the currently selected strategy with your exact numbers
- Plain-language explainer covering retry storms, the thundering herd, delay caps, and why retries are only safe on idempotent operations
- Live recalculation on every input change, with sensible validation and clamping
- Dark and light themes with the choice persisted locally
- Single HTML file, no external dependencies

Pairs with [idempotency-and-safe-retries-reference](https://github.com/0xelitesystem/idempotency-and-safe-retries-reference) in the same portfolio, which covers how to make operations safe to retry in the first place.

This page computes the delay schedule for a single client. It deliberately models no feedback: the delays it prints are the same whether the dependency is healthy or saturated. For the other half of the problem, where retries from many clients raise the failure rate that causes the retries, see [metastable-failure-explorer](https://0xelitesystem.github.io/metastable-failure-explorer/), which solves the amplification fixed point and shows why a system can stay collapsed after load returns to normal.

## How it works

The raw delay for attempt n is `min(cap, base * multiplier^(n-1))`. The jitter strategy then decides the actual sleep: none uses the raw delay as-is, full jitter picks a random value between 0 and raw, equal jitter picks `raw/2` plus a random value up to `raw/2`, and decorrelated jitter picks `min(cap, random(base, prev_sleep * 3))` seeded with the base delay. The table and chart show the analytic min, max, and mean for each attempt (the decorrelated mean is the mean of the capped recursion, an approximation), and the worst-case total assumes every attempt sleeps its maximum. Everything is computed in plain JavaScript in the page.

## Why this exists

Backoff settings are easy to pick by feel, and the total wait only shows up once a dependency is down. This page shows the whole retry schedule before you ship it. It is one HTML file with no tracking and no network calls. MIT licensed.

## Privacy

Everything runs in your browser. Nothing is uploaded or sent anywhere; there are no analytics, no cookies, and no network requests. The only thing stored locally is your theme preference.

## Run locally

```bash
git clone https://github.com/0xelitesystem/retry-backoff-calculator
cd retry-backoff-calculator
```

Open `index.html` in any browser. Or serve the folder with `python -m http.server 8000` and visit http://localhost:8000/.

## Build

No build step. The whole tool is one `index.html` file with its CSS and JavaScript inline.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT
