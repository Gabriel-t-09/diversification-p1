# Diversification Doesn't Fail Slowly

**Project 1 of The Diversification Series**

This project measures how fast the diversification benefit collapses when systemic stress arrives — using algorithmically defined stress episodes, without manual selection by historical name.

The central claim is not that diversification has no effect. The claim is more precise: **the diversification between risk assets collapses rapidly under stress, and this collapse is observable before or within days of the stress signal firing.**

---

## On Correlation as a Measure

This project uses Pearson correlation throughout. This is the industry standard — modern portfolio theory, risk metrics, and the entire diversification framework are built on linear correlation. That choice is followed here for consistency and comparability.

Two limitations of this choice are worth stating before the results, because they bear on how those results should be interpreted.

**Linearity.** Pearson measures linear relationships. Financial markets are not linear — asset relationships are regime-dependent, asymmetric, and exhibit tail dependence that linear correlation does not capture. This means the results presented here are conservative: if linear correlation already collapses in days under stress, the true dependence structure in extreme events is likely more severe. The finding is not surprising given this limitation — it is almost expected. What the project documents is the speed and consistency of that failure across different stress regimes.

**Backward-looking by construction.** Rolling correlation measures how assets behaved over the past N days. Using historical correlation to assume future diversification implicitly assumes regime stability — that assets will continue to relate to each other as they have in the past. This project makes no such assumption and offers no prediction. What it documents is exactly when that assumption fails: in stress. Any practical use of historical correlation to construct a diversified portfolio inherits this assumption. The project shows how quickly it breaks down.

---

## The Finding

In stress episodes defined by a formal drawdown criterion, the average pairwise correlation among risk assets crosses its historical 90th percentile threshold within a **median of 8 trading days across 6 detected episodes**. In several episodes, the collapse had already occurred before the stress signal was detected.

| Episode | Onset | Days to Collapse |
|---|---|---|
| ep_01 (GFC) | 2008-01-22 | 35 days |
| ep_02 (2010) | 2010-06-30 | NOT DETECTED — 23-day episode; post-GFC calibration window had structurally elevated correlation baseline, leaving insufficient room for threshold breach |
| ep_03 (2011 Aug) | 2011-08-08 | 0 days |
| ep_04 (2011 Nov) | 2011-11-17 | 0 days |
| ep_05 (2018) | 2018-12-21 | 18 days |
| ep_06 (COVID) | 2020-03-09 | 0 days |
| ep_07 (2022) | 2022-05-09 | 16 days |

The median is computed over the 6 episodes where collapse was detected (ep_02 excluded). ep_02 is not excluded by choice — the threshold was not breached within the episode window for the structural reason noted above.

**0 days does not mean instantaneous collapse.** It means the correlation collapse had already occurred before the stress onset was detected by the drawdown criterion. When stress becomes algorithmically observable, the diversification between risk assets has already broken down.

A sensitivity analysis across threshold percentiles (p80 through p95) confirms the result is not an artifact of the threshold choice — the median never exceeds 10 days across all tested thresholds (Section 5.6). An absolute correlation threshold analysis (Section 5.6.1) shows that in 4 of 7 episodes, risk asset correlation was already above 0.80 at stress onset.

---

## Episode Detection — Algorithmic, No Manual Selection

Stress episodes are defined entirely by a formal criterion. No episode was selected because it has a famous name.

**Entry condition:**
- SPX cumulative drawdown from rolling 252-day peak exceeds 15%
- AND SPX price is below its 200-day moving average (shifted 1 day)

**Exit condition:**
- SPX price returns above MA200
- AND drawdown has recovered at least 7.5% from the episode trough
  (or crossed above -20% if entry drawdown exceeded 20%)

The algorithm detected **13 episodes since 1980** in Block A and **7 episodes since 2006** in Block B. Some have well-known names (GFC, COVID). Others do not — including December 2018, which had no famous label but met the formal stress criterion.

This design eliminates the circular reasoning of testing whether diversification fails in crises that are famous precisely because diversification failed.

---

## Two-Block Architecture

**Block A — Core assets, long history (1980–2024)**
- Assets: SPX, US Treasury (10Y yield via duration approximation), DXY
- 13 stress episodes detected algorithmically
- Purpose: establish whether the pattern is structural across four decades
- Note: Block A has only one risk asset (SPX). H1 (risk correlation) is not formally testable. Block A contributes to H3 (diversification ratio) and contextual H2 evidence.

