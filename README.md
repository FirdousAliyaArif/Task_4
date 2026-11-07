# Clinical Explainable AI (XAI): Interpreting Breast Cancer Diagnostics via SHAP and LIME

A rigorous machine learning and Explainable AI (XAI) framework designed to bridge the gap between black-box predictive models and clinical adoption in oncology. This repository contains code, analysis, and comprehensive documentation evaluating **SHAP (SHapley Additive exPlanations)** and **LIME (Local Interpretable Model-agnostic Explanations)** on a Random Forest classifier trained to diagnose breast cancer.

---

## 📋 Table of Contents

1. [Project Overview](https://www.google.com/search?q=%23project-overview)
2. [Dataset Architecture](https://www.google.com/search?q=%23dataset-architecture)
3. [Model Performance & Architecture](https://www.google.com/search?q=%23model-performance--architecture)
4. [Explainable AI (XAI) Methodologies](https://www.google.com/search?q=%23explainable-ai-xai-methodologies)
5. [Repository Structure](https://www.google.com/search?q=%23repository-structure)
6. [Installation & Setup](https://www.google.com/search?q=%23installation--setup)
7. [Key Visual Artifacts](https://www.google.com/search?q=%23key-visual-artifacts)
8. [Comparative Synthesis: SHAP vs. LIME](https://www.google.com/search?q=%23comparative-synthesis-shap-vs-lime)

---

## 🔍 Project Overview

While high-accuracy ensemble models excel at clinical predictions, their internal mechanisms are often opaque. In healthcare, high empirical accuracy alone is insufficient; clinicians require transparent, verifiable reasoning to trust automated diagnostic tools.

This project implements an end-to-end XAI workflow to:

* Train a non-linear **Random Forest Classifier** on cytopathological data.


* Provide **global feature importance** using SHAP to discover primary morphological drivers of malignancy.


* Provide **local instance attribution** (Sample Index 0 case study) using both SHAP waterfalls and LIME surrogate approximations.


* Support regulatory compliance (e.g., EU GDPR "right to explanation") and build physician trust.



---

## 📊 Dataset Architecture

The project utilizes the **Breast Cancer Wisconsin (Diagnostic)** dataset sourced directly from `scikit-learn` (`sklearn.datasets.load_breast_cancer`).

* **Total Instances:** 569 patient samples


* **Class Distribution:** 212 Malignant (~37.3%) / 357 Benign (~62.7%)


* **Dimensionality:** 30 continuous numerical features computed from fine-needle aspirate (FNA) digital images (covering radius, texture, perimeter, area, smoothness, compactness, concavity, concave points, symmetry, and fractal dimension across Mean, Standard Error, and "Worst" metrics).


* **Data Split:** 80% training set ($N_{train} = 455$) and 20% testing set ($N_{test} = 114$) using a fixed random seed (`random_state=42`).



---

## 🌲 Model Performance & Architecture

* **Algorithm:** Random Forest Classifier ($B = 100$ estimators).


* **Test Accuracy:** **96.49%**


* **Clinical Significance:** While high accuracy validates generalizability, feature attribution ensures that predictions rely on valid clinical pathology (e.g., cell boundary irregularity and nuclear size) rather than shortcut artifacts.



---

## 🔬 Explainable AI (XAI) Methodologies

### 1. SHAP (SHapley Additive exPlanations)

* **Theoretical Foundation:** Cooperative game theory (Shapley Values).


* **Optimization:** Uses **TreeSHAP** to compute polynomial-time conditional expectations across decision tree paths ($O(TLD^2)$ complexity).


* **Guarantees:** Satisfies strict mathematical axioms including *Efficiency*, *Symmetry*, *Dummy Player*, and *Additivity*.



### 2. LIME (Local Interpretable Model-agnostic Explanations)

* **Theoretical Foundation:** Local surrogate modeling using sparse linear regressors (e.g., Lasso) around an instance neighborhood defined by an exponential distance kernel ($\pi_x(z)$).


* **Output Format:** Discretized rule-based inequalities (e.g., `worst concave points <= 0.06`) indicating local feature contributions.



---

## 📁 Repository Structure

```text
├── data/                      # Dataset references and loaders
├── notebooks/                 # Jupyter Notebook containing model training & XAI code
├── outputs/                   # Saved visual artifacts (.png plots)
├── docs/                      # Comprehensive technical report (Task_4_2.docx)
└── README.md                  # Project documentation

```

---

## ⚙️ Installation & Setup

To run the analysis locally, clone the repository and install the necessary dependencies:

```bash
git clone https://github.com/your-username/breast-cancer-xai.git
cd breast-cancer-xai

# Install required Python packages
pip install numpy pandas scikit-learn shap lime matplotlib seaborn

```

---

## 📈 Key Visual Artifacts

The pipeline automatically generates and saves high-resolution publication-ready visual assets:

1. **SHAP Beeswarm Summary Plot (`shap_summary.png`):** Displays global feature importance and directional impact across all 114 test samples, revealing that *worst concave points*, *worst perimeter*, and *worst radius* are primary indicators of malignancy.


2. **SHAP Local Waterfall Plot (`shap_waterfall.png`):** Breaks down exact additive log-odds contributions for individual patient evaluations (Sample Index 0).


3. **LIME Local Explanation Plot (`lime_local.png`):** Visualizes local rule-based feature intervals and surrogate regression weights for the same test instance.



---

## ⚖️ Comparative Synthesis: SHAP vs. LIME

| Evaluation Dimension | SHAP (SHapley Additive exPlanations) | LIME (Local Interpretable Model-agnostic Explanations) |
| --- | --- | --- |
| **Theoretical Foundation** | Cooperative Game Theory (Shapley Values)

 | Local Surrogate Models (Regularized Linear Regressors)

 |
| **Axiomatic Guarantees** | Guarantees Efficiency, Symmetry, Dummy Player, and Additivity

 | No theoretical guarantees; relies on heuristic local sampling

 |
| **Scope of Explanation** | Global (Summary) and Local (Waterfall)

 | Strictly Local (explains individual predictions)

 |
| **Computational Complexity** | Optimized to $O(TLD^2)$ for trees via TreeSHAP

 | Scales with perturbation sample size $K$

 |
| **Output Consistency** | Deterministic (exact game-theoretic attribution)

 | Stochastic (perturbed samples can cause minor run-to-run variance)

 |
