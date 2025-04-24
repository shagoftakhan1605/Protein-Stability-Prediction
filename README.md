# 🧬 Novozymes Enzyme Stability Prediction Using XGBoost and Explainable AI

## Overview
This project addresses the **prediction of enzyme thermal stability (Tm)** based on protein sequences and environmental factors. Leveraging **machine learning (XGBoost)** and **Explainable AI (SHAP values)**, we built an interpretable predictive model using data from the **Novozymes Enzyme Stability Kaggle dataset**. 

Missense mutations and environmental factors like pH critically affect enzyme stability, making this a key task in protein engineering and biotechnology.

---

## 📂 Dataset
- **Source**: [Novozymes Enzyme Stability Prediction](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction)
- **Files Used**:
  - `train.csv` — Contains protein sequences, pH values, data sources, and target Tm.
  - `test.csv` — Sequences for prediction.
  - `test_labels.csv` — Labeled Test dataset
  - `wildtype_structure_prediction_af2.pdb` — Protein structure for mutation analysis.
  - Generated: `AF70_mutations.txt` & `wildtype_af70.fasta` for mutation tracking.

- **Target Variable**: `tm` — Melting temperature of enzymes (°C).

---

## 🚀 Methodology

### 1. **Data Engineering**
- Extracted features:
  - **Amino Acid Counts & Fractions** for all 20 standard amino acids.
  - Sequence length (`n_AA`).
  - Environmental pH handling and correction.
  - Encoded data source as categorical numeric.
- Addressed pH anomalies (swapped with Tm where pH > 14).

### 2. **Exploratory Data Analysis (EDA)**
- Visualized:
  - Distribution of `tm` and `pH`.
  - Sequence length variability.
  - Spearman correlations between amino acid composition and stability.

### 3. **Mutation Analysis**
- Applied **Levenshtein distance** to detect substitutions, insertions, deletions relative to wildtype.
- Generated mutation files for structural biology insights.

### 4. **Modeling**
- **Algorithm**: `XGBoost Regressor`
- Features: Engineered biochemical features.
- Validation: **Repeated K-Fold Cross-Validation** (10 folds, 3 repeats).
- **Metric**: Mean Absolute Error (MAE).

### 5. **Explainable AI (XAI)**
- Applied **SHAP (SHapley Additive exPlanations)** for global and local interpretability.
- Generated:
  - Feature Importance Bar Plot.
  - SHAP Summary Plot (beeswarm).
  - SHAP Dependence Plot for interaction insights.

---

## 📊 Results

### 🔹 Model Performance
| Metric            | Value      |
|-------------------|------------|
| **CV MAE**        | 6.016 ± 0.116 |
| **Train MAE**     | 1.988      |

The model demonstrates strong in-sample performance with reasonable generalization suggested by CV scores.

---

### 🔹 SHAP Analysis — Feature Importance

#### 1. **Global Importance**
![download](https://github.com/user-attachments/assets/96347e02-c4e0-409a-bd1a-7153ca8faba6)

- **Top Contributors**:
  - `AA_S__fraction` (Serine content) is the most influential feature.
  - Followed by `AA_N__fraction` (Asparagine) and `AA_K__fraction` (Lysine).
  - Environmental **pH** also ranks within the top 5 features, highlighting the interplay between biochemical environment and sequence composition.

---

#### 2. **SHAP Summary Plot**
![download](https://github.com/user-attachments/assets/c3adb759-4b97-4dee-b126-1821e0e3e5c4)

- **Interpretation**:
  - Higher fractions of **Serine (S)** and **Asparagine (N)** generally **increase** predicted Tm.
  - Conversely, variations in **Lysine (K)** show both stabilizing and destabilizing impacts depending on concentration.
  - The **pH effect** is non-linear, with extreme pH values negatively impacting stability.

---

#### 3. **SHAP Dependence Plot for pH**
![download](https://github.com/user-attachments/assets/b9a8ee14-34a8-4a5d-8201-b7b0167457b9)

- Indicates strong interaction between **pH** and `AA_E__fraction` (Glutamic Acid).
- At lower pH, Glutamic Acid presence amplifies the destabilizing effect, consistent with biochemical knowledge of acidic residues in acidic environments.

---

## 🔬 Scientific Insights

- **Amino Acid Influence**:
  - Polar uncharged residues like **Serine (S)** and **Asparagine (N)** enhance stability, potentially due to favorable hydrogen bonding.
  - Basic residues (**Lysine (K)**) exhibit complex behavior, possibly linked to ionic interactions sensitive to environmental pH.
  
- **Environmental Impact**:
  - pH emerges as a critical modulator, confirming known enzyme sensitivity to protonation states.
  
- **Mutation Awareness**:
  - Mutation profiling complements predictive modeling by offering structural insights, bridging sequence-level changes with functional outcomes.

---

## 📈 Conclusion
This project successfully demonstrates how machine learning, combined with explainable AI, can predict enzyme stability with interpretability grounded in biochemical reasoning. The SHAP analysis not only validates model behavior but also uncovers biologically meaningful patterns regarding amino acid composition and environmental effects.

---

## 🔮 Future Work
- **Integrate Structural Features**: Use AlphaFold-derived metrics (e.g., solvent accessibility, secondary structure elements) for richer feature sets.
- **Deep Learning Approaches**: Explore sequence-based models (e.g., Transformers for proteins).
- **Enhanced Interpretability**: Apply **LIME** for localized instance explanations and compare with SHAP.
- **Transfer Learning**: Fine-tune pretrained bioinformatics models for stability tasks.
- **Experimental Validation**: Collaborate with wet-lab teams to validate high-impact predictions.
