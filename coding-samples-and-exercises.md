# Day 6 Coding Samples & Real-Time Exercises Companion Guide

This guide pairs 1-to-1 with every notebook inside `notebooks/`. Use it to pace live demos, support struggling freshers with tiered hints, verify expected classroom outputs, and run crisp post-lab debriefs.

---

## Quick Index of All Notebooks

| # | Notebook File | Type | Module | Timebox |
| --- | --- | --- | --- | --- |
| 00 | `00_environment_check.ipynb` | Smoke Test | Welcome | 10 min |
| 01 | `01_demo_ml_lifecycle_and_project_structure.ipynb` | Live Demo | Module 1 | 20 min (+10 min discussion) |
| 02 | `02_demo_data_ingestion_and_validation.ipynb` | Live Demo | Module 2 | 20 min |
| 03 | `03_exercise_data_ingestion_and_validation_starter.ipynb` / `_solution.ipynb` | Real-Time Lab | Module 2 | 25 min |
| 04 | `04_demo_feature_engineering_and_leakage.ipynb` | Live Demo | Module 3 | 20 min |
| 05 | `05_exercise_feature_engineering_starter.ipynb` / `_solution.ipynb` | Real-Time Lab | Module 3 | 25 min |
| 06 | `06_demo_training_and_testing_pipeline.ipynb` | Live Demo | Module 4 | 20 min |
| 07 | `07_exercise_training_pipeline_starter.ipynb` / `_solution.ipynb` | Real-Time Lab | Module 4 | 25 min |
| 08 | `08_demo_experiments_reproducibility_and_config.ipynb` | Live Demo | Module 5 | 30 min |
| 09 | `09_capstone_end_to_end_pipeline_starter.ipynb` / `_solution.ipynb` | Capstone Lab | Capstone | 30 min |

---

## Coding Sample 0: Environment Smoke Test
- **Notebook:** `notebooks/00_environment_check.ipynb`
- **Timebox:** 10 minutes (09:05 – 09:15)
- **Expected Output:**
  - All 9 libraries print `[PASS]` with pinned versions (`numpy 2.1.3`, `pandas 2.2.3`, `scikit-learn 1.6.1`, `matplotlib 3.10.0`, `seaborn 0.13.2`, `pyyaml 6.0.2`, `joblib 1.4.2`, `pydantic 2.10.6`, `python-dotenv`).
  - All 4 dataset files under `data/` and `configs/train_config.yaml` print `[PASS]`.
  - Write test creates `artifacts/.smoke_test_ok` and prints `You are 100% ready for Day 6!`.
- **Trainer Tip:** If a student ran Jupyter from the wrong directory (e.g., parent folder instead of `notebooks/`), `Path.cwd()` will reveal it immediately.

---

## Coding Sample 1 (Live Demo): ML Lifecycle & Project Structure
- **Notebook:** `notebooks/01_demo_ml_lifecycle_and_project_structure.ipynb`
- **Timebox:** 20 minutes demo + 10 minutes pair discussion
- **What to Highlight Line-by-Line:**
  1. Cell 1: Point out `customers_raw.csv` has **1,003 rows** (1,000 unique + 3 duplicates) and ~30% churn rate.
  2. Cell 3: Walk through the generated folder tree under `artifacts/streampulse_project_demo/` — emphasize why `data/raw` and `data/processed` are separate and why `src/` has `__init__.py` files.
  3. Cell 5: Show how writing `summarize_dataset()` into `src/ingestion/inspector.py` lets any notebook or test import it cleanly in 1 line.
- **Expected Output (Cell 5):**
  ```json
  {
    "rows": 1003,
    "columns": 9,
    "missing_cells": 12,
    "duplicate_rows": 3
  }
  ```
- **10-Minute Pair Discussion Prompt:** *"Look at the 4 numbers in `summary` above (`1003` rows, `12` missing cells, `3` duplicate rows). What could go wrong if you jumped straight into `train_test_split` without fixing them?"*
  - Guide pairs to spot: duplicate rows leaking between train and test, NaNs crashing Logistic Regression, and messy strings (`" Basic "` vs `"basic"`) creating duplicate one-hot columns.

