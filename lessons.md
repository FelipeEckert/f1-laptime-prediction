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
(add issues and solutions here as the project evolves)
