# Yilgarn 40 km review precomputed — provenance

Completed: 2026-09-22T05:47:06.780647+00:00

Code commit: `9331649c857301276c05f202b8572ffdf6a348e3`; script SHA-256: `3e262b7cc006fd277cd02469e8a29aeeac753ba8192630cb421946b72fb0ab98`.

Input: `yilgarn_40km.csv`, SHA-256 `49b7855efb95a59590c80fb2e1e818bacbd02dfaecc9cde7d0eeab05335af151`; 1122 blocks, recorded P244/U878.

Source data: kigam-research/mpm-pu-conformal-yilgarn commit c03c67df88bb7591943bdc00e1a3e10c2b5de3bc. Ni_mine derivatives: CC BY-NC 4.0; predictors follow DATA-LICENSE.md.

## Configuration and interpretation

Spatial groups: 160 km; outer 5, inner 3, grid 6, bootstrap 50, PU bags 8, U fraction 0.35, seed 20260922, CPU threads 2. All42 columns are21 predictors plus fixed missing indicators. Native NaN handling; no global imputation or screening.

Each outer fold partitions into proper train (~40% total), Platt calibration (~20%), positive inclusion calibration (~20%), test (~20%). Tuning uses proper-train inner validation only. All four roles have disjoint spatial groups. Outer OOF is evaluation only; no full-data deployment fit.

Positive inclusion uses split-conformal nonconformity1-score with rank ceil((nP+1)*(1-alpha)), alpha=.15; calibration positives only, no negative-class set. Platt is fitted on a different held-out pool. Hoeffding nP>=82 is reported as a support diagnostic, not an implemented risk guarantee. Raw rho=(Q75-Q25)/mean with validity mean>=1e-4; no invented stability for invalid values. Rank dispersion is within each outer fold.

Grid search replaces optional Optuna; this is a new documented review adapter, not the pinned upstream pipeline. Upstream core ideas retained: XGBoost vs U-subsampled BaggingPU, external bootstrap and relative IQR. Changes: block bootstrap, separated calibration roles, finite-sample positive-only split conformal (not upstream pooled cross-conformal/FNR-direct), native missing handling, explicit missing indicators, no regulatory zone terminology.

Main scores/membership are in `oof_scores.csv` and `conformal_membership.csv`. `bootstrap_matrix.npz` arrays are model×item with shape(50,1122), IDs in block_id; separate xgboost/baggingpu arrays. `m3_split_manifest.csv` is long-format: one row perID perouterfold, not1122unique rows. Use the test-role rows for the canonical outer fold of eachID. A newly generated chat split must not be relabeled onto these OOF scores.

## Limitations

- 40 km aggregated review data; not a 5 km study reproduction or deployment model.
- U is unlabeled, not a verified negative. AP/Brier/Platt describe recorded P versus U.
- 160 km grouped folds are an explicit design candidate, not a validated decorrelation range; no buffer.
- Grouped interpolation evaluation is not a held-out new-region transfer experiment.
- Previous exploratory M0/M1 used all labels; this is exploratory, not confirmatory evidence.
- Positive-class split-conformal uses held-out calibration; spatial exchangeability and population-positive representativeness are unverified.
- No exact spatial coverage guarantee, no negative-class membership, no discovery probability.
- Bootstrap holds selected parameters fixed; dispersion excludes model-selection/data/label uncertainty.
- Raw bootstrap rho is conditional model-score dispersion, not total uncertainty or cross-method superiority.
- Maps are outer OOF mosaics with fold-specific fits; fold seams are not geological boundaries.
- Budgets are block-count shares, not ground-area shares; partial edge cells have different support.
- Permutation importance is descriptive outer-held-out AP drop and is not used for feature selection.

## Method references

- https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedGroupKFold.html
- https://xgboost.readthedocs.io/en/stable/python/python_api.html
- https://arxiv.org/abs/2107.07511

Package versions, settings, file hashes and timestamps: `manifest.json`. All predictions are actual computations, not fixtures.
