# FIFA World Cup 2026 Prediction

An end-to-end data science and machine learning project that predicts the 2026 FIFA World Cup, from the 72 group-stage fixtures through the final.

The pipeline combines historical international results, FIFA rankings, World Cup records and football event statistics. It cleans and standardizes the sources, builds chronological team-strength features, compares several machine learning models and runs 10,000 tournament simulations.

## Headline result

The model ranked **Spain as the most likely champion**, with a **16.26% championship probability**. Its representative bracket predicted a Spain–England final, with Spain winning after a 1–1 draw and penalties.

> This is a probabilistic model output, not a guarantee. A 16.26% probability also means the model expected another team to win in most simulations.

## Prediction validated: champion correctly identified

**Spain went on to win the 2026 FIFA World Cup.** On 19 July 2026, Spain defeated Argentina 1–0 in the final and became world champion for the second time ([official FIFA match report](https://www.fifa.com/en/tournaments/mens/worldcup/canadamexicousa2026/articles/spain-argentina-final-report-highlights)).

The model therefore correctly identified the eventual champion as its highest-ranked candidate before the tournament outcome was known. It did **not** predict the complete final correctly: the representative bracket selected England rather than Argentina as Spain's opponent and predicted a penalty decision rather than the actual 1–0 result. This is a successful winner prediction, not a claim of perfect tournament accuracy.

## Project highlights

- Cleaned and standardized 49,000+ historical international matches
- Engineered 77 leakage-aware features using only information available before each match
- Built dynamic Elo ratings, recent-form measures, FIFA-ranking features, head-to-head statistics and tournament-experience indicators
- Compared Random Forest, Gradient Boosting, XGBoost and LightGBM models with chronological cross-validation
- Created separate models for goals, match outcome, corners and disciplinary events
- Combined the strongest models using inverse-error ensemble weights
- Simulated the complete 48-team tournament 10,000 times
- Exported reproducible group, knockout and tournament-probability results

## Model performance

| Target | Best model | Validation metric |
|---|---|---:|
| Home goals | XGBoost | MAE 1.067 |
| Away goals | XGBoost | MAE 0.875 |
| Match outcome | XGBoost | Accuracy 58.67% |
| Yellow cards | Random Forest | MAE 1.803 |
| Red cards | Random Forest | MAE 0.400 |

Chronological `TimeSeriesSplit` validation is used instead of random splitting to better represent prediction on future matches.

## Repository structure

```text
.
├── notebook.ipynb                 # Complete analysis and ML pipeline
├── data/
│   ├── group_fixtures.csv         # Group-stage schedule used by the model
│   └── knockout_slots.csv         # Knockout-bracket slot definitions
├── outputs/
│   ├── group_predictions.csv
│   ├── knockout_predictions.csv
│   └── tournament_progression_probabilities.csv
├── docs/
│   └── PROJECT_REPORT.md          # Methodology, results and limitations
└── requirements.txt
```

The larger public datasets are downloaded and cached automatically under `data/external/`; that cache is intentionally excluded from Git.

## How to run

1. Clone this repository and enter the project directory.
2. Create and activate a Python virtual environment.
3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Open `notebook.ipynb` in Jupyter Notebook or JupyterLab and run all cells from top to bottom.

The notebook uses a fixed random seed (`42`) and writes the final CSV files to `outputs/`.

## Data sources

- [International football results](https://github.com/martj42/international_results)
- [Historical FIFA rankings](https://github.com/cnc8/fifa-world-ranking)
- [Alternative FIFA ranking history](https://github.com/Dato-Futbol/fifa-ranking)
- [World Cup data](https://github.com/jfjelstul/worldcup)
- [Football-Data.co.uk](https://www.football-data.co.uk/)
- [FIFA Men's World Ranking](https://inside.fifa.com/fifa-world-ranking/men)

Please consult each upstream source for its own terms and attribution requirements.

## Limitations

- Football results are uncertain; simulation probabilities should not be interpreted as certainty.
- Placeholder playoff teams use synthetic regional or global-average profiles.
- Historical international corner data is incomplete, so corner predictions use league-event calibration and a conservative proxy.
- Ranking mirrors and optional web sources can vary in freshness and availability.
- The representative knockout bracket uses the most frequent team for each simulated slot and is not a single literal simulation path.

## Skills demonstrated

Python, pandas, NumPy, data cleaning, exploratory analysis, feature engineering, time-series validation, classification, regression, ensemble learning, Monte Carlo simulation, post-event validation, data visualization and reproducible reporting.

## Author

**Omarijbaril** — aspiring Data Analyst and Machine Learning practitioner.
