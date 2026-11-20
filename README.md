# Day 6 Master Trainer Runbook — End-to-End ML Engineering & Pipelines

**Program:** AI/ML & Generative AI Training Focus (Week 2–4)
**Target Audience:** Freshers (entry-level graduates / junior engineers with Days 1–5 Python, Pandas, and basic scikit-learn exposure)
**Total Duration:** 8 Hours (09:00 – 17:00, including 2 tea breaks & lunch; 6 hours 45 minutes active instruction & labs)
**Unifying Case Study:** **StreamPulse Subscription Churn Prediction** (100% offline-ready synthetic CRM + product usage dataset — zero paid API keys or cloud accounts required)

---

## 1. Where Day 6 Fits in the Curriculum

- **Bridge from Week 1 (Days 1–5):** In Days 1–5, freshers learned Python, Pandas, exploratory data analysis, and basic `model.fit()` / `model.predict()` inside standalone exploratory notebooks. Today marks the leap from **"student notebook scripts"** to **structured, repeatable, team-ready ML engineering pipelines**.
- **Day 6 Exact Syllabus Coverage:**

| Syllabus Category | Topic from Curriculum | Module | Notebooks |
| --- | --- | --- | --- |
| **Mandatory (In Depth)** | ML Lifecycle | Module 1 | `01_demo_ml_lifecycle_and_project_structure.ipynb` |
| **Mandatory (In Depth)** | Project Structure | Module 1 | `01_demo_ml_lifecycle_and_project_structure.ipynb` |
| **Mandatory (In Depth)** | Data Ingestion | Module 2 | `02_demo_data_ingestion_and_validation.ipynb`, `03_exercise_*` |
| **Mandatory (In Depth)** | Data Validation | Module 2 | `02_demo_data_ingestion_and_validation.ipynb`, `03_exercise_*` |
| **Mandatory (In Depth)** | Feature Engineering | Module 3 | `04_demo_feature_engineering_and_leakage.ipynb`, `05_exercise_*` |
| **Mandatory (In Depth)** | Training & Testing Pipeline | Module 4 | `06_demo_training_and_testing_pipeline.ipynb`, `07_exercise_*` |
| **Introduction (High Level)** | Experiment Tracking | Module 5 | `08_demo_experiments_reproducibility_and_config.ipynb`, `09_capstone_*` |
| **Introduction (High Level)** | Reproducibility | Module 5 | `08_demo_experiments_reproducibility_and_config.ipynb`, `09_capstone_*` |
| **Introduction (High Level)** | Configuration Management | Module 5 | `08_demo_experiments_reproducibility_and_config.ipynb`, `09_capstone_*` |

- **Bridge to Day 7 & Day 8:** Tomorrow (Day 7) builds directly on today's pipeline by adding **Data Drift, Concept Drift, Monitoring Metrics, Retraining Strategy, Feature Stores, and Training-Serving Skew**. Day 8 adds **Explainable AI (SHAP), Model Deployment, Fairness, and Governance**.

---

## 2. Measurable Learning Objectives

By 17:00 today, every fresher will be able to:
1. Explain all **7 stages of the ML Lifecycle** and organize an ML codebase into clean folders (`src/`, `configs/`, `data/`, `artifacts/`, `tests/`) instead of a single monolithic notebook.
2. Ingest multi-source data (**CSV + JSON**), standardize messy text/casing, deduplicate primary keys, and record SHA-256 data lineage hashes without mutating raw files.
3. Implement automated **Data Validation contracts** (schema, uniqueness, missingness thresholds, numeric ranges, allowed categories) to fail fast or quarantine corrupted records.
4. Engineer domain-driven features and assemble a **leak-free `ColumnTransformer`** (`SimpleImputer`, `StandardScaler`, `OneHotEncoder(handle_unknown="ignore")`) fitted strictly on training data.
5. Build, cross-validate (`StratifiedKFold`), evaluate on a held-out test set, serialize (`joblib`), and reload-predict with an end-to-end **`sklearn.pipeline.Pipeline`**.
6. Explain at a high level how **Configuration Management (YAML)**, **Reproducibility (seeds + hashes + Docker)**, and **Experiment Tracking (run ledgers / MLflow)** work together, and apply them in a complete capstone pipeline.

