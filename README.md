<!-- genere-par-report.py -->
# Crypto Pilot — historique public / public track record

**Page : https://peppa1514.github.io/trading-pilot-track-record/** · X : [@CryptoPilotBot](https://x.com/CryptoPilotBot)

## Français

**Ce que c'est.** L'historique quotidien, public et horodaté d'un système de trading **mécanique** sur contrats perpétuels crypto
(8 marchés Binance Futures : BTC, ETH, SOL, BNB, XRP, DOGE, ADA, AVAX), démarré le **9 octobre 2026 à 04:20 UTC** avec **1 000 $ fictifs**.

**Ce que ce n'est pas.**
- **L'argent est FICTIF.** Aucun ordre n'est envoyé à une bourse. Les **prix, frais (0,05 %) et taux de financement sont réels** (Binance),
  le glissement est simulé.
- **Ce n'est pas un conseil en investissement**, ni une offre, ni un signal à copier. Rien ici ne promet de gain.

**Comment il décide.** Les règles viennent de trois livres : Andreas Clenow (cassure 50 jours, méthode A), Rachid Kabbaj (repli avec
rapport gain/risque ≥ 3, méthode B), Van K. Tharp (cassure de canal, méthode C ; swing à 3R, méthode D). Risque fixe de 0,75 % du compte
par position, stops calculés sur l'ATR. Le code applique ces règles seul, sans exception manuelle. **Aucune IA ne décide de trade** :
on a testé 6 modèles de langage comme « veto » sur 300 signaux historiques, aucun n'a fait mieux que de tout prendre. Une IA sert
seulement à proposer des hypothèses de recherche, testées par le code sur l'historique puis sur un portefeuille fantôme avant toute adoption.

**Honnêteté.** Les règles en vigueur sont publiées (`data/resume.json`, chaque rapport donne la version du cahier). Les pertes,
les échecs et les incidents techniques sont publiés aussi. Trois références sont toujours affichées à côté : BTC conservé, 50/50 BTC-ETH,
BTC filtré par sa moyenne 200 jours. En test sur 2023-12 → 2026-10, ce système faisait **moins** que BTC conservé en rendement brut
(+16,5 % contre +122 %) avec une baisse maximale bien plus faible (14 % contre 53 %). Le direct dira ce qu'il en est vraiment.

**But.** 12 mois d'historique vérifiable, sans interruption. L'historique git (un commit par jour, horodaté par GitHub) rend toute
réécriture visible. Le 1er de chaque mois, le rapport publie l'empreinte SHA-256 des fichiers du mois précédent.

**Fichiers.** `data/equity.csv` (valeur du compte à chaque minuit UTC et références), `data/trades.csv` (tous les trades fermés, en R),
`data/positions.csv` (positions ouvertes au moment du rapport), `data/resume.json` (chiffres clés et règles), `reports/AAAA-MM-JJ.md`
(rapport du jour). Publié automatiquement chaque jour vers 00:45 UTC.

## English

**What this is.** The daily, public, timestamped track record of a **mechanical** trading system on crypto perpetual futures
(8 Binance Futures markets), started on **9 October 2026 at 04:20 UTC** with **$1,000 of simulated money**.

**What this is not.**
- **The money is SIMULATED (paper trading).** No order reaches an exchange. **Prices, fees (0.05%) and funding rates are real** (Binance);
  slippage is simulated.
- **This is not investment advice**, nor an offer, nor a signal to copy. Nothing here promises any profit.

**How it decides.** The rules come from three books: Andreas Clenow (50-day breakout), Rachid Kabbaj (pullback with reward/risk ≥ 3),
Van K. Tharp (channel breakout; 3R swing). Fixed risk of 0.75% of the account per position, ATR-based stops. Code applies the rules on
its own, with no manual exceptions. **No AI decides any trade**: 6 language models were tested as a "veto" on 300 historical signals and
none did better than taking every signal. An AI is only used to propose research hypotheses, which code tests on history and then on a
shadow portfolio before any adoption.

**Honesty.** The rules in force are published (`data/resume.json`; each report states the rulebook version). Losses, failures and
technical incidents are published too. Three benchmarks are always shown: BTC buy-and-hold, 50/50 BTC-ETH, and BTC filtered by its
200-day moving average. In backtests over 2023-12 → 2026-10 this system returned **less** than holding BTC (+16.5% vs +122%) with a much
smaller maximum drawdown (14% vs 53%). Live trading will tell.

**Goal.** 12 months of verifiable history, without interruption. The git history (one commit per day, timestamped by GitHub) makes any
rewrite visible. On the 1st of each month the report publishes SHA-256 hashes of the previous month's files.
