# Can a language model improve a mechanical trading system by vetoing its signals?

*A small, pre-launch study by the Crypto Pilot project. Run on 2026-10-08, published on 2026-10-11.
All figures below were recomputed from the raw results table with SQL for this page.*

## Short answer

**No, not in this test.** Six language models were each asked to accept or reject the same 300 historical signals from a
rule-based trend-following system. None of them improved on simply taking every signal. For every model, the signals it
rejected went on to perform about as well as the ones it accepted (no difference was statistically significant), so every
model ended with a lower total than "take everything". As a result, the published system uses **no AI to make trading decisions**.

## The question

The system trades four methods taken from trading books (Clenow: 50-day breakout; Kabbaj: pullback with reward/risk ≥ 3;
Tharp: channel breakout and 3R swing) on 8 crypto perpetual futures markets. Its rules are mechanical. An earlier version of
the project let a language model make the trading decisions, and it did worse than the plain rules. That led to a narrower question:

> If the rules generate a signal, can a model add value by **rejecting** the bad ones, a veto, while the rules do everything else?

A useful veto would reject mostly losing signals: the average result of the rejected signals should be clearly negative,
and the filtered total should be higher than taking every signal.

## Protocol

- **Signals.** 300 signals from a historical backtest of the four methods (2024-09-01 → 2026-09-22 entries), outside any
  period used earlier to train or test the AI agent. Random draw (fixed seed) stratified by method: 75 signals per method,
  about half long and half short. Each signal has a known outcome in **R** (the profit or loss divided by the initial risk),
  net of fees and funding.
- **What each model saw.** The method's written rules, the side (long/short), the stop and target rules, and a snapshot of the
  market at the moment of entry: computed indicators (trend, RSI, ATR %, 5/20/50-day change, volume ratio, distance to the 20/50/200
  EMAs in ATR), the last 40 daily and 24 four-hour candles, the same kind of summary for the largest market (BTC) when the signal was
  on another market, the funding rate, and the Fear & Greed index.
- **Anonymization.** The market name was hidden, all prices were rebased to 100 (100 = last daily close), and no date was given.
  The reason: a model that recognizes "BTC in March 2025" may remember what happened next. Rebasing and hiding names and dates makes
  that much harder, so the test measures judgment on the context, not memory.
- **The question asked.** One fixed prompt for all models: the system has a slightly positive expectancy, take the signal unless
  the context looks bad, do not reject out of general caution; answer in JSON with "take" or "pass", a confidence and a short reason.
  If a model failed to answer, the signal counted as taken (the system trades by default).
- **Measure.** Mean R of taken vs. rejected signals, filtered total vs. "take all", and Welch's t-test between taken and rejected.

## Baseline

| | Signals | Total | Mean per signal | t (mean > 0) | Winning signals |
|---|---|---|---|---|---|
| Take every signal | 300 | **+43.4 R** | +0.145 R | 1.66 | 108 (36%) |

By method: A +0.22 R, B −0.05 R, C +0.15 R, D +0.26 R per signal (75 each).

## Results

| Model | Taken | Rejected | Errors | Mean R taken | Mean R rejected | Filtered total | Gain vs. take all | Expected gain of a random veto* | t (taken vs. rejected) | Cost | Seconds / answer |
|---|---|---|---|---|---|---|---|---|---|---|---|
| openai/gpt-oss-120b | 202 | 98 | 0 | +0.174 | +0.085 | +35.1 R | −8.3 R | −14.2 R | 0.48 | $0.08 | 4.9 |
| qwen/qwen3.5-plus | 132 | 166 | 2 | +0.255 | +0.069 | +31.9 R | −11.5 R | −24.0 R | 1.07 | $2.70 | 87.7 |
| anthropic/claude-haiku-5.5 | 181 | 119 | 0 | +0.141 | +0.151 | +25.5 R | −17.9 R | −17.2 R | −0.06 | $0.22 | 7.4 |
| deepseek/deepseek-v4-flash | 155 | 143 | 2 | +0.167 | +0.137 | +23.8 R | −19.6 R | −20.7 R | 0.17 | $0.24 | 47.7 |
| anthropic/claude-sonnet-5.5 | 139 | 161 | 0 | +0.161 | +0.130 | +22.4 R | −21.0 R | −23.3 R | 0.18 | $2.57 | 16.5 |
| deepseek/deepseek-v4-pro | 124 | 176 | 0 | +0.125 | +0.159 | +15.5 R | −27.9 R | −25.5 R | −0.20 | $1.64 | 60.5 |

