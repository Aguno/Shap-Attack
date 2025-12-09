# SHAP-based Explanations are Sensitive to Feature Representation

Hyunseung Hwang, Andrew Bell, Joao Fonseca, Venetia Pliatsika,  
Julia Stoyanovich, and Steven Euijong Whang  

FAccT '25 — The 2025 ACM Conference on Fairness, Accountability, and Transparency  
Athens, Greece, June 23–26, 2025

---

This repository contains code and experiments for the paper:

> **SHAP-based Explanations are Sensitive to Feature Representation**  
> Hyunseung Hwang, Andrew Bell, Joao Fonseca, Venetia Pliatsika,  
> Julia Stoyanovich, and Steven Euijong Whang.  
> *FAccT '25, Athens, Greece.* :contentReference[oaicite:0]{index=0}

---

## Overview

Local feature-based explanations (such as SHAP) are widely used to understand and audit machine-learning models, especially in high-stakes domains like lending, healthcare, and public policy. In tabular data, it is common to treat raw feature values as “interpretable.”

This project shows that **SHAP-based explanations are highly sensitive to how features are represented**, and that **simple, seemingly benign data engineering operations can be used to manipulate feature importance** without changing the underlying model. :contentReference[oaicite:1]{index=1}

Concretely, we study:

- **Continuous features (e.g., age)**  
  - Bucketization / binning (equi-width, equi-depth, and custom buckets) can change the SHAP rank of age by **up to ~20 positions** for some individuals.  
  - Even when age is originally the most important feature, its rank often drops by **3–5 positions** after bucketization. :contentReference[oaicite:2]{index=2}  

- **Categorical features (e.g., race)**  
  - Different encodings (one-vs-rest, merging categories into 2 or 3 buckets) substantially change the SHAP importance of race.  
  - Merging race values (e.g., White with Black or Asian) can **drive the importance of race towards zero**, even when it mattered under the original encoding. :contentReference[oaicite:3]{index=3}  

- **Adversarial “data engineering attacks”**  
  - We use **Bayesian optimization** to select bucket boundaries for a protected feature (such as age) that *minimize its SHAP rank* while keeping **high explanation fidelity** (i.e., explanations still reconstruct the model’s predictions).  
  - For race, we show that carefully merging categories can hide the apparent contribution of race while maintaining near-perfect fidelity. :contentReference[oaicite:4]{index=4}  

Our results highlight that **upstream feature engineering decisions must be part of any robust fairness / transparency audit**, not just the model and explainer.

---

## Key Contributions

1. **Systematic sensitivity analysis of SHAP to feature engineering**
   - For continuous features, we study the effect of different bucketization schemes (equi-width, equi-depth, varying numbers of buckets) on SHAP value magnitudes and ranks.
   - For categorical features, we evaluate multiple race encodings (one-vs-rest and grouped encodings with 2 or 3 buckets) and measure their impact on race’s SHAP importance. :contentReference[oaicite:5]{index=5}  

2. **Feature engineering attack on SHAP**
   - We formalize a **two-party audit scenario** (vendor vs. auditor) and define an optimization objective that lowers the SHAP rank of a protected feature while enforcing a fidelity constraint.
   - Using **Bayesian optimization**, we learn custom buckets for age that significantly reduce its SHAP rank compared to standard equi-width bucketization, at fidelities typically above ~90%. :contentReference[oaicite:6]{index=6}  

3. **Implications for practice and governance**
   - We show that standard, defensible data preprocessing steps (like histogramming age or grouping race categories) can be repurposed as a stealthy attack surface on post-hoc explanations.
   - We argue that **governance and auditing frameworks must explicitly account for data engineering choices**, not just models and explanation tools. :contentReference[oaicite:7]{index=7}  

---

## Datasets

The experiments in the paper are based on **U.S. Census ACS-derived benchmark tasks**: :contentReference[oaicite:8]{index=8}  

- **ACS Income (Virginia, 2018)**  
  - Task: predict whether an individual’s income is above \$50K.  
  - 46,144 observations; 8 features (5 categorical).  

- **ACS Public Coverage (Virginia, 2018)**  
  - Task: predict whether an individual is covered by public health insurance.  
  - 25,524 observations; 16 features (13 categorical).  

We treat **age** and **race** as protected features and one-hot encode all categorical features in the baseline setting.

---

