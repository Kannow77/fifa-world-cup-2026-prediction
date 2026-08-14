# Project Report

## 1. Business question

Can historical team performance, rankings, tournament experience and recent form be combined to estimate match outcomes and simulate the full 2026 FIFA World Cup?

## 2. Analytical workflow

The notebook implements one reproducible workflow:

1. Load the supplied 2026 fixtures and knockout-slot definitions.
2. Download and cache public football datasets.
3. Standardize dates, team names, scores and tournament labels.
4. Process matches chronologically and calculate each team's state before every fixture.
5. Train regression models for goals and event totals and classification models for match outcomes.
6. Rank models using chronological cross-validation.
7. Fit weighted ensembles using inverse validation error.
8. Predict all group fixtures and simulate the tournament 10,000 times.
9. Export group predictions, knockout predictions and progression probabilities.

## 3. Data preparation

The cleaned international-results table contains 49,404 matches. The modeling window produces 31,567 training observations and 77 predictors. Cleaning includes duplicate removal, date and numeric conversion, text repair, team-name standardization, missing-value handling and tournament categorization.

The features include:

- dynamic Elo strength
- FIFA rank and ranking points
- recent wins, draws, losses and points per match
- rolling goals scored and conceded
- clean-sheet rate
- home and host advantage
- head-to-head history
- World Cup experience
- weighted recent form and tournament performance

Features are generated in chronological order so the state recorded for a historical match is based on information available before that match.

## 4. Modeling

Separate supervised-learning tasks are used because the targets have different behavior:

- goal totals: Random Forest, Gradient Boosting and XGBoost regression
- match outcome: Random Forest, Gradient Boosting, XGBoost and LightGBM classification
- corners and cards: tree-based regression ensembles

`TimeSeriesSplit` is used for evaluation. The final ensemble weights are inversely related to validation error, allowing stronger models to contribute slightly more without relying on only one algorithm.

## 5. Results

The best observed validation results were:

- home goals: XGBoost, MAE 1.067
- away goals: XGBoost, MAE 0.875
- outcome: XGBoost, accuracy 58.67% and log loss 0.894
- yellow cards: Random Forest, MAE 1.803
- red cards: Random Forest, MAE 0.400

Across 10,000 simulations, the leading championship probabilities were:

| Team | Champion probability | Finalist probability |
|---|---:|---:|
| Spain | 16.26% | 25.61% |
| England | 11.57% | 19.79% |
| Argentina | 11.15% | 19.22% |
| France | 10.93% | 19.08% |
| Brazil | 7.71% | 14.06% |

The representative bracket selected Spain and England for the final and Spain as the predicted winner after penalties.

## 6. Interpretation

Spain is the model's leading candidate, not an overwhelming favorite. Its probability is higher than every individual alternative, while the combined probability of all other teams remains much larger. This distinction matters when communicating probabilistic predictions.

The result is driven by the combined team-strength profile, including Elo, rankings, recent performance and tournament features—not by a manually selected winner.

### Post-tournament validation

Spain subsequently won the tournament, defeating Argentina 1–0 in the final on 19 July 2026 ([FIFA match report](https://www.fifa.com/en/tournaments/mens/worldcup/canadamexicousa2026/articles/spain-argentina-final-report-highlights)). The model's top-ranked champion prediction was therefore correct.

This result should be interpreted precisely. The model correctly selected the winner, but its representative bracket predicted England as Spain's final opponent, a 1–1 score and a Spanish win on penalties. It did not predict the full final matchup or score correctly. A single successful tournament-winner prediction is encouraging portfolio evidence, but it is not sufficient by itself to establish general model accuracy; repeated out-of-time backtesting remains necessary.

## 7. Limitations and next steps

- Replace playoff placeholders once the qualified teams are known.
- Add calibration plots and out-of-time backtesting by historical World Cup edition.
- Evaluate probability calibration with Brier scores.
- Use richer international event data for corners and disciplinary predictions.
- Track data snapshots and model versions for stronger experiment reproducibility.
- Add a small Streamlit dashboard for interactive team and matchup exploration.

## 8. Responsible use

The predictions are an analytical exercise and should not be treated as certain outcomes or financial/betting advice. Results depend on the supplied fixtures, the available source snapshots, modeling assumptions and random simulation.
