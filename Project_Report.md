# Income Propensity Modeling and Population Segmentation  
## Analytical Report  

---

# 1. Executive Summary

This project develops two complementary analytical components:

1. A supervised machine learning model to estimate the probability that an individual earns more than $50,000 annually.
2. An unsupervised segmentation framework to identify structurally distinct socioeconomic groups within the population.

The final classification model (Random Forest) achieves:

- Test ROC-AUC: 0.949  
- Test PR-AUC: 0.656  
- Lift up to 15x at top 1% targeting  

The segmentation model (PCA + KMeans, k=6) identifies six economically interpretable clusters, including:

- A high-income professional segment (32.7% high earners)
- Stable middle-class workforce segments
- Low-income / low-participation segments
- A clearly separated dependent population

The framework is statistically robust, economically coherent, and operationally deployable subject to governance review.

---

# 2. Problem Framing

The engagement addresses two analytical objectives:

1. Develop a reliable ranking model for high-income classification under severe class imbalance.
2. Identify latent population structure independent of income label.

The modeling approach emphasizes:

- Proper validation discipline
- Metric selection aligned with imbalance
- Economic interpretability
- Business applicability
- Governance awareness

---

# 3. Data Exploration & Preprocessing

## 3.1 Dataset Characteristics

- 40 demographic and employment variables
- Sampling weight column
- Binary income label (>50K vs ≤50K)
- Class imbalance (~6% high-income)

Sampling weights were incorporated during training and evaluation to maintain population representativeness.

---

## 3.2 Exploratory Findings

Key structural observations:

- Capital gains and dividends are highly right-skewed and zero-inflated.
- Weeks worked in year exhibits bimodality (0 vs ~52 weeks).
- Education and occupation strongly correlate with income.
- Age demonstrates nonlinear relationship with income.
- Presence of dependents (very young individuals) structurally separable.

These insights informed both feature engineering and modeling decisions.

---

## 3.3 Feature Engineering

Based on EDA:

Added:

- log(capital gains)
- log(capital losses)
- has_capital_gain (binary)
- worked_full_year (binary)
- log(wage per hour)

Rationale:

- Stabilize heavy-tailed distributions
- Improve separability
- Capture nonlinear employment effects

All preprocessing was executed within sklearn pipelines to prevent leakage.

---

## 3.4 Data Splitting

- 70% Train
- 15% Validation
- 15% Test
- Stratified by income
- Sample weights preserved

Test set held fully out until final evaluation.

---

# 4. Model Architecture & Training

## 4.1 Model Candidates

Two models were evaluated:

- Logistic Regression (baseline linear model)
- Random Forest (nonlinear ensemble model)

Random Forest outperformed Logistic Regression on PR-AUC and was selected for final deployment.

---

## 4.2 Training Details

Random Forest parameters:

- 400 trees
- min_samples_leaf = 2
- Sample weights incorporated
- Class imbalance handled via weighting

Tree-based architecture selected due to:

- Nonlinear modeling capacity
- Robustness to skew
- Minimal scaling sensitivity
- Strong interpretability

Neural networks were not pursued given:

- Tabular data structure
- Interpretability requirements
- Governance complexity vs marginal gain

---

# 5. Evaluation Framework

Due to class imbalance (~6%), evaluation focused on ranking metrics:

- ROC-AUC
- Precision-Recall AUC
- Precision@Top-K
- Lift

Accuracy was intentionally excluded.

---

# 6. Model Performance

## 6.1 Validation Performance

- ROC-AUC ≈ 0.950
- PR-AUC ≈ 0.676

## 6.2 Test Performance

- ROC-AUC: 0.949
- PR-AUC: 0.656

Minimal degradation between validation and test indicates stable generalization and limited overfitting.

---

# 7. Business-Oriented Performance

Precision@Top-K (Test Set):

| Top % | Precision | Lift |
|-------|-----------|------|
| 1%    | 93%       | 15x  |
| 5%    | 67%       | 10.8x|
| 10%   | 46%       | 7.4x |
| 20%   | 28%       | 4.5x |

Interpretation:

- Targeting top 5% captures ~54% of high-income individuals.
- Strong ranking allows dynamic budget allocation.
- Significant improvement over random outreach.

The model is well-suited for targeted acquisition and premium client prioritization.

---

# 8. Model Interpretability

Feature importance and SHAP analysis indicate primary drivers:

- Capital gains / dividends
- Weeks worked in year
- Education attainment
- Occupational classification
- Age

These factors align with economic intuition and reinforce model credibility.

No non-economic variables dominate model behavior.

Interpretability supports compliance and model risk review requirements.

---

# 9. Segmentation Analysis

## 9.1 Objective

Segmentation aims to identify structural socioeconomic clusters independent of income label.

Income was excluded from clustering to preserve unsupervised integrity.

---

## 9.2 Methodology

Steps:

- One-hot encoding
- Standardization
- PCA reduction (30 components; ~89% variance retained)
- KMeans clustering (k=6 via elbow method)

PCA used to:

- Reduce 413-dimensional space
- Improve clustering stability
- Mitigate sparsity effects

---

## 9.3 Segment Profiles

### Segment 3 – High-Income Professionals
- Income rate: 32.7%
- Strong capital income
- Higher education
- Executive/managerial occupations
- Full-time employment

Strategic implication:
Premium client targeting; wealth management.

---

### Segments 0 & 2 – Stable Workforce
- Income rate: ~10%
- Full-time employment
- Moderate education
- Service / retail / manufacturing occupations

Strategic implication:
Mass affluent growth and cross-sell potential.

---

### Segments 4 & 5 – Low Participation / Older Population
- Income rate: ~1–2%
- Lower employment participation
- Older demographic

Strategic implication:
Conservative product strategy; lower acquisition priority.

---

### Segment 1 – Dependents
- Income rate: 0%
- Median age ~7
- No workforce participation

Strategic implication:
Household-level targeting only.

---

## 9.4 Integration with Propensity Model

The classification model provides ranking power.  
The segmentation model provides structural context.

Combined application allows:

- Within-segment targeting
- Segment-specific thresholds
- Differentiated product alignment
- Risk-adjusted engagement

---

# 10. Business Judgment & Model Recommendation

The Random Forest model is recommended due to:

- Strong and stable ranking performance
- Interpretability
- Operational feasibility
- Alignment with governance expectations

Segmentation adds structural intelligence for differentiated strategy.

Both models are suitable for deployment subject to compliance validation and monitoring.

---

# 11. Limitations & Risk Considerations

- Income is proxy for financial capacity; transactional enrichment recommended.
- Demographic variables require fairness review.
- PCA reduces direct feature transparency.
- Cluster stability should be monitored over time.

Recommended next steps:

- Fairness and compliance assessment
- Drift monitoring framework
- Integration with transaction-level data
- Scenario-based threshold calibration


---

# Conclusion

This analysis demonstrates:

- Rigorous modeling discipline
- Strong ranking capability under imbalance
- Economically coherent interpretability
- Meaningful structural segmentation
- Clear business applicability