\* A veto that rejects the same number of signals **at random** loses on average (number rejected) × 0.145 R, since the average
signal is positive. "Errors" (no valid answer) counted as taken. Costs are the API costs for the 300 answers (gpt-oss-120b includes an
8-signal dry run). Total cost of the study: about $7.45.

## Reading

- **No model improved the system.** Every filtered total is below +43.4 R. The rejected signals have a positive average for every
  model (+0.07 to +0.16 R): the models mostly rejected signals that would have made money.
- **No model showed a significant ability to tell good signals from bad ones.** The highest t-value is 1.07 (qwen3.5-plus); none
  is close to the usual threshold of 2, and with six models tested, one of them reaching about 1 is what chance alone would produce.
- **The losses look like random rejection.** For four models, the loss is within 2.5 R of what rejecting the same number of
  signals at random would cost. gpt-oss-120b and qwen3.5-plus lost less than a random veto would, but not significantly so
  (t = 0.48 and 1.07).
- **The loss mostly tracks how many signals a model rejected, not which ones.** gpt-oss-120b rejected 98 signals and lost 8.3 R;
  deepseek-v4-pro rejected 176 and lost 27.9 R. The prompt asked the models not to reject out of general caution; four of the six
  still rejected more than 40% of the signals.
- **Price and quality are unrelated here.** The cheapest model (gpt-oss-120b, $0.08) lost the least; the two most expensive ones did not do better.
- **A post-hoc check, not a test:** when at least 4 of the 6 models rejected a signal (132 signals), those signals averaged
  +0.067 R against +0.206 R for the others (t = 0.79). Following the majority would still have lost 8.8 R against taking everything.

## Limits

- **One sample of 300 signals.** With a spread of about 1.5 R per signal, a real but small edge in the veto (say 0.1 R per rejected
  signal) could go undetected. The conclusion is "no useful veto found", not "no model could ever help".
- **One way of asking.** A single prompt, a single context format, one answer per signal, in French. Other framings (ranking signals,
  several answers per signal, other information such as news or order books) were not tested.
- **One period and one system.** Signals come from 2024-2026 crypto markets and from these four methods. Results may differ elsewhere.
- **Anonymization is imperfect.** Candle shapes, funding and the Fear & Greed index could in principle hint at the period. If a model
  had recognized periods, it should have done *better*, which it did not.
- **Signal outcomes come from a per-method backtest**, not from the live portfolio (where one position per market and a risk cap also apply).
- **Cost.** About $7.45 and a few hours, which limited the sample size and the number of variants.

## What this means for the project

- The live system ([track record](https://github.com/peppa1514/trading-pilot-track-record)) applies its rules mechanically.
  **No AI makes or vetoes any trade.**
- An AI is used only as a **researcher**: once a week, a model may propose at most two rule changes, written as backtest
  parameters. The code tests them on historical data with strict, pre-set criteria (training and validation periods, every
  validation half-year non-negative, minimum number of trades, a significance threshold that rises with the number of ideas tried,
  maximum drawdown, stability of neighboring parameters). Most proposals are expected to fail. A proposal that passes would
  then trade on a separate shadow portfolio before it could replace anything.

## Data and code

- Raw results: [`ai-veto-study-results.csv`](ai-veto-study-results.csv): one row per model and signal (1,800 rows: model, signal
  number, market, method, side, entry date, outcome in R, decision, confidence). Every table above can be recomputed from it.
  Decisions are in the prompt's language: `prendre` = take, `passer` = pass, empty = no valid answer (counted as taken).
- Code: `school/veto_test.py` (signal sampling, anonymized context, prompt, scoring) in the project's private repository; the
  prompt and context format are described above.
- This study is not investment advice. The money in this project is simulated.
