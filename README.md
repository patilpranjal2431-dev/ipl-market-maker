# 🏏 IPL Market Maker: Win Probability Exchange

An interactive cricket **market-making simulator**. Watch a T20 chase unfold ball by ball, trade virtual contracts on "chasing team wins", and see how a market maker quotes prices, manages inventory and loses money to informed traders.

**Live demo:** https://YOUR-USERNAME.github.io/ipl-market-maker/

> ⚠️ Educational simulation using **virtual coins only**. No real money, no betting, no payments, and nothing here is betting or financial advice.

## What it does

- **Live Match:** an LED-style scoreboard and a simulated second-innings chase. You start with 10,000 virtual coins. A contract pays 100 if the chasing team wins and 0 if it loses. Buy at the ask, sell at the bid.
- **Market maker:** quotes bid and ask around a win-probability estimate. The spread widens with uncertainty, after boundaries and wickets, and late in the innings. Quotes are skewed by the bot's inventory.
- **Simulated bettors:** noise traders, momentum chasers and an informed trader who knows the exact probability, so you can see adverse selection happen.
- **Exact oracle:** a dynamic-programming solution gives the true win probability under the simulator's rules.
- **Model Lab:** a what-if probability calculator, a calibration chart (predicted vs actual) with Brier scores, and a loader for your own trained logistic regression.
- **Portfolio and dashboard:** trade history, equity curve, achievements, market-maker P&L and inventory charts, tunable spread parameters.
- **Help and self-tests:** guided tour, glossary, keyboard shortcuts (B, S, N, Space) and 7 built-in correctness tests (About page).

## How it works

| Piece | Method |
|---|---|
| Ball outcomes | Phase-dependent probabilities (powerplay, middle, death) adjusted for wickets lost and team strength |
| Baseline model | Normal approximation: `p = Φ((E − runs_needed + 0.5) / sd)`, with `E = balls_left × (0.75 + 0.07 × wickets_in_hand)` and `sd = 1.6 √balls_left` |
| Oracle | Dynamic programming over (balls left, wickets lost, runs needed) |
| Quotes | `half-spread = h0 + a·4p(1−p) + b·event + c/√(balls_left+1)`; `mid = 100p − k·inventory` |
| Evaluation | Reliability (calibration) curve and Brier score for each model vs the oracle |

## Using your own trained model

Train a logistic regression on real IPL ball-by-ball data (for example from [Cricsheet](https://cricsheet.org)) with features `runs_needed, balls_left, wickets_in_hand, required_run_rate, current_run_rate`, then paste this JSON into **Model Lab → Load Trained Model**:

```json
{
  "features": ["runs_needed","balls_left","wickets_in_hand","required_run_rate","current_run_rate"],
  "mean": [0,0,0,0,0],
  "std": [1,1,1,1,1],
  "intercept": 0.0,
  "weights": [0,0,0,0,0]
}
```

Note: the in-app matches come from a simulator, not real IPL data, so a model trained on real matches will not be perfectly calibrated inside it. Comparing the two is part of the exercise.

## Run locally

No install needed. Download `index.html` and open it in a browser (internet is needed once to load Chart.js from a CDN).

## Tech

Vanilla HTML, CSS and JavaScript in a single file, with [Chart.js](https://www.chartjs.org/) for charts. Progress is saved in your browser's `localStorage`.

## Roadmap

- Calibrate the simulator's ball probabilities from real Cricsheet phase statistics
- Gradient-boosting lookup-grid model import
- Multi-match tournament mode and leaderboard

## License

MIT, see [LICENSE](LICENSE).