---

## 3. Minute-by-Minute 8-Hour Classroom Schedule (09:00 – 17:00)

| Time | Duration | Block | Activity & Format | Slides / Notebooks |
| --- | --- | --- | --- | --- |
| **09:00 – 09:15** | 15 min | **Welcome & Setup Check** | Recap Days 1–5; introduce StreamPulse scenario; everyone launches Docker JupyterLab and runs `00_environment_check.ipynb` (green sticky note when all checks print `PASS`). | Slides 1–3; `00_environment_check.ipynb` |
| **09:15 – 09:45** | 30 min | **Module 1: Concepts** | **ML Lifecycle & Production Project Structure** — why ad-hoc notebooks break in teams; the 7-stage lifecycle; mapping lifecycle stages to `src/`, `configs/`, `data/`, `artifacts/`, `tests/`. | Slides 4–9 |
| **09:45 – 10:15** | 30 min | **Module 1: Live Demo & Mini-Activity** | Live walk-through of `01_demo_ml_lifecycle_and_project_structure.ipynb`; scaffold a modular project and import a reusable helper; 10-min pair discussion ("spot 5 bugs in a spaghetti notebook"). | `01_demo_ml_lifecycle_and_project_structure.ipynb` |
| **10:15 – 10:30** | 15 min | **Morning Tea Break** | Stretch, coffee/tea; trainer helps anyone still resolving Docker or laptop issues. | — |
| **10:30 – 11:00** | 30 min | **Module 2: Concepts** | **Data Ingestion & Data Validation** — multi-source joins, raw immutability, data contracts, schema/range/category checks, fail-fast vs. row quarantine, intro to Pydantic row validation. | Slides 10–16 |
| **11:00 – 11:20** | 20 min | **Module 2: Live Demo** | Live coding of `02_demo_data_ingestion_and_validation.ipynb` — ingest CSV+JSON, run validator on clean data, then break it on `customers_corrupted_batch.csv` and inspect the JSON audit report. | `02_demo_data_ingestion_and_validation.ipynb` |
| **11:20 – 11:45** | 25 min | **Exercise 1 (Hands-On Lab)** | Real-time lab: participants clean, deduplicate, and quarantine 4 bad rows in `incoming_batch_exercise.csv` until all `assert` self-checks pass. 5-min group debrief. | `03_exercise_data_ingestion_and_validation_starter.ipynb` |
| **11:45 – 12:15** | 30 min | **Module 3: Concepts** | **Feature Engineering & Leakage Prevention** — domain features (ratios, flags, rates), missing value imputation, encoding, scaling, and the #1 fresher trap: **Data Leakage**. | Slides 17–23 |
| **12:15 – 12:35** | 20 min | **Module 3: Live Demo** | Live coding of `04_demo_feature_engineering_and_leakage.ipynb` — demonstrate leaky vs. clean scaling, create 4 churn features, build `ColumnTransformer`, and stress-test with an unseen country `"JP"`. | `04_demo_feature_engineering_and_leakage.ipynb` |
| **12:35 – 13:00** | 25 min | **Exercise 2 (Hands-On Lab)** | Real-time lab: engineer 3 churn features + assemble a leak-free `ColumnTransformer` passing missing-value & unseen-category tests. 5-min debrief before lunch. | `05_exercise_feature_engineering_starter.ipynb` |
| **13:00 – 13:45** | 45 min | **Lunch Break** | Full disconnect & recharge! | — |
| **13:45 – 14:15** | 30 min | **Module 4: Concepts** | **Training & Testing Pipeline** — `sklearn.pipeline.Pipeline`, stratified train/test splits, Stratified K-Fold CV, choosing metrics for imbalanced churn (ROC-AUC, Precision, Recall, F1), artifact serialization (`joblib`) & inference smoke tests. | Slides 24–30 |
| **14:15 – 14:35** | 20 min | **Module 4: Live Demo** | Live coding of `06_demo_training_and_testing_pipeline.ipynb` — compare Logistic Regression, Random Forest, and Gradient Boosting via 5-fold CV, evaluate winner on held-out test set, save `.joblib` + metadata JSON, reload and score 2 new customers. | `06_demo_training_and_testing_pipeline.ipynb` |
| **14:35 – 15:00** | 25 min | **Exercise 3 (Hands-On Lab)** | Real-time lab: assemble, cross-validate, test, save, and reload an end-to-end `Pipeline`, verifying bit-identical predictions after reload. Bonus: threshold tuning (`0.35` vs `0.50` vs `0.65`). | `07_exercise_training_pipeline_starter.ipynb` |
| **15:00 – 15:15** | 15 min | **Afternoon Tea Break** | Coffee/tea; trainer checks progress on Exercises 1–3. | — |
| **15:15 – 15:45** | 30 min | **Module 5: High-Level Overview** | **Configuration Management, Reproducibility & Experiment Tracking** — why magic numbers fail, YAML configs, the 4 pillars of reproducibility (seed, data hash, config, Docker), experiment run ledgers, and how MLflow/W&B work in industry. | Slides 31–36 |
| **15:45 – 16:15** | 30 min | **Module 5: Live Demo** | Live walk-through of `08_demo_experiments_reproducibility_and_config.ipynb` — load `configs/train_config.yaml`, prove seed determinism (`True` vs `False`), log 3 tracked runs to JSONL, plot leaderboard, and review 1-to-1 MLflow code mapping. | `08_demo_experiments_reproducibility_and_config.ipynb` |
| **16:15 – 16:45** | 30 min | **Day 6 Capstone Mini-Challenge** | Real-time synthesis lab: participants wire `YAML Config → Ingest → Validate → Feature Engineering → Train/Test Pipeline → Experiment Log → Save Artifact` in `09_capstone_end_to_end_pipeline_starter.ipynb`. | `09_capstone_end_to_end_pipeline_starter.ipynb` |
| **16:45 – 17:00** | 15 min | **Wrap-Up, Quiz & Bridge to Day 7** | 5-question rapid-fire recap quiz, open Q&A, show Capstone solution, and tease Day 7 ("What happens 3 months after we deploy this pipeline and customer behavior changes? → Drift & Monitoring!"). | Slides 37–39; `09_capstone_end_to_end_pipeline_solution.ipynb` |

