# Strategy lab — pre-registration

*Simulated money only. Not investment advice.*

This file pre-registers every strategy of the lab **before** its results exist. The commit that adds a strategy here is its timestamped proof of registration (GitHub records the date; the history cannot be rewritten without trace).

## Protocol

- Each strategy is a fixed piece of code plus fixed parameters, **taken as published in the literature, not optimized**.
- Its SHA-256 fingerprint covers its name, parameters, its own code and the shared simulator files. Any change gives a **new version** with a new start date; the old version is archived, never rewritten.
- **Forward test**: the public record of a version starts with the first full daily candle after its registration (first decision at the open of that day). Every night (00:30 UTC) the whole record is recomputed deterministically from that date (same code + same data = same numbers) and published at about 00:45 UTC.
- Common simulator: $1,000 per strategy, Binance Futures perpetuals on 8 markets (BTC, ETH, SOL, BNB, XRP, DOGE, ADA, AVAX), 0.05% fee per order, slippage, real funding rates, stops and targets checked on hourly candles, one position per market, decisions on the daily close only (no candle after the decision is ever read).
- **The ranking uses the forward test only.** Each strategy also has a historical backtest (2023-12 → the day before its registration, same simulator), published separately and labelled *backtest (not live)*: it proves nothing, it only shows what the rule would have done on data that was known when it was chosen.
- Results under ~6 months are mostly noise.

## Strategies

### 1. Pilot v1 (live rules) — `v1-books` v1

The live pilot's rulebook v1: the 4 book methods (D > A > C > B), 0.75% risk per position, 5% total open-risk cap, one position per market. Should match the live account up to small execution differences.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": false, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `a0769463b55178576794ff9fdab2de092cf6ed7cbf8cfe6e43fa3e2d60c9be4f`

### 2. Pilot v1, methods D+C only — `v1-dc` v1

Pilot v1 restricted to methods D (Tharp 3R swing) and C (Tharp channel breakout). Pre-registered hypothesis #65.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007); hypothesis from PLAN §12.6 bis (observed in backtest, so it must be judged on new data)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": false, "methodes": ["D", "C"], "params": {"C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `babecc693125d3438b75be9420e0dced570ac0360b7344f271585ca84c9f8f8e`

### 3. Pilot v1, longs only — `v1-long` v1

Pilot v1, but short signals are ignored.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": true, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `0163f0c6449c44071dc1c8f7e6217501d474f7dbd30faecf1d76f341c663bffe`

### 4. Pilot v1, D+C longs only — `v1-dc-long` v1

Pilot v1 restricted to methods D and C, long signals only.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": true, "methodes": ["D", "C"], "params": {"C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `46e3843a2de03fa3832a5de2edc77f88e622d1d098483292ea8122283375a2ad`

### 5. Pilot v1, max 3 same side — `v1-max3` v1

Pilot v1 with at most 3 open positions in the same direction (crypto markets are highly correlated).

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007); correlation limit in the spirit of the Turtle rules (C. Faith, 'Way of the Turtle', 2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": false, "max_meme_sens": 3, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `9ca119602be53ba0a6c2d28d70672b92d2c82fad5491b1f4985883a4d0b47a0e`

### 6. Pilot v1, BTC 200-day regime — `v1-regime` v1

Pilot v1; new longs only when BTC closes above its 200-day moving average, new shorts only when it closes below.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007); 200-day regime filter: M. Faber, 'A Quantitative Approach to Tactical Asset Allocation' (2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "longs_seulement": false, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "deux_sens", "risque_pct": 0.75}`
- SHA-256: `905248a4a2fa44f29b1a3008a0ae0e6b581544179fb0ce949b8de19eb5a543d1`

### 7. Pilot v1, funding filter — `v1-funding` v1

Pilot v1; no new long when the market's 3-day average funding rate is above +0.03% per 8 h, no new short when it is below -0.03% (avoid crowded trades).

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007); funding as a crowding signal: Binance Futures funding rates (baseline 0.01% per 8 h)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "filtre_funding": 0.0003, "longs_seulement": false, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `137632a5eebfa67732a5475c66a2229a01060408103256e2cde9a51efdc8cbb7`

### 8. Pilot v1, half risk in high DVOL — `v1-fear` v1

Pilot v1; risk per position is halved when BTC implied volatility (Deribit DVOL) is above its 90th percentile of the past year.

- Source: Pilot rulebook v1 (PLAN §12.2): rules from A. Clenow, 'Following the Trend' (2013), Kabbaj, 'L'art du trading' (Eyrolles) and V. K. Tharp, 'Trade Your Way to Financial Freedom' (2007); implied volatility index: Deribit DVOL; risk scaling as in A. Clenow, 'Following the Trend' (2013)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fenetre_c": 20, "filtre_dvol": {"centile": 90, "jours": 365, "min_jours": 300, "mult": 0.5}, "longs_seulement": false, "methodes": ["D", "A", "C", "B"], "params": {"A": {"close_trail_atr": 3.0, "exit_window": 25, "stop_atr": 3.0, "trail_atr": 4.0}, "B": {"rr": 3.0, "stop_atr": 1.5}, "C": {"adverse_atr": 2.0, "stop_atr": 3.0, "trail_atr": 3.0}, "D": {"rr": 3.0, "stop_atr": 1.5, "time_stop_days": 0}}, "plafond_pct": 5.0, "regime": "aucun", "risque_pct": 0.75}`
- SHA-256: `3f74d32000526359845d9e5270478db1085fbd6eb33e5bac379a873817408996`

