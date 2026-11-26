# ML Signal Research with Leak-Proof Validation

## Research spec

**1. Target.** Direction of SPY's next-day return (binary classification, not price prediction).
- Next-day return: $r_{t+1} = P_{t+1} / P_t - 1$, where $P$ is the adjusted close
- Label on row $t$: $y_t = 1$ if $r_{t+1} > 0$, else $0$
- The last row has no next-day return, so it is dropped (not filled).

**2. Timing and information.** The prediction is made at the close of day $t$ for day $t+1$.
Features may only use data up to and including the close of day $t$. Any feature that touches
day $t+1$ or later is leakage.

**3. Universe and period.** SPY, daily data, [START]–[END], from the Project 1 SQLite database.
Extension to the 7-ETF universe (SPY, QQQ, XLK, XLF, XLE, TLT, GLD) if time allows.

**4. From prediction to position.** Long/flat, fixed before seeing any results:
hold SPY on day $t+1$ if the model's $P(\text{up}) > 0.5$, otherwise hold cash.
Predictions dated $t$ are passed to `run_backtest` unshifted; the engine (from Project 2)
applies the one-day shift, so the position earns exactly the return the label describes.

**5. Success criterion.** The model must beat all three baselines **out-of-sample and after
transaction costs**:
- always long (buy-and-hold)
- yesterday's direction
- 12-month time-series momentum (from Project 2)

Accuracy alone is not a success criterion. Realistic expectation: ~51–53% accuracy,
with little or no edge after costs. Reporting that honestly is a valid result.