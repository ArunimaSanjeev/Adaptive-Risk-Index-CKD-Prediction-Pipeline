# Adaptive Risk Index – CKD Prediction Pipeline

## Project Overview

This project implements a full pipeline for Chronic Kidney Disease (CKD) risk prediction.

### Key Features

- Computes an **Adaptive Risk Index** using clinical, lifestyle, and lab data.  
- Uses **Random Forests** for adaptive feature weighting.  
- Applies **PCA** for dimensionality reduction and **KMeans** for patient clustering.  
- Categorizes patients into **Low, Moderate, High risk**.  
- Provides visualization and **SHAP-based explainable AI** for clinical interpretation.  
- Preliminary ROC-AUC: **0.551**.

---

## Dataset

**Source:** [Kaggle – CKD Dataset](https://www.kaggle.com/datasets/rabieelkharoua/chronic-kidney-disease-dataset-analysis?resource=download)  

- **Samples:** 1659 patients  
- **Features:** 54 (demographics, lifestyle, lab results, medications, symptoms, diagnosis)  

**Preprocessing:**

- Handle missing values (mean/mode)  
- Drop duplicates  
- Outlier handling via IQR capping  
- Label encoding for categorical features  
- Scaling numeric features to [0,1]  

---

## Methodology / System Design

### 1. Feature Engineering
- GFR discretization: Severe, Moderate, Mild, Normal  
- PCA (Top 5 components for optional dimensionality reduction)  

### 2. Adaptive Weighting
Random Forest feature importance used to compute adaptive weights:

| Feature           | Weight |
|------------------|-------|
| GFR               | 0.284 |
| Serum Creatinine  | 0.299 |
| BUN Levels        | 0.217 |
| Hemoglobin Levels | 0.199 |

*Weights normalized to sum to 1.*

### 3. Risk Index Computation
**Formula:** `Risk Index = Σ (Feature Value × Adaptive Weight)`  

**Categories:**
- Low: ≤ 0.33  
- Moderate: 0.34–0.66  
- High: > 0.66  

### 4. Patient Clustering
- Method: **KMeans (3 clusters)**  
- Cluster distribution:
  - Cluster 0: 518 patients  
  - Cluster 1: 647 patients  
  - Cluster 2: 494 patients  

### 5. Explainable AI
- SHAP values for Random Forest model  
- Summary plots and patient-level force plots for interpretation  

### 6. Evaluation
- Train/test split: 80/20 stratified  
- Metrics: ROC-AUC (0.551), Accuracy, F1-score, Confusion Matrix  

### 7. Outputs
- Refined dataset (`Refined_Chronic_Kidney_disease.csv`) including:
  - Risk Index  
  - Risk Category  
  - Patient Cluster assignment  

---

## Results & Discussion

- **Risk Index Distribution:** Most patients fall into Moderate risk category.  
- **Feature Importance:** Serum Creatinine and GFR contributed most.  
- **Clustering:** KMeans identified 3 patient clusters revealing distinct lab profiles.  
- **Evaluation:** Moderate ROC-AUC; improvement possible with more data.  
- **Explainability:** SHAP enables clinician-level interpretation.  

---

## Conclusion & Future Work

- Developed a fully automated CKD risk pipeline.  
- Adaptive weights improve interpretability and reflect real-world clinical relevance.  
- Provides actionable risk categories and patient clustering insights.  
- Future work: Incorporate more data, advanced models, and real-time clinical integration.
