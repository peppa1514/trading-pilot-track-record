<!-- genere-par-report.py -->
# Crypto Pilot — public track record

**Live page: https://peppa1514.github.io/trading-pilot-track-record/** · Repository: https://github.com/peppa1514/trading-pilot-track-record · X: [@CryptoPilotBot](https://x.com/CryptoPilotBot)

**What this is.** The daily, public, timestamped track record of a **mechanical** trading system on crypto perpetual futures
(8 Binance Futures markets: BTC, ETH, SOL, BNB, XRP, DOGE, ADA, AVAX), started on **2026-10-09 at 04:20 UTC** with
**$1,000 of simulated money**.

**What this is not.**
- **The money is SIMULATED (paper trading).** No order reaches an exchange. **Prices, fees (0.05%) and funding rates are real**
  (Binance); slippage is simulated.
- **This is not investment advice**, nor an offer, nor a signal to copy. Nothing here promises any profit.

**How it decides.** The rules come from three books: Andreas Clenow (50-day breakout, method A), Rachid Kabbaj (pullback with
reward/risk ≥ 3, method B), Van K. Tharp (channel breakout, method C; 3R swing, method D). Fixed risk of 0.75% of the account per
position, ATR-based stops. The code applies the rules on its own, with no manual exceptions. **No AI decides any trade**: 6 language
models were tested as a "veto" on 300 historical signals and none did better than simply taking every signal
([AI veto study](docs/ai-veto-study.md)). An AI is only used to
propose research hypotheses, which the code tests on historical data and then on a shadow portfolio before any adoption.

**Honesty.** The rules in force are published (`data/resume.json`; each report states the rulebook version). Losses, failures and
technical incidents are published too. Three benchmarks are always shown alongside: BTC buy & hold, 50/50 BTC-ETH, and BTC with a
200-day moving-average filter. In backtests over 2023-12 → 2026-10, this system returned **less** than holding BTC (+16.5% vs +122%)
with a much smaller maximum drawdown (14% vs 53%). Live trading will show what it is really worth.

**Goal.** 12 months of verifiable history, without interruption. The git history (one commit per day, timestamped by GitHub) makes any
rewrite visible. On the 1st of each month, the report publishes the SHA-256 hashes of the previous month's files.

**Files.**
- `data/equity.csv` — account value at each midnight UTC, with the three benchmarks
- `data/trades.csv` — every closed trade, with its result in R (multiples of the initial risk)
- `data/positions.csv` — open positions at report time
- `data/resume.json` — key figures and the rules in force
- `reports/YYYY-MM-DD.md` — daily report

Published automatically every day at about 00:45 UTC. Column names in the data files are in French (e.g. `compte_usd` = account value,
`btc_conserve_usd` = BTC buy & hold, `methode` = method, `sens` = side, `autre` = other).

## Version française (résumé)

Historique quotidien, public et horodaté d'un système de trading **mécanique** sur contrats perpétuels crypto (8 marchés Binance
Futures), démarré le 9 octobre 2026 à 04:20 UTC avec **1 000 $ fictifs**. **L'argent est fictif** ; les prix, frais et taux de
financement sont réels. **Ce n'est pas un conseil en investissement.** Les règles viennent de trois livres (Clenow, Kabbaj, Tharp) ;
aucune IA ne décide de trade. Pertes et incidents sont publiés, toujours à côté de trois références (BTC conservé, 50/50 BTC-ETH,
BTC filtré par sa moyenne 200 jours). Rapport publié chaque jour vers 00:45 UTC, en anglais.
