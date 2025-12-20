# Avatar 3 Domestic Opening Weekend Forecast

This repository contains a forecasting project that predicts the domestic opening weekend box office revenue of Avatar 3 using historical movie data, leakage-free pre-release features, and conditioned prediction intervals. It is designed to be reproducible and defensible for academic review or portfolio evaluation.

## Project overview

Question:
Given only information available before release, what can we predict about Avatar 3's domestic opening weekend revenue, and how uncertain is that prediction?

Key characteristics:

* Forecasting (not hindsight fitting)
* Time-aware evaluation
* No post-release or leaky features
* Explicit uncertainty handling (heteroskedasticity-aware)
* Clear separation between dataset construction and forecasting

## Methodology summary

### Data construction

Historical movie data is compiled and cleaned in a dedicated notebook. Raw data is transformed into a model-ready dataset that contains pre-release features only.

### Modeling strategy

* Target: domestic opening weekend revenue (USD)
* Target transform: log1p to handle heavy-tailed revenue distributions
* Evaluation: strict time-based train/validation split
* Models evaluated (exactly three):

  1. Median baseline
  2. Ridge regression
  3. XGBoost regressor

### Uncertainty estimation (prediction interval)

Naive percentage-error intervals are unstable for box office because micro-releases can create extreme percentage errors. Instead, this project constructs prediction intervals using:

* Log-space residual quantiles
* Conditioned residual distributions based on pre-release scale indicators (e.g., major studio and high-budget class)

This better reflects uncertainty for a blockbuster forecast such as Avatar 3.

## Key result

Avatar 3 domestic opening weekend forecast:

* Point estimate: $161M
* 80% conditioned prediction interval: $23M - $792M

Interpretation: the interval reflects historical model error among comparable large releases and should be treated as probabilistic uncertainty, not scenario bounds.

## Repository structure

```
avatar3-opening-forecast/
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
│
├── notebooks/
│   ├── 01_box_office_dataset_builder.ipynb
│   └── 02_train_and_forecast_opening.ipynb
│
├── data/
│   ├── raw/
│   │   └── raw_movies_all.csv
│   └── processed/
│       └── preprocessed_movies_all.csv
│
└── artifacts/
    └── avatar3_forecast/
        ├── final_model.pkl
        ├── feature_cols.json
        ├── metrics_backtest.csv
        └── avatar3_prediction.json

```

## How to run

1. Clone the repository

   git clone [https://github.com/56Percentt/avatar3-opening-forecast.git](https://github.com/56Percentt/avatar3-opening-forecast.git)
   cd avatar3-opening-forecast

2. Install dependencies

   python -m pip install -r requirements.txt

3. Run the main notebook

Open and run:

* notebooks/02_train_and_forecast_opening.ipynb

The notebook assumes the repository structure is preserved and loads data via repository-relative paths.

## Interpretation notes and limitations

* This is a forecast, not a guarantee.
* Prediction intervals are statistical and reflect historical model error.
* Lower bounds represent rare underperformance within the conditioned class and may be economically implausible for Avatar 3 given franchise strength; they are retained for statistical honesty.
* The model intentionally excludes post-release signals (reviews, word-of-mouth, realized popularity) to preserve causal validity.

## License

MIT License.

## Context

This repository was created as a research-style forecasting exercise and portfolio project. Feedback is welcome.
