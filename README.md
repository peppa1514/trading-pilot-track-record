<!-- genere-par-report.py -->
# Crypto Pilot — public track record

**→ Live account: https://peppa1514.github.io/trading-pilot-track-record/**  
**→ The lab (20 strategies side by side): https://peppa1514.github.io/trading-pilot-track-record/lab.html**  
Repository: https://github.com/peppa1514/trading-pilot-track-record · X: [@CryptoPilotBot](https://x.com/CryptoPilotBot)

**What is this?** A computer program trades 8 cryptocurrencies with **pretend money** ($1,000 from 9 Oct 2026), but at real market
prices and with real fees. **No AI decides anything**: it follows fixed rules written in advance. Everything is published here every
day, losses included. **This is not investment advice**, nor a signal to copy, and nothing here promises any profit.

**In a bit more detail.**
- The money is simulated: no order reaches an exchange. Prices, fees (0.05%) and funding are real (Binance Futures, 8 markets: BTC,
  ETH, SOL, BNB, XRP, DOGE, ADA, AVAX); slippage is simulated.
- The rules come from three trading books: Andreas Clenow (method A, buy a 50-day high in a rising market), Thami Kabbaj (method B,
  buy a dip in a rising trend, target three times the risk), Van K. Tharp (method C, buy a 20-day high; method D, buy a new high after
  a short pullback, target three times the risk). Each trade risks 0.75% of the account; the code applies the rules with no exceptions.
- Why no AI: 6 language models were tested as a filter on 300 past signals, and none did better than taking every signal
  ([AI veto study](docs/ai-veto-study.md)). An AI only proposes research ideas, which the code tests before anything changes.
- Honesty: on past prices (Dec 2023 → Oct 2026) these rules made **+13%**, while just holding bitcoin made **+78%** (same fees and
  funding), with a much smaller worst dip (14% vs 61%). That is modest; only the live record will tell what it is worth. Three simple
  alternatives are always shown next to the account: just holding bitcoin, 50/50 bitcoin-ether, and bitcoin with a 200-day rule.
- The lab: 20 other ways of trading the same 8 cryptos, each with its own simulated $1,000, all started on 15 Oct 2026. Their rules
  were published before day 1 ([LAB.md](LAB.md), one SHA-256 fingerprint per strategy) and nothing is tuned afterwards. Under 3 months
  of data, the ranking is mostly luck; under 6 months, treat it as a hint.

**Goal.** 12 months of verifiable history, without interruption. The git history (one commit per day, timestamped by GitHub) makes any
rewrite visible. On the 1st of each month, the report publishes the SHA-256 hashes of the previous month's files.

**Files.**
- `data/equity.csv` — account value at each midnight UTC, with the three benchmarks
- `data/trades.csv` — every closed trade, with its result in R (multiples of the initial risk)
- `data/positions.csv` — open positions at report time
- `data/resume.json` — key figures and the rules in force
- `reports/YYYY-MM-DD.md` — daily report
- `lab.html`, `LAB.md`, `data/lab.csv`, `data/lab_strategies.json`, `data/lab_backtests.csv` — the Strategy Lab (forward test,
  pre-registration, frozen list, backtests not live)

Published automatically every day at about 00:45 UTC. Column names in the data files are in French (e.g. `compte_usd` = account value,
`btc_conserve_usd` = BTC buy & hold, `methode` = method, `sens` = side, `autre` = other).

## Version française (résumé)

Historique quotidien, public et horodaté d'un système de trading **mécanique** sur contrats perpétuels crypto (8 marchés Binance
Futures), démarré le 9 octobre 2026 à 04:20 UTC avec **1 000 $ fictifs**. **L'argent est fictif** ; les prix, frais et taux de
financement sont réels. **Ce n'est pas un conseil en investissement.** Les règles viennent de trois livres (Clenow, Kabbaj, Tharp) ;
aucune IA ne décide de trade. Pertes et incidents sont publiés, toujours à côté de trois références (BTC conservé, 50/50 BTC-ETH,
BTC filtré par sa moyenne 200 jours). Un laboratoire teste en parallèle 20 autres stratégies pré-enregistrées (lab.html), classées
uniquement sur les données postérieures à leur enregistrement. Rapport publié chaque jour vers 00:45 UTC, en anglais.