---

## Coding Sample 2 (Live Demo): Data Ingestion & Data Validation
- **Notebook:** `notebooks/02_demo_data_ingestion_and_validation.ipynb`
- **Timebox:** 20 minutes
- **What to Highlight Line-by-Line:**
  1. **Raw messiness inspection:** Show `" Basic "`, `"PREMIUM"`, `" Standard"`, `" us "`, `"in"` in value counts before normalization.
  2. **Preserving `NaN` during string cleaning:** Point out `lambda x: x.strip().lower() if isinstance(x, str) else x` — calling `.astype(str)` carelessly would turn real `NaN`s into the literal string `"nan"`!
  3. **`validate="one_to_one"` in `pd.merge`:** Explain that this Pandas guard prevents silent row multiplication if duplicates sneak through. Also note the orphan record `"CUST-99998"` in `usage_events.json` is safely excluded by `how="left"`.
  4. **Data Contract on Clean vs. Corrupted Batch:**
     - Clean ingested dataset → `"passed": true`, `error_count: 0`, `3` informational warnings for small missingness (< 1%) that our pipeline will impute.
     - `customers_corrupted_batch.csv` → `"passed": false`, catches **all 5 error types** (age outside `[18, 100]`, negative `tenure_months`, `monthly_charges` outside `[0, 100]`, unexpected `plan_tier` `'ultra_vip'`, invalid `churned` label `2`) and saves `artifacts/validation_report_demo.json`.
  5. **Pydantic preview:** Show the crystal-clear 3 field-level validation errors on `bad_payload`.
- **"What If We Break It?" Moment:** Temporarily drop `"age"` from `corrupted_df` (`corrupted_df.drop(columns=["age"])`) and re-run `validate_customer_dataframe` live to show the fail-fast missing-column check firing immediately.

---

## Real-Time Exercise 1: Data Ingestion, Validation & Row Quarantine
- **Starter Notebook:** `notebooks/03_exercise_data_ingestion_and_validation_starter.ipynb`
- **Solution Notebook:** `notebooks/03_exercise_data_ingestion_and_validation_solution.ipynb`
- **Timebox:** 25 minutes (20 min coding + 5 min debrief)
- **Business Scenario:** Clean and deduplicate `data/incoming_batch_exercise.csv` (122 rows), then split valid customers from 4 corrupted rows into `(valid_df, quarantine_df)`.

