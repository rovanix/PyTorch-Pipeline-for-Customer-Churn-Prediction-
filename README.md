# PyTorch-Pipeline-for-Customer-Churn-Prediction
This repository contains an end-to-end machine learning pipeline to predict telecom customer churn using the Telco Customer Churn dataset, with preprocessing, feature engineering, PyTorch MLP models, and business-oriented evaluation emphasizing recall/F1 for imbalanced data.

### Key features
- Clean preprocessing for mixed-type data, engineered features, and stratified train/test splits to preserve churn ratio.
- Two PyTorch MLP variants: baseline and class-weighted loss to address class imbalance, with clear metric comparisons and confusion matrices.
- Business impact framing with threshold tuning guidance and a simple net-benefit calculator example for decision support.

## Dataset
- Source: Telco Customer Churn; target label mapped as Yes → 1 and No → 0, with CustomerID dropped to avoid leakage.
- Observed imbalance: approximately 26.5% churn positives and 73.5% negatives, motivating recall- and F1-focused evaluation.

## Approach
- EDA: checked missing values, duplicates, target distribution, and basic numeric summaries; TotalCharges required numeric coercion and imputation for blanks.
- Preprocessing:
  - Categoricals: trim whitespace, one‑hot encode, ensure consistent columns across splits.
  - Numericals: median imputation and z‑score scaling.
  - Split: stratified 80/20 train/test to keep class ratios stable.
- Feature engineering: IsNewCustomer from tenure, ChargeRatio = TotalCharges/MonthlyCharges, ContractIsMonthToMonth, and IsAutoPay from PaymentMethod patterns.

## Models and training
- Models: PyTorch MLP baseline vs. PyTorch MLP with class-weighted BCEWithLogitsLoss using positive class weights from training distribution.
- Training details: fixed seeds, Adam optimizer, hidden layers such as 128→64 with dropout 0.3, 20 epochs example loop shown in notebook outputs.
- Thresholding: default 0.5 shown, with additional threshold sweep illustrating precision/recall/F1 trade-offs for operational tuning.

## Results
- Summary (test set):
  - PyTorch MLP weighted: Accuracy ≈ 0.7502, Precision ≈ 0.5195, Recall ≈ 0.7834, F1 ≈ 0.6247.
  - PyTorch MLP default: Accuracy ≈ 0.7899, Precision ≈ 0.6423, Recall ≈ 0.4706, F1 ≈ 0.5432.
- Selection: the class‑weighted model is preferred for deployment due to higher recall and F1, aligning with churn use-cases where false negatives are costlier than some extra outreach to non‑churners.

## Business framing
- Objective: prioritize catching true churners to protect recurring revenue and improve retention ROI in a highly competitive market.
- Example calculator: a simple function computes SavedRevenue, MissedRevenue, UnnecessaryCost, RetentionSpend, and NetBenefit from a confusion matrix given assumed customer value and contact cost; one example shows how net benefit varies with errors and thresholding.
- Guidance: collaborate with retention teams on threshold selection under budget/capacity, monitor false negatives vs. false positives, and periodically recalibrate.
  
## Repository structure
- notebooks/: cleaned notebook “PyTorch_Pipeline_for_Customer_Churn_Prediction.ipynb” without personal identifiers; optional exported HTML.
- src/: reusable modules for data loading, preprocessing, feature engineering, training, evaluation, and inference (recommended refactor of notebook cells).
- data/: empty placeholder with a README pointing to the dataset source; do not commit raw data files.
- results/: saved metrics CSV and confusion matrices as demonstrated (e.g., telco_pytorch_results.csv, cm files).
- requirements.txt or environment.yml: packages include numpy, pandas, scikit‑learn, seaborn, matplotlib, torch, etc., matching imports in the notebook.

## Quickstart
1. Setup environment:
   - Python 3.x with packages: numpy, pandas, scikit‑learn, seaborn, matplotlib, torch, jupyter; install via pip or conda as preferred.
2. Obtain data:
   - Download the Telco Customer Churn CSV to data/ and ensure expected column names (e.g., Churn, TotalCharges) as used in the notebook.
3. Run the notebook:
   - Open notebooks/PyTorch_Pipeline_for_Customer_Churn_Prediction.ipynb, execute cells end‑to‑end to reproduce preprocessing, training, evaluation, and results artifacts in results/.
4. Optional scripts:
   - If refactored into src/ and CLI scripts (train.py, evaluate.py, predict.py), use documented arguments to train and produce metrics/plots into results/.

## Results artifacts
- Metrics CSV: model, accuracy, precision, recall, F1 for the baseline and weighted MLP; the example notebook saves telco_pytorch_results.csv.
- Confusion matrices: saved as plain text arrays for default and weighted models, matched to the evaluation threshold.
- Plots: confusion matrix visualization and optional threshold vs. metrics curves to support threshold selection.

## Roadmap
- Add calibration and cost-sensitive learning to align with business economics across segments and offers.
- Enrich features with behavioral intensity and trend signals (support/billing/contract changes) and add drift monitoring dashboards for production.
- Package the pipeline for inference with consistent preprocessing and a lightweight predict API or CLI.


