# F1 Lap Time Prediction — Roadmap
Last updated: May 2026

## Pipeline

[✅] Project setup — venv, requirements.txt, folder structure
[✅] notebook 01 — data collection via FastF1 (5 races, 2023 season)
[ ] Run notebook 01 and confirm laps_2023.csv is saved correctly
[ ] notebook 02 — EDA (exploratory data analysis)
[ ] notebook 03 — ML pipeline (Linear Regression → Random Forest → XGBoost → SHAP)
[ ] src/data_loader.py — extract reusable loading functions from notebook 01
[ ] src/features.py — feature engineering functions
[ ] src/evaluate.py — metrics and plot functions
[ ] README.md — final write-up with findings, visuals, how to run
[ ] outputs/figures/ — export key charts for README
[ ] GitHub — push everything and verify presentation

## notebook 02 — EDA (planned)
- Distribution of LapTime (histogram + boxplot)
- Lap time evolution across race (LapNumber vs LapTime per driver)
- Compound effect on lap time (boxplot per compound)
- Tyre degradation curve (TyreLife vs LapTime)
- Weather correlation heatmap
- Missing values analysis
- Outlier check

## notebook 03 — ML pipeline (planned)
- Feature encoding (Compound, Driver, Team → label or one-hot)
- Train/test split (by race — avoid data leakage)
- Model 1: Linear Regression (baseline)
- Model 2: Random Forest
- Model 3: XGBoost (main)
- Metrics comparison table (MAE, RMSE, R²)
- SHAP summary plot — feature importance
- Predicted vs actual plot

## Backlog — future improvements
- Add more races (full 2023 season or 2024)
- Add qualifying data as a feature (driver baseline pace)
- Sector time breakdown (S1, S2, S3 as targets)
- Streamlit dashboard for interactive prediction
- Deploy on Hugging Face Spaces or Render

## Notes for next sessions
- Always read CLAUDE.md first for full project context
- laps_2023.csv is gitignored — must re-run notebook 01 to regenerate if needed
- When adding new decisions or issues, update lessons.md
- When completing a step, check it off in this file
