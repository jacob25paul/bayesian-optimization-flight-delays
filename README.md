# Bayesian Optimization for LightGBM

Springboard case study applying Gaussian-process Bayesian optimization to flight-delay classification, with feature engineering, cross-validation and visualizations.

## Run locally

Use Python 3.12, install dependencies with `python -m pip install -r requirements.txt`, and launch JupyterLab. Open `Bayesian_Optimization_Completed.ipynb`, restart the kernel and run all cells. Keep the two CSVs in `data/`. The input loader also supports CSVs beside the notebook or in `../data/`.

## Contents

- Completed notebook with all prompts answered, saved outputs and figures.
- `data/flight_delays_train.csv` and `data/flight_delays_test.csv`: original supplied files.
- `outputs/`: 100,000 unlabeled-test probabilities in original row order, seven-trial search history, baseline comparison and selected parameters. These outputs can be regenerated.
- `requirements.txt`: tested package versions.

## Method

Reserve 20% of the 100,000 labeled rows for holdout evaluation. Optimize mean ROC-AUC on three fixed stratified development folds, fitting category maps and frequency features independently per fold. Run the required 2 random initial points plus 5 Bayesian steps using UCB. Fit an all-labeled model after evaluation to predict the unlabeled file.

Corrections to the starter include HHMM-to-minute cyclic encoding, fold-local learned features, removal of duplicate LightGBM parameter aliases, reproducible seeds, capped thread use and early stopping. Search bounds and boosting budget are explicitly reduced to practical laptop-sized values; this is an exploratory finite search.

## Results

- Selected mean CV ROC-AUC: 0.71974.
- Reserved holdout ROC-AUC: 0.72594; baseline: 0.72573.
- Selected holdout average precision: 0.40730.
- Effective settings: 50 leaves, max depth 10, L1 5.0, L2 3.97226, minimum leaf data 73; final training budget 37 boosting rounds.

The AUC improvement over the baseline is tiny and does not establish a meaningful or statistically significant advantage. At threshold 0.5, delayed-class recall is only about 6%, despite roughly 82% accuracy. The supplied test file has no labels; no test accuracy is claimed. Whether departure time is scheduled or actual must be verified before using this as an advance-prediction model. Random-split evaluation does not establish future-flight performance.

## Verification and submission

All 24 code cells were executed twice in fresh Python/IPython processes, with integrity checks and outputs saved. This does not reproduce your exact Mac/Jupyter environment. Plots and notebook schema were inspected.

Upload the extracted files to a public GitHub repository and submit the completed notebook URL. No GitHub upload has been performed here. Generated `outputs/` files are optional for the submission; the notebook recreates them.

Suggested repository name: `bayesian-optimization-flight-delays`.

Repository description: Tune a LightGBM flight-delay classifier with Bayesian optimization, cross-validation, and performance visualizations.

Prepared with AI assistance; review the code and findings before your mentor discussion.
