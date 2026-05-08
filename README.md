# quant-notebooks

I started this repo to learn how trend-following and risk-control strategies actually behave on real data. Six notebooks in, the lesson I keep relearning is that the math is the easy part. The hard part is figuring out which assumptions you were unknowingly making.

## The notebooks

| # | Notebook | What it does |
|---|----------|--------------|
| 01 | [SMA 50/200 crossover on SPY](01_sma_crossover_spy.ipynb) | Vanilla golden-cross / death-cross backtest against buy-and-hold |
| 02 | [Transaction costs + multi-asset sweep](02_transaction_costs_multi_asset.ipynb) | Same strategy across 8 ETFs at 0/5/10/25 bps cost — checks whether the edge survives realistic frictions |
| 03 | [Volatility-targeted sizing on SPY](03_vol_targeted_sizing.ipynb) | Scale exposure inversely with realized vol to hit a constant 15% target. Beats buy-and-hold on every metric |
| 04 | [Vol-targeted SMA, multi-asset](04_vol_targeted_sma_multi_asset.ipynb) | Compare four variants per asset. The "obvious" combination of trend filter + vol targeting underperforms pure vol targeting on 4 of 5 tickers |
| 05 | [Diversified portfolio of 8 vol-targeted equity ETFs](05_diversified_vol_target_portfolio.ipynb) | Diversification math says we should be combining 8 risk streams. We aren't really — mean correlation is 0.71 and we capture 11% of the theoretical benefit |
| 06 | [Cross-asset (SPY/TLT/GLD/DBC)](06_cross_asset_diversification.ipynb) | Genuine cross-asset basket fixes the correlation problem (0.07 average) but Sharpe still drops, because diluting with low-Sharpe assets and a 2022 bond-equity crash both hurt |

## What I actually learned

When you ship the first naive backtest (notebook 01), you write the `pct_change()` and the rolling means and read off the result. Then you read it again and realize you let tomorrow's signal trade today's bar. Shifting positions by one day is the kind of correction that separates a lab toy from anything you would put a dollar behind. Until I actually broke that and re-ran, "lookahead bias" was a phrase from a textbook. After I ran it both ways, it was an instinct.

I expected vol targeting to be a small improvement on buy-and-hold. It is the largest improvement in the repo. Across 16 years of SPY data, scaling exposure to a 15% vol target beats buy-and-hold on CAGR, Sharpe, and drawdown. That surprised me. The story is that SPY's variance-of-variance is positively skewed: vol clusters, and trimming exposure exactly when vol clusters mechanically harvests a small premium. No prediction, no trend timing — just risk control.

Then I did the natural next thing: add the trend filter on top of the vol target. Notebook 04 was supposed to show "best of both." It didn't. Stacking the SMA filter onto the vol-targeted size makes the strategy doubly conservative. It sits in cash through long bull regimes and gives back the CAGR you got from sizing. Adding a layer is not the same as adding alpha. I had to be honest about that result in writing before I really believed it.

Notebook 05 walked into a similar trap with diversification. Eight ETFs sounds like eight names. The correlation matrix says they are essentially one name dressed up eight ways. The diversification ratio is 1.21 against a theoretical max of 2.83. We are using barely a tenth of the diversification we are paying for in complexity.

Notebook 06 was the obvious fix — bring in actual different asset classes. Bonds, gold, commodities. The correlation collapses to 0.07. The diversification ratio jumps to 1.79. The math finally works. And the Sharpe is *worse than buy-and-hold SPY*. Because vol-diversification isn't return-diversification: TLT and DBC have lower individual Sharpe than SPY over this period, and the SPY/TLT negative correlation that the "60/40" thesis depends on flipped positive when inflation came back in 2022. The hedge breaks exactly when you need it.

The thread connecting all six is that every "obvious next step" produces a result that is mathematically correct and economically wrong. Real research is mostly that.

## Setup

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter notebook
```

## Conventions

- Numbered prefix per notebook (`NN_topic.ipynb`)
- All data pulled live from yfinance — no checked-in datasets
- Lookahead bias avoided by shifting positions one bar after the signal
- Costs explicit per notebook; default is 5 bps one-way on each rebalance