**Total Time Check:** 15 + 60 + 15 + 75 + 75 + 45 + 75 + 15 + 60 + 30 + 15 = **480 minutes (exactly 8 hours)**.

---

## 4. Pre-Class Trainer Checklist

### T-7 Days (One Week Before)
- [ ] Share [`docker-jupyterlab/DOCKER_SETUP.md`](docker-jupyterlab/DOCKER_SETUP.md) and ask all freshers to install Docker Desktop (Windows WSL2 / macOS / Linux).
- [ ] Ask participants to run `docker compose build` on home/office broadband so image downloads finish before classroom day.
- [ ] Confirm projector resolution (1080p recommended) and increase JupyterLab font size (`Settings → Theme → Increase Code Font Size` to 16–18px).

### T-1 Day (Evening Before)
- [ ] On the trainer laptop, run `./start.sh` (or `start.bat`), open `http://localhost:8888/lab?token=day6-training`, and execute [`notebooks/00_environment_check.ipynb`](notebooks/00_environment_check.ipynb).
- [ ] Prepare an offline backup USB drive containing `day6-jupyterlab-image.tar` (via `docker save`) + the entire `day6/` repository folder in case classroom Wi-Fi drops.
- [ ] Stock green and red sticky notes (or digital equivalent: Zoom/Teams green check vs. raised hand) for silent lab progress tracking.

### T-30 Minutes (Morning of Day 6)
- [ ] Start JupyterLab container and keep `00_environment_check.ipynb` open on the projector with the URL & token (`day6-training`) written on the whiteboard.
- [ ] Delete any leftover `notebooks/artifacts/` folder from yesterday's dry run so the live demos create fresh artifacts cleanly in front of the class.

---

## 5. Fresher Facilitation Playbook

1. **Use the Sticky-Note System during Labs:**
   - **Green sticky note on laptop lid** = "All self-checks passed / ready to continue."
   - **Red sticky note on laptop lid** = "Stuck — need trainer or TA help."
   - This eliminates freshers feeling shy about raising hands and lets you gauge classroom completion in 2 seconds.