**Important qualification on the four-decade claim:** The formal statistical evidence for the central finding is concentrated in Block B (2006–2024, 7 episodes). Block A contributes the DR over time chart (1980–2024) as visual evidence that the pattern is not purely modern — but H1 is not testable in Block A, H2 is contextual only, and H3 is not significant with algorithmic episodes (p=0.187). The claim that this is a structural feature across four decades is supported by visual evidence in Block A, not by formal statistical tests over the full period.

**Block B — Expanded universe, modern period (2006–2024)**
- Assets: SPY, EFA, EEM, TLT, IEF, GLD, DBC
- Risk assets: SPY, EFA, EEM, DBC
- Defensive assets: TLT, IEF, GLD
- 7 stress episodes detected algorithmically
- Purpose: formal hypothesis testing on an investable universe

Episodes with identical onset dates appear in both blocks, enabling direct cross-block comparison (H5).

---

## Hypotheses and Results

**H1 — Risk asset correlation is materially higher in stress than in normal regimes**
- Block B: normal mean = 0.58, stress mean = 0.67. Mann-Whitney p < 0.0001. ✓
- Tested on Block B only — Block A has only 1 risk asset (SPX), making pairwise correlation among risk assets uncomputable.

**H2 — When stress is observable, correlation collapse has already occurred or occurs within days**
- Block B: median 8 days across 6 detected episodes. Maximum: 35 days.
- 3 of the 6 detected episodes had collapse at onset (0 days) — correlation was already elevated when the stress signal fired.
- Result stable across threshold percentiles p80–p95 (Section 5.6).

**H3 — Diversification ratio deteriorates in stress**
- Block A: not significant (p=0.187) with algorithmic episodes.
- Block B: significant (p < 0.0001). Normal DR = 1.71, stress DR = 1.64.
- Key finding: in 4 of the 6 episodes with detected correlation collapse in Block B, the DR was **at or above the normal baseline** at the exact moment risk asset correlation peaked. Note: the denominators in H2 and H3 are both 6 (episodes with detected collapse), but they measure different things — H2 counts episodes where collapse was at onset (0 days), H3 counts episodes where DR was above normal at that collapse moment. These are overlapping but not identical subsets of the same 6 episodes.
- The total portfolio appeared diversified while the risk side was already highly correlated — because defensive assets (TLT, IEF, GLD) were moving against risk assets, inflating the ratio.

**H4 — Breadth failure: proportion of assets with negative returns is higher in stress**
- Block B: p = 0.058 — not significant with algorithmic episodes.
- Result declared as null. Not used as primary evidence.
- Note: H4 was significant with manually selected episodes but is not significant with algorithmically defined episodes. This raises a legitimate question about whether the earlier result reflected the specific characteristics of famous crises rather than a general pattern. The null result with algorithmic episodes is the more credible finding.

**H5 — Universe expansion does not prevent or meaningfully delay collapse**
- H5 is primarily supported by Block B internal evidence: 7 assets collapsed in a median of 8 days across detected episodes. No portfolio can be repositioned in 8 trading days regardless of how many assets it contains.
- The cross-block comparison between Block A and Block B is limited by metric heterogeneity. Block A measures full-universe correlation (SPX + Treasury + DXY); Block B measures risk-asset-only correlation (SPY, EFA, EEM, DBC). These are not directly comparable. The only episode where both blocks produce a valid, comparable result is 2011 (Block A: 1 day, Block B: 0 days) — one episode is insufficient to sustain a cross-block inference.
- GFC comparison (Block A: 8 days, Block B: 35 days) is reported for transparency but reflects metric heterogeneity, not a genuine advantage of the smaller universe.
- Conclusion: the evidence that universe expansion does not help comes from Block B's own internal consistency across 7 episodes — not from formal cross-block comparison.

---

## Key Finding Not in Original Scope

In 4 of the 6 episodes with detected correlation collapse in Block B, the Diversification Ratio was **at or above the normal baseline** at the exact moment risk asset correlation peaked.

The standard portfolio-level DR metric was signaling health while the risk side of the portfolio was already highly correlated. This occurs because defensive assets moving against risk assets inflate the portfolio DR even as risk asset correlations converge.

**The "portfolio acting as a concentrated position" finding applies specifically to the risk asset universe — not to the total portfolio.** Bonds and gold functioned as hedges in most episodes, supporting total portfolio diversification. The argument is not that diversification is absent — it is that the diversification between risk assets, which investors assume exists, collapses rapidly. The hedge provided by defensive assets is a separate mechanism that may not hold in all regimes (as observed in 2022).

**The cost of carrying defensive assets in normal periods versus the actual benefit they deliver in stress is beyond the scope of this project.** That question — whether the hedge is worth the drag — will be addressed in Project 2.

