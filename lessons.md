# F1 Lap Time Prediction — Decisions & Lessons Log
Last updated: May 2026

## Project decisions

| Decision | Rationale |
|---|---|
| FastF1 as data source | Real F1 telemetry, public API, well-maintained Python library |
| 2023 season, 5 races | Enough data for ML without overloading first version; scalable later |
| Races: Bahrain, Saudi Arabia, Australia, Azerbaijan, Monaco | Diverse track types (street, permanent, high-speed) — model generalizes better |
| LapTime in seconds (float) | Timedelta not compatible with ML libraries — converted on collection |
| pick_quicklaps(1.07) + TrackStatus == 1 | Removes in-laps, out-laps, SC/VSC laps — cleaner target variable |
| telemetry=False in FastF1 load | Keeps data loading fast; high-freq telemetry not needed for lap-level ML |
| weather=True in FastF1 load | Air temp, track temp, humidity, wind speed merged automatically |
| XGBoost as main model | Handles mixed features (numeric + categorical), robust, industry standard |
| SHAP for interpretability | Explains predictions — separates this from a black-box model |

## Technical notes
- First FastF1 run downloads ~200–400 MB (cached after that — subsequent runs are instant)
- Monaco is Round 6 in 2023 (Round 5 is Miami) — FastF1 resolves name correctly
- Pandas 2.3.x installed (not 3.0) — API stable, compatible with all libraries
- GrandPrix column added manually as race label for grouping in EDA

## Issues encountered
- **FastF1 weather columns not in laps DataFrame:** `AirTemp`, `TrackTemp`, `Humidity`, `WindSpeed` are not part of `session.laps` — they live in `session.weather_data` and require a time-based merge. Removed from `KEEP_COLS` in notebook 01. Will be merged in feature engineering step (notebook 03).
- **Monaco GP is a structural outlier:** Shortest track on the calendar, unique tyre strategy (MEDIUM start → early HARD switch), and naturally faster lap times (~76 s). It distorts compound comparisons and degradation curves when aggregated with other races. Solution: always show per-race breakdown alongside aggregate views.
- **Compound imbalance:** HARD=2827 laps, MEDIUM=847, SOFT=358. Reflects real race strategy but will affect the ML model. Handle with stratified train/test split in notebook 03.
- **ipywidgets added for interactive plots** inside Jupyter notebooks. Requires `ipywidgets` installed in the venv (added to requirements.txt). Already bundled with jupyterlab — no separate install needed.
