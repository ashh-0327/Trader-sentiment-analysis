# Trader Performance vs Market Sentiment — Write-up

## Methodology

**Data.** 211,224 Hyperliquid trades across 32 accounts (May 2023 – May
2025), joined to the Bitcoin Fear/Greed index on date. Both source files
were clean going in — zero nulls, zero duplicate rows — so no imputation
was needed. The trade data only overlaps the sentiment index for its 2023
onward range, so the merge is an inner join on date; this drops the
2018–2023 portion of the sentiment file, which isn't relevant here.

**Grain.** The sentiment index has one value per day, but a single
account can place 50+ trades a day. So every trade is first rolled up to
one row per **(account, day)** — daily PnL, trade count, win rate, average
trade size, long/short ratio — and *then* joined to that day's sentiment
label. That's the level every comparison below runs at.

**Fear vs Greed.** The real index has 5 classes (Extreme Fear → Extreme
Greed). Collapsing "Neutral" into either side would bias the comparison,
so it's kept as a third bucket rather than forced into Fear or Greed.

**No leverage field.** The brief mentions leverage, but it isn't in the
data. Average trade size is used as a stand-in for risk appetite in the
segmentation section — flagged here rather than silently assumed.

**Statistics.** Fear/Greed comparisons use a Mann-Whitney U test (PnL is
heavily right-skewed by a handful of large trades, so a t-test's
normality assumption doesn't hold). Every comparison is also checked at
the per-account level (32 accounts, each counted once) alongside the
account-day level (2,340 rows), since a few very active accounts could
otherwise dominate a naive average.

## Key findings

**1. Traders flip their long/short bias almost completely between Fear and Greed — and it's contrarian.**
On Fear days, ~83% of new positions opened are longs; on Greed days that
drops to ~37% (i.e. mostly shorts). This is highly significant
(p < 0.0001) and holds at both the account-day and per-account level.
These traders are, on average, buying into fear and shorting into greed —
the opposite of what "greed = more longs" intuition would predict.
See `outputs/charts/performance_and_behavior.png`.

**2. Drawdowns are significantly *worse* on Greed days, not Fear days.**
Using a running-peak drawdown proxy on cumulative realized PnL, the
average drawdown per account is -$7,259 on Fear days vs. **-$11,852 on
Greed days** (p = 0.002). This cuts against the common assumption that
fear is when things go wrong — for this group, it's euphoria that
precedes the bigger give-back. See
`outputs/charts/performance_and_behavior.png`.

**3. Trade frequency rises during Fear, and PnL is directionally (not conclusively) higher too.**
Accounts place a median 31 trades/day on Fear days vs. 28 on Greed days
(p = 0.039). Median daily PnL is also higher on Fear ($122.74) than Greed
($265.25 — note: Greed is actually higher on the median-account-day view,
but the *per-account average* flips the other way, $9,495 vs $6,023).
The overall PnL difference lands at p = 0.062 — just outside the
conventional 0.05 cutoff, so this is a lean, not a proven edge. Win rate
shows no real difference (p = 0.264).

**4. Segments don't respond uniformly — activity level matters more than "risk size."**
Frequent traders (by total trade count) earned far more in total PnL than
infrequent ones ($496.5k vs $144.4k average) — though this is partly a
volume-of-opportunity effect, not proof of higher skill per trade.
Large-size vs. small-size traders performed about the same ($305k vs
$335k). Somewhat counterintuitively, the "Inconsistent" segment
(higher day-to-day win-rate volatility) slightly out-earned the
"Consistent" segment ($364.9k vs $276.0k) — in this sample, steadier
win rates didn't translate to more total profit. See
`outputs/charts/segments.png`.

**5. Bonus — three rough behavioral archetypes emerge from clustering (n=32, illustrative only).**
KMeans on log-scaled trade count/size, win rate, long bias, and PnL
volatility separates: a **high-frequency, small-size, low-volatility**
group (12 accounts); a **larger-size, long-biased, higher win-rate**
group (14 accounts); and a small **high-volatility, short-biased** group
(6 accounts) whose PnL swings are far larger than the other two. With
only 32 points this is a description of these traders, not a claim about
trader types generally. See `outputs/charts/trader_archetypes.png`.

## Strategy recommendations

**Rule 1 — Tighten risk on Greed days, not Fear days.**
The data says the bigger give-back historically happens during Greed
(deeper drawdown proxy, p = 0.002), which is the opposite of where most
risk-management instinct points. A concrete rule: cap position size or
max concurrent exposure more aggressively when the index reads Greed/
Extreme Greed, and relax that cap somewhat during Fear, rather than the
default of "de-risk when the market looks scared."

**Rule 2 — Let the frequent-trading, contrarian-long behavior during Fear continue, but only for accounts that already show it.**
Traders already increase trade frequency and flip long-biased during Fear
days, and that lean shows a (directionally positive, not yet
statistically airtight) PnL edge. Rather than a blanket instruction to
"trade more in fear," the rule is segment-specific: for accounts in the
frequent-trader / historically-long-biased cluster, keep leaning into
that pattern during Fear regimes; for infrequent or short-biased
accounts, this edge hasn't been demonstrated and shouldn't be assumed to
transfer.

## Honest caveats

- 32 accounts is a small sample for any segment- or cluster-level claim;
  results describe this dataset, not traders in general.
- The PnL-by-sentiment result (finding #3) sits right at the edge of
  conventional significance — worth monitoring with more data, not
  treating as settled.
- "Drawdown proxy" is a running-peak measure on realized PnL, not a true
  percentage drawdown against account equity (equity/margin data isn't
  in this file).