2. **Narrate Errors Out Loud (Normalize Debugging!):**
   - Freshers often panic at red tracebacks. During live demos, intentionally trigger 2 controlled errors (e.g., running the validator on `customers_corrupted_batch.csv`, passing `"ultra_vip"` to Pydantic) and read the **bottom line of the traceback first** so they learn calm debugging habits.
3. **Pacing Fast vs. Struggling Finishers:**
   - Every exercise includes **3 tiered hints** in [`coding-samples-and-exercises.md`](coding-samples-and-exercises.md) (conceptual → structural → near-solution) AND a **Bonus Stretch Task** at the bottom of the notebook so fast finishers stay engaged while others complete the core self-checks.
   - Move to the debrief once **~80% of the room has green sticky notes**; never let the schedule slip more than 5 minutes — share the `*_solution.ipynb` notebook after each lab so everyone starts the next module on equal footing.
4. **Prevent Common Fresher Mistakes Early:**
   - **Mutating `df` in place** without `.copy()` (triggers `SettingWithCopyWarning` or corrupts earlier cells).
   - **Out-of-order cell execution** — remind students to test with **Kernel → Restart Kernel and Run All Cells** before declaring victory.
   - **Calling `fit_transform()` on `X_test`** instead of `transform()` — pause and make the whole class chant: *"Fit on train, transform on test!"*

---

## 6. Complete Day 6 File Guide

Quick links: [`slides-and-explanations.md`](slides-and-explanations.md) · [`coding-samples-and-exercises.md`](coding-samples-and-exercises.md) · [`docker-jupyterlab/DOCKER_SETUP.md`](docker-jupyterlab/DOCKER_SETUP.md) · [`notebooks/`](notebooks/)

```text
day6/
├── .gitignore                                 # Ignores checkpoints, caches, .env & generated artifacts
├── README.md                                  # This Master Trainer Runbook
├── slides-and-explanations.md                 # 39-slide complete deck + diagrams + fresher scripts
├── coding-samples-and-exercises.md            # Live demo scripts, exercise guides, hints & rubrics
├── docker-jupyterlab/
│   ├── Dockerfile                             # Python 3.11-slim + JupyterLab + pinned ML stack
│   ├── docker-compose.yml                     # Mounts ../notebooks -> /workspace/notebooks
│   ├── requirements.txt                       # Exact verified Python dependencies
│   ├── .env.example                           # Port, token, seed & environment defaults
│   ├── start.sh                               # 1-command launcher for macOS / Linux
│   ├── start.bat                              # 1-command launcher for Windows
│   └── DOCKER_SETUP.md                        # Cross-OS setup, offline USB tips & troubleshooting
└── notebooks/
    ├── configs/
    │   └── train_config.yaml                  # YAML config for Module 5 & Capstone
    ├── data/
    │   ├── generate_data.py                   # Deterministic dataset generator (seed=42)
    │   ├── customers_raw.csv                  # 1,003 raw CRM rows (1,000 unique + 3 duplicates)
    │   ├── usage_events.json                  # 1,001 JSON usage records (1,000 + 1 orphan)
    │   ├── customers_corrupted_batch.csv      # Corrupted feed for Data Validation demo
    │   └── incoming_batch_exercise.csv        # 122-row incoming batch for Exercise 1
    ├── 00_environment_check.ipynb             # Start-of-day verification smoke test
    ├── 01_demo_ml_lifecycle_and_project_structure.ipynb
    ├── 02_demo_data_ingestion_and_validation.ipynb
    ├── 03_exercise_data_ingestion_and_validation_starter.ipynb
    ├── 03_exercise_data_ingestion_and_validation_solution.ipynb
    ├── 04_demo_feature_engineering_and_leakage.ipynb
    ├── 05_exercise_feature_engineering_starter.ipynb
    ├── 05_exercise_feature_engineering_solution.ipynb
    ├── 06_demo_training_and_testing_pipeline.ipynb
    ├── 07_exercise_training_pipeline_starter.ipynb
    ├── 07_exercise_training_pipeline_solution.ipynb
    ├── 08_demo_experiments_reproducibility_and_config.ipynb
    ├── 09_capstone_end_to_end_pipeline_starter.ipynb
    └── 09_capstone_end_to_end_pipeline_solution.ipynb
```
