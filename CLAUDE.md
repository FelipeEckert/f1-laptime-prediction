# F1 Lap Time Prediction — Project Context for Claude Code

## Project goal
Predict F1 lap times using real telemetry data via FastF1.
This is a portfolio project showcasing Python, data engineering, and ML skills
for a career transition from automotive engineering to data analytics.

## Owner
Felipe Eckert — Electrical Engineer (Poli-USP), Junior R&D Analyst at Hyundai Motor Brasil.
Target: remote international roles in data analytics / automation / Industry 4.0.

## Stack
- Data: FastF1, Pandas, NumPy
- Visualization: Matplotlib, Plotly, Seaborn
- ML: Scikit-learn, XGBoost, SHAP
- Environment: Python .venv, Jupyter notebooks in VSCode

## Project structure
notebooks/01_data_collection.ipynb  ← data collection via FastF1 (done)
notebooks/02_eda.ipynb              ← exploratory data analysis (next)
notebooks/03_modeling.ipynb         ← ML pipeline and evaluation
src/data_loader.py                  ← reusable data loading functions
src/features.py                     ← feature engineering
src/evaluate.py                     ← metrics and plots
data/raw/laps_2023.csv              ← collected dataset (gitignored)
outputs/figures/                    ← exported charts

## ML approach
1. Baseline: Linear Regression
2. Intermediate: Random Forest
3. Main model: XGBoost
4. Interpretability: SHAP values
Metrics: MAE, RMSE, R²

## Features used
LapTime (target, in seconds), LapNumber, Stint, TyreLife, Compound,
AirTemp, TrackTemp, Humidity, WindSpeed, Driver, Team, GrandPrix

## Conventions
- All code and comments in English
- Notebooks have markdown cells explaining every step (why, not just what)
- Functions go in src/ — notebooks import from src/ when reusing logic
- No proprietary data — only FastF1 public API
- Figures saved to outputs/figures/ as PNG (1200x600 minimum)

## Current status
See todo.md for pipeline and lessons.md for decisions already made.
