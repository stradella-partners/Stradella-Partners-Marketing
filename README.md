# Stradella Partners — Marketing Site

Password-gated investor teaser for the Hybrid Engine v1.1 strategy.

## Structure

- `index.html` — the entire site. Single self-contained file: all CSS and JS inline.
  The only external dependency is Google Fonts (Archivo).
- `data/performance.json` — live paper-trading data contract. The page fetches this
  on load and re-renders the Live Verification section from it.
- `.nojekyll` — tells GitHub Pages to serve files as-is (no Jekyll processing).

## Live data contract

The Live Verification section renders entirely from data, never hardcoded:

1. An embedded `PERFORMANCE_DATA` object renders immediately on page load.
2. On load the page fetches `./data/performance.json`. If it resolves and parses,
   every live element re-renders from it. If it fails, the embedded object is used
   silently with no error UI.

To update live figures, replace `data/performance.json` and commit. No code changes needed.

### Schema

```json
{
  "as_of": "2026-08-18",
  "capital_base": 100000,
  "ending_capital": 111371.47,
  "total_return_pct": 11.37,
  "max_drawdown_pct": -1.73,
  "profit_factor": 3.80,
  "win_rate_pct": 63.6,
  "sessions_traded": 11,
  "sessions_total": 21,
  "equity_curve": [{ "date": "2026-07-20", "equity": 102381.76 }],
  "trades": [{ "date": "2026-07-20", "symbol": "SPXS", "side": "SHORT",
               "gross_exposure": 399987, "pnl": 2381.76, "ret_on_capital_pct": 2.38 }]
}
```

Derived automatically from the above: payoff ratio, day count, trade count,
equity-chart geometry, gridlines, and the ledger.

## Preview ribbon

`index.html` has a single boolean near the top of the script:

```js
var SHOW_RIBBON = true;
```

`true` shows the "Password-protected preview · not for distribution" ribbon.
Set to `false` to remove it.

## Credentials

**No API keys or broker credentials belong in this repo.** The page never calls
Alpaca directly. Any pipeline that regenerates `performance.json` must run
server-side with `APCA-API-KEY-ID` / `APCA-API-SECRET-KEY` as environment
variables only.

## Compliance notes

Copy in this site is approved language. When editing, preserve:

- The phrase "market neutral" is never used. The strategy holds directional
  intraday positions; approved framings are "no directional bias", "zero
  overnight exposure", "flat by the close every session".
- Backtest figures stay labeled backtest/hypothetical. Paper-trading figures stay
  labeled paper trading. The two are never blended into one track record or chart.
- Annualized live ratios (Sharpe, Calmar) appear only inside the honesty box.
- No forward-looking language.
- The footer disclosure paragraph must appear verbatim.
- Primary figures are on the $100k-capital / 4x live-sizing basis; per-dollar-of-
  exposure figures appear only as labeled secondaries.
