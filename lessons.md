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
- **Correlation heatmap on raw pooled data is misleading:** track-level variance (e.g. Monaco ~76 s vs Bahrain ~95 s base lap time) dominates all correlations and masks within-race dynamics. TyreLife showed a *negative* correlation with LapTime due to the circuit confound (Monaco = short stints + fast laps) compounded by the fuel burn effect (high TyreLife → lighter car → faster). Fix: z-score normalize LapTime, TyreLife, LapNumber, and Stint within each race before pooling. This removes between-circuit variance entirely so the correlation matrix reflects only within-race dynamics. After normalization, TyreLife is correctly positive (tyre degradation) and LapNumber is correctly negative (fuel burn).
- **TyreLife and LapNumber correlation is 0.77 (not 1.0):** they only move together within a single stint. After a pit stop, LapNumber continues incrementing but TyreLife resets to zero. They diverge across stints, which explains the sub-1.0 correlation.
- **TyreLife vs LapTime shows -0.36 (negative) even after normalization:** caused by the tyre warm-up window effect — early stint laps are slow (cold tyres, TyreLife low), then the tyre enters its optimal window and gets faster despite TyreLife increasing. Degradation only appears late in the stint, which this dataset does not capture well because drivers pit before full degradation.
