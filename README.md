# 🩺 Predicting Body‑Weight & Obesity Risk with R

![R](https://img.shields.io/badge/R-Programming-blue?logo=r)
![Status](https://img.shields.io/badge/Project-Complete-brightgreen)
![Models](https://img.shields.io/badge/Models-Linear%20Reg%20%7C%20Tree%20%7C%20Random%20Forest-orange)
![Dataset](https://img.shields.io/badge/Data-UCI--Obesity%20%28n=2111%29-lightgrey)

> **Goal** Identify the demographic & behavioural drivers of body‑weight and pinpoint the model that best predicts weight (kg).

---

## 📑 Contents
1. [Background](#background)  
2. [Dataset](#dataset)  
3. [Exploratory Data Analysis](#exploratory-data-analysis)  
4. [Modelling Workflow](#modelling-workflow)  
5. [Results](#results)  
6. [Interpretation](#interpretation)  
7. [Reproduction](#reproduction)  


---

## Background
Obesity is a growing global epidemic.  Using the **UCI “Estimation of Obesity Levels”** repository, this study treats **Weight** as the response variable and benchmarks three algorithms—stepwise **linear regression**, **regression tree (CART)**, and **random forest**—to evaluate predictive power and business realism. :contentReference[oaicite:0]{index=0}

---

## Dataset
* **Rows**  2 111  
* **Predictors** 17 demographic & lifestyle features  
* **Target** Weight (kg)  
* All variables are numeric or factor‑encoded.  See full variable glossary in the report. :contentReference[oaicite:1]{index=1}

---

## Exploratory Data Analysis
* **Demographics** skew younger; max age = 61, min = 14.  
* **Weight distribution** is bimodal (≈ 60–70 kg & 90–100 kg peaks).  
* **Height vs Weight** shows positive correlation with notable heteroskedasticity. :contentReference[oaicite:2]{index=2}  
* **Obesity class (`NObeyesdad`)** strongly stratifies weight ranges. :contentReference[oaicite:3]{index=3}  

Visuals (histograms, scatter, density) are saved in `/fig/` for quick reference.

---

## Modelling Workflow
| Step | Technique | Key Details & Diagnostics |
|------|-----------|---------------------------|
| **1** | Linear Regression + Stepwise | 90 / 10 train‑test; *R² ≈ 0.963*; insignificant vars (e.g., **SMOKE**, **SCC**) dropped. Test MAE ≈ 3.7 kg. :contentReference[oaicite:4]{index=4} |
| **2** | Regression Tree (CART) | Splits driven mainly by **NObeyesdad** and **Height**; post‑prune CP ≈ 0.0015; training MSE ≈ 7.07, test MSE ≈ 20.75. :contentReference[oaicite:5]{index=5} |
| **3** | Random Forest | 100 trees, *mtry = 5*; training MSE ≈ 12.35, test MSE ≈ 59.19; top importance: **NObeyesdad**, **Height**, family history. :contentReference[oaicite:6]{index=6} |

---

## Results
| Model                | Train MSE | Test MSE | Overfit? |
|----------------------|-----------|----------|----------|
| Linear Regression    | **25.95** | **23.85** | Low |
| Regression Tree      | 7.07 | 20.75 | Moderate |
| Random Forest        | 12.35 | 59.19 | High |

*Linear regression* offers the lowest generalisation error despite its simpler form.  Full score table reproduced. :contentReference[oaicite:7]{index=7}

---

## Interpretation
* **`NObeyesdad`** (obesity category) and **Height** consistently rank as dominant predictors, aligning with physiological expectations. :contentReference[oaicite:8]{index=8}  
* Tree structure exposes non‑linear thresholds—for instance, heights ≥ 1.649 m raise expected weight by ~16 kg within *Overweight I–II* strata. :contentReference[oaicite:9]{index=9}  
* Random‑forest overfit indicates excessive model variance; tuning *ntree* and *mtry* or additional cross‑validation could close the gap.

Overall, **stepwise linear regression balances accuracy and interpretability**, making it the preferred choice for practical deployment. :contentReference[oaicite:10]{index=10}

---

## Reproduction

```bash
# clone repo
git clone https://github.com/your‑username/obesity‑weight‑prediction.git
cd obesity‑weight‑prediction

# open R and install dependencies
install.packages(c(
  "tidyverse","rpart","rpart.plot",
  "randomForest","leaps"
))

# run analysis
source("analysis.R")
