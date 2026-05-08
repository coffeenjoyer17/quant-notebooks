# quant-notebooks

Public notebooks exploring quantitative finance ideas. Each notebook is self-contained: data download, transformation, backtest, metrics, plots.

## Notebooks

| # | Notebook | Topic |
|---|----------|-------|
| 01 | [SMA 50/200 Crossover on SPY](01_sma_crossover_spy.ipynb) | Classic golden-cross / death-cross trend strategy backtested against buy-and-hold |

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
