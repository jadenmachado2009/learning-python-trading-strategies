# Algorithmic Trading — Python Learning Journal

> Self-directed study in quantitative finance and systematic strategy development.

---

## Vectorised EMA Crossover Backtest

**Date:** 2026-05-20

**Script:** `ema_crossover_backtest.py`

**Asset:** NQ=F (Nasdaq-100 Futures)

**Timeframe:** 5-minute bars

**Lookback:** 60 days

**Stack:** `yfinance` · `pandas` · `numpy` · `matplotlib`

---

### Strategy Logic

- Fast EMA (9) > Slow EMA (21) → Long (signal = 1)
- Fast EMA < Slow EMA → Flat (signal = 0)
- Long-only · no short side · no stop-loss

---

### Key Concepts Learned

**General strategy coding structure**
- Every strategy follows the same pipeline: `imports` → `config` → `data` → `indicators` → `signals` → `returns` → `metrics` → `plot`
- The format each basic line of code follows

**`ewm(span, adjust=False)` — exponential moving average**
- Smaller span reacts faster to price but generates more noise. Larger span is smoother but lags
- `adjust=False` uses the standard EMA formula

- Learnt what key parameters do within each function, such as `span` and `adjust` in `ewm()`, `periods` in `shift()` and `diff()`, and how `cumprod()` compounds returns into an equity curve
  
**Annualised return approximation**
```python
bars_per_year = 252 * 78  # 252 sessions × 78 five-minute bars in a day
annualised_return = df['Strategy_Return'].mean() * bars_per_year
```
- Mean bar return scaled to the number of bars in a trading year — arithmetic approximation

---

### Limitations Identified

| Gap | Impact |
|-----|--------|
| No transaction costs or slippage | Overstates net performance |
| No stop-loss or take-profit | Unrealistic trade management |
| Bar-level P&L only | Win rate and avg R not calculable |
| No Sharpe ratio or max drawdown | Incomplete risk picture |
| 60-day sample window | Insufficient for statistical significance |

---

### Notes
**The goal of this script was not to find an edge.** It was to build and fully understand the core backtest pipeline: data → indicators → signals → returns → equity curve. That objective was met.

---

### Open Questions

- [ ] How do I correctly deduct costs at the trade level rather than the bar level?
- [ ] How does `yfinance` data quality compare to exchange-grade tick data? Is it possible to use institutional level data for free? 
- [ ] What sample size is needed for statistically significant backtest conclusions?
- [ ] How to build more complicated entry models?

---

### Next Build Targets

- [ ] Trade-level metrics: win rate, avg win, avg loss, profit factor
- [ ] Risk metrics: Sharpe ratio, max drawdown, sortino ratio
- [ ] Proper cost model: deduct on entry and exit bars only
- [ ] Regime filter: ATR or volume condition
- [ ] RSI based entry models