### Tiered Hints for Struggling Freshers
- **Hint 1 (Gentle Nudge):** In Task 1, remember Pandas string accessors `.str.strip()`, `.str.lower()`, and `.str.upper()` handle strings cleanly while preserving `NaN`s! Use `.drop_duplicates(subset=["customer_id"], keep="first")`.
- **Hint 2 (Structural Hint):** In Task 2, build 5 separate boolean Series (`rule_age`, `rule_tenure`, `rule_charges`, `rule_plan`, `rule_country`) and combine them with bitwise `&` (not Python's `and`!). Remember `~is_valid` selects the failing rows!
- **Hint 3 (Near-Solution):**
  ```python
  is_valid = (
      df["age"].between(18, 100)
      & (df["tenure_months"] >= 0)
      & df["monthly_charges"].between(0, 100)
      & df["plan_tier"].isin(ALLOWED_PLANS)
      & df["country"].isin(ALLOWED_COUNTRIES)
  )
  ```

### Expected Output
- `Cleaned rows: 120` (2 exact duplicates removed from 122 raw rows)
- `Valid rows: 116`
- `Quarantined rows: 4` (rows with `age=10`, `tenure_months=-3`, `monthly_charges=-15`, and `plan_tier='enterprise'`)
- All assertions pass and print `[ALL TESTS PASSED]`.

### Debrief Talking Points
1. Why did we normalize `plan_tier` and `country` **before** validating? (Otherwise `" BASIC "` and `"uk"` would have been falsely quarantined!)
2. Common fresher bug to call out: writing `and` instead of `&` between Pandas Series raises `ValueError: The truth value of a Series is ambiguous` — explain parentheses around comparisons + `&`.

### Bonus Stretch Task
Add a `failure_reason` column explaining which rule(s) each quarantined row broke and save `quarantine_df` to `artifacts/quarantined_batch.csv` (included in `_solution.ipynb`).

---

## Coding Sample 3 (Live Demo): Feature Engineering & Data Leakage Prevention
- **Notebook:** `notebooks/04_demo_feature_engineering_and_leakage.ipynb`
- **Timebox:** 20 minutes
- **What to Highlight Line-by-Line:**
  1. **Part 1 (Leakage Proof):** Point to the printed numbers: the leaky scaler's mean (~13.0) was pulled upward by the test set (centered at 25), whereas the clean train-only scaler stays honestly near ~10.0.
  2. **Part 2 (Domain Features):** Show the `groupby("churned").mean()` table — churned customers have noticeably higher `charges_per_login`, higher `tickets_per_tenure_month`, and higher `is_new_customer` rates!
  3. **Part 3 (`ColumnTransformer`):**
     - Transforms 15 raw/derived columns into **27 clean numeric features** with **zero NaNs**.
     - Stress-test cell: passing `country = "JP"` (unseen) and `age = np.nan` succeeds cleanly without crashing because of `SimpleImputer` + `OneHotEncoder(handle_unknown="ignore")`.
- **"What If We Break It?" Moment:** Change `handle_unknown="ignore"` to `handle_unknown="error"` for 10 seconds and re-run the last cell so freshers see the exact `ValueError: Found unknown categories ['JP']` crash they just prevented!

---

## Real-Time Exercise 2: Domain Features & Leak-Free `ColumnTransformer`
- **Starter Notebook:** `notebooks/05_exercise_feature_engineering_starter.ipynb`
- **Solution Notebook:** `notebooks/05_exercise_feature_engineering_solution.ipynb`
- **Timebox:** 25 minutes (20 min coding + 5 min debrief)

### Tiered Hints for Struggling Freshers
- **Hint 1 (Gentle Nudge):** For `high_ticket_flag`, `(out["support_tickets_30d"] >= 3).astype(int)` converts `True/False` cleanly into `1/0`. Always add `+ 1.0` in denominators to avoid division by zero.
- **Hint 2 (Structural Hint):** Inside `build_preprocessor`, build two small `Pipeline` objects (`numeric_pipe` and `categorical_pipe`) and pass them as tuples `("num", numeric_pipe, numeric_cols)` and `("cat", categorical_pipe, categorical_cols)` inside `ColumnTransformer(transformers=[...])`.
- **Hint 3 (Near-Solution):** Remember to pass `sparse_output=False` to `OneHotEncoder(handle_unknown="ignore", sparse_output=False)` so the transformed output is a dense NumPy array compatible with `np.isnan(...)`.

### Expected Output
- `train=(800, 26), test=(200, 26)`
- Zero NaNs in both train and test matrices.
- Probe row with `plan_tier="future_tier_2027"` and `monthly_charges=np.nan` transforms cleanly.
- Prints `[ALL TESTS PASSED]`.

### Debrief Talking Points
1. Why did `X_train` and `X_test` grow from 14 columns to 26 columns? (One-hot encoding expanded `plan_tier`, `country`, `payment_method`, and `last_active_device` into binary indicator columns!)
2. Ask: *"If we saved only `X_train_t` and threw away `preprocessor`, could we predict on a new customer tomorrow?"* — Sets up Module 4 (`Pipeline` + `joblib`) perfectly!

### Bonus Stretch Task
Create a `tenure_bucket` categorical feature with `pd.cut(...)`, add it to `CATEGORICAL_COLS`, and inspect `preprocessor.get_feature_names_out()`.

---

## Coding Sample 4 (Live Demo): End-to-End Training & Testing Pipeline
- **Notebook:** `notebooks/06_demo_training_and_testing_pipeline.ipynb`
- **Timebox:** 20 minutes
- **What to Highlight Line-by-Line:**
  1. **Stratified Split:** Verify Train (`800` rows) and Test (`200` rows) both have `29.5%` churn rate.
  2. **`FunctionTransformer(add_domain_features)`:** Emphasize that placing feature creation inside `Pipeline` means raw inputs at prediction time automatically get engineered features!
  3. **5-Fold CV Comparison (Expected Numbers):**
     - `LogisticRegression`: CV ROC-AUC `~0.8207`
     - `RandomForest`: CV ROC-AUC `~0.8193`
     - `GradientBoosting`: CV ROC-AUC `~0.8129`
     - *Teaching gold:* Point out that a clean, regularized linear model with strong domain features beats or matches tree ensembles — never skip simple baselines!
  4. **Final Held-Out Test Evaluation:** Logistic Regression achieves Test ROC-AUC `~0.8134`, F1 `~0.5942`. Walk through the side-by-side Confusion Matrix and ROC Curve plot.
  5. **Save, Reload & Smoke Test:**
     - Saves `artifacts/demo_churn_pipeline.joblib` + `artifacts/demo_churn_metadata.json`.
     - Reloaded pipeline scores Customer #1 (new, 4 tickets, 2 logins, no auto-pay) at **`~100.0%` HIGH RISK** and Customer #2 (loyal, 36m tenure, 26 logins, 0 tickets, auto-pay) at **`~3.0%` LOW RISK**.

---

## Real-Time Exercise 3: Build, Cross-Validate, Test & Serialize a Pipeline
- **Starter Notebook:** `notebooks/07_exercise_training_pipeline_starter.ipynb`
- **Solution Notebook:** `notebooks/07_exercise_training_pipeline_solution.ipynb`
- **Timebox:** 25 minutes (20 min coding + 5 min debrief)

### Tiered Hints for Struggling Freshers
- **Hint 1 (Gentle Nudge):** Look closely at how `cross_val_score` works: `cross_val_score(pipeline, X_train, y_train, cv=cv, scoring="roc_auc").mean()` handles fitting and scoring across all 5 folds automatically — do NOT fit `pipeline` before calling `cross_val_score` (actually `cross_val_score` clones it, so fitting afterwards on full `X_train` is clean and explicit!).
- **Hint 2 (Structural Hint):** For `roc_auc_score(y_test, y_prob)`, remember to pass **predicted probabilities for class 1**: `y_prob = pipeline.predict_proba(X_test)[:, 1]`, NOT hard `0/1` predictions from `pipeline.predict(X_test)`!
- **Hint 3 (Near-Solution):**
  ```python
  y_pred = pipeline.predict(X_test)
  y_prob = pipeline.predict_proba(X_test)[:, 1]
  metrics = {
      "cv_roc_auc": round(cv_roc_auc, 4),
      "test_roc_auc": round(float(roc_auc_score(y_test, y_prob)), 4),
      "test_f1": round(float(f1_score(y_test, y_pred)), 4),
  }
  ```

### Expected Output (RandomForest Solution)
- `{'cv_roc_auc': 0.8203, 'test_roc_auc': 0.8006, 'test_f1': 0.531}`
- `artifacts/ex3_pipeline.joblib` exists and `np.allclose(orig_probs, reloaded_probs)` passes!
- **Bonus Threshold Output:**
  - `threshold=0.35 -> precision=0.500 | recall=0.814 | f1=0.619`
  - `threshold=0.50 -> precision=0.556 | recall=0.508 | f1=0.531`
  - `threshold=0.65 -> precision=0.630 | recall=0.288 | f1=0.395`

### Debrief Talking Points
1. Show the bonus threshold table on the projector! Ask: *"Notice we didn't retrain the model at all — just moving the decision threshold from `0.50` to `0.35` raised Recall from 51% to 81% and improved F1 from 0.53 to 0.62!"*
2. Explain why `roc_auc_score` needs `predict_proba()[:, 1]` while `f1_score` needs binary `predict()` labels.

---

## Coding Sample 5 (Live Demo): Config Management, Reproducibility & Experiment Tracking
- **Notebook:** `notebooks/08_demo_experiments_reproducibility_and_config.ipynb`
- **Timebox:** 30 minutes
- **What to Highlight Line-by-Line:**
  1. Open `configs/train_config.yaml` side-by-side in a second JupyterLab tab so freshers see YAML syntax cleanly.
  2. **Reproducibility Proof (Cell 5):**
     - `Run A (seed=42) vs Run B (seed=42) exact match?: True`
     - `Run A (seed=42) vs Run C (seed=99) exact match?: False`
  3. **Experiment Ledger (Cell 7–8):**
     - `baseline_logreg` → CV AUC `0.8207`, Test AUC `0.8134`, F1 `0.5942`
     - `rf_depth6` → CV AUC `0.8193`, Test AUC `0.7954`, F1 `0.5217`
     - `gb_lr05` → CV AUC `0.8129`, Test AUC `0.8024`, F1 `0.5102`
     - Open `artifacts/demo_experiments.jsonl` in JupyterLab text viewer so freshers see the raw JSON lines containing `run_id`, `data_fingerprints`, `sklearn_version`, `params`, `metrics`, and `artifact_path`.
  4. **MLflow Comparison (Part 4):** Emphasize that tools like MLflow or Weights & Biases add a UI, team database, and model registry on top of the exact 4 things we just logged!

---

## Day 6 Capstone Mini-Challenge: Complete Config-Driven ML Pipeline
- **Starter Notebook:** `notebooks/09_capstone_end_to_end_pipeline_starter.ipynb`
- **Solution Notebook:** `notebooks/09_capstone_end_to_end_pipeline_solution.ipynb`
- **Timebox:** 30 minutes (16:15 – 16:45)

### Challenge Objectives
Wire 4 clean stage functions driven exclusively by `configs/train_config.yaml`:
1. `stage_ingest(cfg)` → `(merged_df, data_hash)`
2. `stage_validate(df, cfg)` → `{"passed": True, "rows": ...}` (raising `ValueError` if rules fail)
3. `stage_engineer_features(df)` → DataFrame with all 4 derived features
4. `stage_train_and_log(df, data_hash, cfg, log_path)` → runs CV + test eval, saves `artifacts/capstone_<run_id>.joblib`, appends JSONL experiment record, returns record dict

### Capstone Completion Rubric (Self-Checked Automatically)

| Criterion | Verification Check | Pass Requirement |
| --- | --- | --- |
| Config-Driven Ingestion | Uses `cfg["data"]`, deduplicates IDs, returns 12-char SHA-256 hash | `len(data_hash) == 12` & unique IDs |
| Automated Validation | Checks bounds & allowed tiers from `cfg["validation"]` | `val_report["passed"] is True` |
| Feature Engineering | Creates all 4 configured domain features with zero NaNs/inf | Columns match `cfg["features"]["numeric_columns"]` |
| Pipeline & Metrics | Stratified split + CV + test evaluation | `cv_roc_auc >= 0.72` and `test_roc_auc >= 0.72` |
| Artifact & Tracking | Saves `.joblib`, reloads & predicts probabilities in `[0, 1]`, writes JSONL | Artifact & JSONL file exist on disk |

### Expected Capstone Output (Default `train_config.yaml` — `random_forest`)
- `cv_roc_auc`: `0.8193`
- `test_roc_auc`: `0.7954`
- `test_f1`: `0.5217`
- Reloaded sample churn probabilities on first 3 customers: `[0.31, 0.714, 0.612]`
- Prints **`[CAPSTONE COMPLETE!]`**.

### Stretch Experiment for Fast Finishers
Edit `configs/train_config.yaml` to switch `model.type` from `"random_forest"` to `"logistic_regression"` (with `params: {C: 0.5, max_iter: 1000, class_weight: "balanced"}`), re-run the Capstone notebook **without editing any Python code**, and compare the two rows inside `artifacts/capstone_experiments.jsonl`!