### 9. BTC buy & hold — `btc-hold` v1

Buy BTC perpetual once with 100% of the capital and hold it (funding paid or received like any position).

- Source: Standard benchmark
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"symbol": "BTCUSDT", "weight": 1.0}`
- SHA-256: `06553c1ad0a466f017431e0fac5b0fd1f7413c77e4b821b4002a980fb7b38103`

### 10. Equal-weight basket of 8 — `basket-8` v1

1/8 of the capital in each of the 8 markets, rebalanced to equal weights on the first day of each month.

- Source: Equal-weight (1/N) allocation: V. DeMiguel, L. Garlappi & R. Uppal, Review of Financial Studies (2009)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{}`
- SHA-256: `26866e01d920da391a0e32fbabc2ca5f7bcec99b769205d7d4c7a70163b12396`

### 11. BTC 200-day MA filter — `btc-ma200` v1

Hold BTC while its daily close is above its 200-day simple moving average, cash otherwise.

- Source: M. Faber, 'A Quantitative Approach to Tactical Asset Allocation', Journal of Wealth Management (2007)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"ma": 200, "symbol": "BTCUSDT"}`
- SHA-256: `86a6d98c14da24d10129479dc9a66bc809350f8b3da3b2c38fc246e5fa47f542`

### 12. Golden cross 50/200 — `golden-cross` v1

On each of the 8 markets: long (1/8 of the capital) while the 50-day moving average is above the 200-day one, flat otherwise. Long only.

- Source: Moving-average crossover rules: W. Brock, J. Lakonishok & B. LeBaron, Journal of Finance (1992); J. Murphy, 'Technical Analysis of the Financial Markets' (1999)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"fast": 50, "slow": 200}`
- SHA-256: `64efeaf214076765d341821a4df94b833b24c4bb6d1b39195d7b8ea5c9364d2c`

### 13. Turtle System 1 (20/10) — `turtle-s1` v1

Long on a close above the 20-day high, short on a close below the 20-day low; exit on the 10-day opposite channel; stop at 2 N (N = 20-day ATR); 1 unit = 1% of equity per N. Skips a breakout if the previous one was a winner (unless it is also a 55-day breakout). Simplified: entries at the next open, one unit, no pyramiding.

- Source: C. Faith, 'Way of the Turtle' (2007) and 'The Original Turtle Trading Rules' (2003)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"entry": 20, "exit": 10, "failsafe": 55, "n": 20, "risk_pct_per_n": 1.0, "skip_after_win": true, "stop_n": 2.0}`
- SHA-256: `11c19fa2a611f591113a308c017a713c8168d123da954516da5f99bbf3c2810c`

### 14. Turtle System 2 (55/20) — `turtle-s2` v1

Long on a close above the 55-day high, short below the 55-day low; exit on the 20-day opposite channel; stop at 2 N; 1% of equity per N. Same simplifications as System 1.

- Source: C. Faith, 'Way of the Turtle' (2007) and 'The Original Turtle Trading Rules' (2003)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"entry": 55, "exit": 20, "failsafe": null, "n": 20, "risk_pct_per_n": 1.0, "skip_after_win": false, "stop_n": 2.0}`
- SHA-256: `a683b6e7a7a02775796494d46f14c56d95307deecc3a68d6fbb079b0d168107a`