---

## Methodology

**No look-ahead bias.** All rolling thresholds use only data available at time t (252-day rolling window, shifted 1 day).

**Calendar intersection.** No forward-fill or imputation. Analysis restricted to dates where all assets in each block have observed returns.

**Stress onset.** First day of each algorithmically detected episode (entry condition met).

**Correlation collapse.** First day within episode where rolling 20-day average pairwise correlation among risk assets reaches or exceeds its rolling 252-day 90th percentile threshold.

**Diversification ratio.** Equally-weighted average of individual asset volatilities divided by portfolio volatility. Rolling 20-day window. Simple daily returns, non-annualized.

**Statistical tests.** Mann-Whitney U (non-parametric, one-sided). No correction for multiple comparisons — primary results have p < 0.0001. P-values are anti-conservative due to autocorrelation in daily time series. Results treated as descriptive evidence, not classical inference.

---

## Declared Limitations

- Treasury returns in Block A are estimated via duration approximation (r ≈ −8.5 × Δyield / 100), not observed directly.
- Block A has only 1 risk asset (SPX). H1 not applicable. H2 in Block A uses full-universe correlation, which includes defensive pairs and is not directly comparable to Block B H2.
- The LBMA Gold series was permanently removed from FRED in January 2022. Block A uses SPX, Treasury, and DXY.
- Equal weighting throughout. Equal-weighted DR may be higher than market-cap-weighted DR when volatile assets (EEM, DBC) have higher individual volatility. This biases H3 toward harder confirmation.
- Rolling 20-day correlation with 3 asset pairs (Block A) is noisier than with 6 pairs (Block B risk assets).
- Stress onset defined by SPX drawdown. For episodes originating outside US equities, onset may lag actual stress start. This biases H2 toward faster collapse, not slower.
- Algorithmic episodes include stress and recovery phases. H1, H3, H4 measure a broad stress/recovery regime, not pure panic. This is consistent with the detection criterion but affects interpretation of episode averages.
- Correlation is measured using Pearson (linear). See "On Correlation as a Measure" above.
- TLT, IEF, and GLD are treated as a single defensive group, but they have materially different behaviors. TLT has significantly longer duration than IEF and is far more sensitive to interest rate changes — in 2022, TLT fell substantially more than IEF for this reason. GLD has its own distinct dynamics. Treating these three as equivalent "defensive assets" is a simplification that may obscure important differences, particularly in rate-driven stress regimes.
- H3 Block A was significant with manual episodes but is not significant with algorithmic episodes (p=0.187). The algorithmic episodes include periods where DR was elevated due to defensive asset behavior.
- H4 is not significant with algorithmic episodes (p=0.058). Declared as null result. See H4 note above.
- The formal statistical evidence is concentrated in Block B (2006–2024). The four-decade claim relies on visual evidence from Block A.
- H5 cross-block comparison is limited to one valid episode (2011) due to metric heterogeneity between blocks. The H5 conclusion rests on Block B internal evidence.

---

## Data Sources

| Series | Source | Ticker/ID |
|---|---|---|
| US Equities (SPX) | Yahoo Finance | ^GSPC |
| US Equities ETF | Yahoo Finance | SPY |
| International Developed | Yahoo Finance | EFA |
| Emerging Markets | Yahoo Finance | EEM |
| Long Bonds | Yahoo Finance | TLT |
| Intermediate Bonds | Yahoo Finance | IEF |
| Gold | Yahoo Finance | GLD |
| Commodities | Yahoo Finance | DBC |
| US Dollar Index | Yahoo Finance | DX-Y.NYB |
| 10Y Treasury Yield | FRED | DGS10 |

---

## Reproducing the Analysis

```bash
git clone https://github.com/YOUR_USERNAME/diversification-p1
cd diversification-p1
pip install -r requirements.txt
jupyter notebook p1_diversification.ipynb
```

On first run, the notebook downloads data and saves to `data/ret_a.csv` and `data/ret_b.csv`. Subsequent runs load from cache.

---

## Part of The Diversification Series

This project stands alone. Data, methodology, hypotheses, and conclusions are self-contained.

Within the series:
- **Project 1 (this):** The mechanism — risk asset correlation collapses rapidly under stress, regardless of how many risk assets are held.
- **Project 2:** The practical consequence — what this mechanism means for portfolio performance, including the cost of carrying defensive assets in normal periods versus the protection they deliver in stress.
- **Project 3:** The structural explanation — why traditional assets share the same payoff profile under stress.

---

## Requirements

```
yfinance
pandas_datareader
numpy
scipy
matplotlib
jupyter
```
