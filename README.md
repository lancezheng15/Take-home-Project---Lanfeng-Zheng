# Income Classification & Customer Segmentation Project

## Project Overview

This project delivers two analytical solutions using U.S. Census Bureau data:

1. **Income Classification**: Predict income **>50K** vs **≤50K** using ~40 demographic and employment variables, with proper use of Census **sample weights**.
2. **Customer Segmentation**: Build an unsupervised segmentation model (PCA + KMeans) and profile the resulting groups for targeted marketing.

The main analysis is in **`Analysis.ipynb`**.

---

## Data

- **Source**: Census Bureau data (`census-bureau.data` + `census-bureau.columns`).
- **Size**: 199,523 rows × 42 columns.
- **Target**: Binary label (e.g. `50000+.` vs `- 50000.`); positive rate ~**6%** (severe class imbalance).
- **Weights**: Sample weights are used for training and evaluation to reflect population representativeness.

---

## Key Results

### Classification (Random Forest)

- **Test ROC-AUC**: 0.949  
- **Test PR-AUC**: 0.656  
- **Business metrics (weighted)**:
  - Top 1%: Precision 93.3%, Lift **15.0x**
  - Top 5%: Precision 66.8%, Lift **10.8x**
  - Top 10%: Precision 46.0%, Lift **7.4x**
  - Top 20%: Precision 28.2%, Lift **4.5x**

The model is well-suited for ranking and targeting (e.g. top-K for campaigns), with strong generalization and low overfitting risk.

### Top Feature Drivers (Classification)

Among the most important features: *dividends from stocks*, *detailed occupation recode*, *capital gains*, *age*, *log capital gains*, *detailed industry recode*, *weeks worked in year*, *worked full year*, and *major occupation code* (e.g. Executive/managerial).

### Segmentation (K = 6)

| Segment | Description | Approx. size | Income rate |
|--------|-------------|--------------|-------------|
| **3** | High-income professionals (executive/managerial, capital gains, advanced education) | ~7.4k | **32.7%** |
| **0 & 2** | Middle-class working professionals (full-time, retail/manufacturing/professional) | ~46k each | ~10% |
| **4 & 5** | Older, low labor participation (retired / inactive) | ~26k each | ~1–2% |
| **1** | Children / non-working dependents | ~47.6k | 0% |

Segmentation supports **segment-based campaign design**, **budget allocation by ROI**, **personalization**, and **product/inventory planning** (see notebook for full marketing use cases and a worked campaign example).

---

## Setup & Running the Analysis

### 1. Environment

```bash
conda create -n income_segmentation python=3.9 -y
conda activate income_segmentation
```

### 2. Dependencies

```bash
pip install --upgrade pip
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

(Or use the versions listed in the Dependencies section below.)

### 3. Data

Place (or keep) in the project root:

- `census-bureau.data`
- `census-bureau.columns`

Ensure `Analysis.ipynb` is run from the project root so paths to these files resolve correctly (or set `DATA_FILE` and `COLUMNS_FILE` in the notebook to your paths).

### 4. Run the notebook

```bash
jupyter notebook Analysis.ipynb
```

Or open `Analysis.ipynb` in JupyterLab / VS Code and run all cells.

---

## Project Structure

```
.
├── Analysis.ipynb           # Main analysis: classification + segmentation
├── census-bureau.data       # Census data
├── census-bureau.columns    # Column names
├── README.md
└── requirements.txt         # (optional) pip install -r requirements.txt
```

---

## Notebook Outline

1. **Load data** – Read census data and infer label/weight columns.
2. **Target & weights** – Binary target (>50K), sample weights; note class imbalance.
3. **EDA** – Numeric and categorical summaries and implications for modeling.
4. **Feature engineering & preprocessing** – Log transforms, binary indicators (e.g. capital gain, full-year work), train/val/test (70/15/15), pipelines (numeric: median impute + scale; categorical: one-hot).
5. **Classification** – Logistic Regression (class_weight=balanced) and Random Forest; weighted ROC-AUC and PR-AUC; business metrics (precision/recall/lift at top 1%, 5%, 10%, 20%); feature importances.
6. **Segmentation** – PCA + KMeans (k=6); segment profiles and business interpretation; how a retail client can use segments for marketing (campaign design, budget allocation, personalization, inventory, cross-sell) and a practical campaign example with budget allocation.

---

## Dependencies

- pandas ≥ 1.5.0  
- numpy ≥ 1.23.0  
- scikit-learn ≥ 1.2.0  
- matplotlib ≥ 3.6.0  
- seaborn ≥ 0.12.0  
- jupyter ≥ 1.0.0  

(Optional, if you use them elsewhere: category-encoders, xgboost, lightgbm, shap.)