### 15. Time-series momentum — `tsmom` v1

Each month, on each of the 8 markets: long if past returns are positive (average of the 1-, 3- and 12-month signs), cash otherwise, each position scaled to 40% annualized volatility (capped at 1x), 1/8 of the capital per market.

- Source: T. Moskowitz, Y. H. Ooi & L. H. Pedersen, 'Time Series Momentum', Journal of Financial Economics (2012); long-only, multi-horizon variant
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"lookbacks": [30, 91, 365], "max_leverage": 1.0, "vol_com": 60, "vol_target": 0.4}`
- SHA-256: `abbb8ee0c97e9099bc4ee4512d98d12c3aa7815b44df1f6d10fd203fee1497a3`

### 16. Cross-sectional momentum — `xs-momentum` v1

Every Monday: long the 2 best of the 8 markets over the past 4 weeks (50% each), only while BTC is above its 200-day moving average; cash otherwise.

- Source: N. Jegadeesh & S. Titman, Journal of Finance (1993); for crypto: Y. Liu, A. Tsyvinski & X. Wu, 'Common Risk Factors in Cryptocurrency', Journal of Finance (2022)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"band": 0.02, "lookback": 28, "ma": 200, "top": 2, "weekday": 0}`
- SHA-256: `d434ae878319ed67e59378ab05279c15f293058e14ba79210714b55a6f7437c1`

### 17. Connors RSI(2) — `rsi2` v1

On each market: buy (1/8 of the capital) when the 2-day RSI is below 10 and the close is above the 200-day moving average; sell when the close is above the 5-day moving average. Long only, no stop.

- Source: L. Connors & C. Alvarez, 'Short Term Trading Strategies That Work' (2008)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"entry_below": 10, "exit_ma": 5, "rsi_n": 2, "trend_ma": 200}`
- SHA-256: `33f506a7a606a67aeede855b1d9b3f2805d19b394d275a4be86d592e3bd9bfe7`

### 18. Bollinger mean reversion — `bollinger-mr` v1

On each market: buy (1/8 of the capital) on a close below the lower Bollinger band (20 days, 2 standard deviations) while the close is above the 200-day moving average; sell at the middle band. Long only, no stop.

- Source: J. Bollinger, 'Bollinger on Bollinger Bands' (2001)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"k": 2.0, "n": 20, "trend_ma": 200}`
- SHA-256: `f569bf3caca0c4c7e6e0c555f44a3c4d1366e4806471240a80c9dcb4d20a4c09`

### 19. Fear & Greed contrarian — `fear-greed` v1

Buy BTC (100%) when the Crypto Fear & Greed Index is below 20 (extreme fear), sell when it is above 80 (extreme greed).

- Source: alternative.me Crypto Fear & Greed Index; contrarian rule 'be fearful when others are greedy' (W. Buffett, 1986 letter)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"buy_below": 20, "sell_above": 80, "symbol": "BTCUSDT"}`
- SHA-256: `584f89257ff20df385b6ea8bed58b880546e437ed3b838680bab85bdd9ccdf90`

### 20. BTC volatility targeting — `btc-voltarget` v1

BTC exposure = 40% / realized 30-day annualized volatility, capped at 100%; rebalanced daily when the exposure is more than 5 points away from target.

- Source: A. Moreira & T. Muir, 'Volatility-Managed Portfolios', Journal of Finance (2017); C. Harvey et al., 'The Impact of Volatility Targeting', Journal of Portfolio Management (2018)
- Registered: 2026-10-14T21:22:44Z · forward test from: 2026-10-15T00:00:00Z
- Parameters: `{"band": 0.05, "max_weight": 1.0, "symbol": "BTCUSDT", "target": 0.4, "window": 30}`
- SHA-256: `52ec03ed8a8f1dc33803af32982b5f98200a836fb2fdebd97cb0279fb4405a4b`

