# quant-notebooks

Public notebooks exploring quantitative finance ideas. Each notebook is self-contained: data download, transformation, backtest, metrics, plots.

## Notebooks

| # | Notebook | Topic |
|---|----------|-------|
| 01 | [SMA 50/200 Crossover on SPY](01_sma_crossover_spy.ipynb) | Classic golden-cross / death-cross trend strategy backtested against buy-and-hold |
| 02 | [Transaction costs and multi-asset sweep](02_transaction_costs_multi_asset.ipynb) | SMA 50/200 across SPY, QQQ, IWM and 5 sector ETFs at 0/5/10/25 bps cost levels — Sharpe and drawdown heatmaps |
| 03 | [Volatility-targeted position sizing](03_vol_targeted_sizing.ipynb) | Scale SPY exposure inversely with realized vol to target a constant 15% annualized — beats buy-and-hold on CAGR, Sharpe, and drawdown over 16 years |

## Setup

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter notebook
```

## Conventions

- Numbered prefix per notebook (`NN_topic.ipynb`)
- All data pulled live from free sources (yfinance, FRED, etc.) — no checked-in datasets
- Lookahead bias avoided by shifting position by 1 bar after signal
- No transaction costs unless explicitly modeled (noted in each notebook)
