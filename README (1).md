# Mentorship Synergy: Evaluating Relational Drivers and Sustained Satisfaction in Academic Apprenticeships

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Streamlit](https://img.shields.io/badge/dashboard-Streamlit-red.svg)](https://streamlit.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end applied data science, statistical evaluation, and interactive machine learning platform analyzing quasi-experimental undergraduate research apprenticeship data ($N = 170$). 

This project investigates how deep psychological alignment between faculty mentors and undergraduate mentees drives longitudinal mentorship satisfaction and survey persistence compared against surface-level demographic matching.

---

## 📑 Table of Contents
- [Executive Overview & Key Insights](#-executive-overview--key-insights)
- [Repository Structure](#-repository-structure)
- [Data Dictionary & Preprocessing](#-data-dictionary--preprocessing)
- [Methodology & Analytical Pipeline](#-methodology--analytical-pipeline)
  - [1. Statistical Hypothesis Testing (OLS)](#1-statistical-hypothesis-testing-ols)
  - [2. Machine Learning: Longitudinal Retention Classification](#2-machine-learning-longitudinal-retention-classification)
  - [3. Unsupervised Profiling: Mentorship Archetypes](#3-unsupervised-profiling-mentorship-archetypes)
- [Installation & Environment Setup](#-installation--environment-setup)
- [Running the Streamlit Dashboard](#-running-the-streamlit-dashboard)
- [Compiling the Formal Word Report (.docx)](#-compiling-the-formal-word-report-docx)

---

## 💡 Executive Overview & Key Insights

1. **Deep Similarity Dominates Demographics:** Perceived psychological similarity (`sMSIM_1`) is the strongest statistical predictor of mentee satisfaction ($\beta \approx 0.69, p < 0.001$). Surface demographic alignment (`gendermatch`, `ethnicitymatch`) exhibits no statistically significant direct impact on satisfaction when controlled for psychological alignment.
2. **Apprenticeship Program Dosage:** Students participating in the formal Research Apprenticeship Program (`RAP = 1`) who complete higher research dosages experience sustained psychosocial and instrumental support across longitudinal waves.
3. **Survey Retention Dynamics:** Wave 2 attrition ($38.8\%$ non-response, $n=66$ with sentinel code `-99`) correlates strongly with early relational disengagement in Wave 1. Early intervention using machine learning classification identifies at-risk mentor-mentee dyads with an ROC-AUC of $\sim 0.74$.
4. **Relational Archetypes:** Unsupervised K-Means clustering uncovers three distinct mentoring patterns: *High-Synergy Holistic*, *Task-Centric / Standard*, and *At-Risk / Disconnected*.

---

## 🗂️ Repository Structure

```text
mentorship-synergy-analytics/
├── RAP_Dataset_Public_deident.xlsx    # Primary de-identified research dataset
├── requirements.txt                   # Pinned Python package dependencies
├── README.md                          # Project documentation and reproduction guide
├── generate_report_docx.py            # Automated script to generate the .docx report
├── app.py                             # Interactive Streamlit Web Application
├── src/
│   ├── __init__.py
│   └── data_processing.py             # Ingestion, -99 imputation, scaling, feature engineering
└── notebooks/
    └── mentorship_synergy_analysis.py # Statistical OLS regressions, clustering, and modeling
```

---

## 📊 Data Dictionary & Preprocessing

The primary file `RAP_Dataset_Public_deident.xlsx` contains 170 observations and 32 variables:

| Feature Name | Description | Type / Range |
| :--- | :--- | :--- |
| `RAP` | Research Apprenticeship Program participation | Binary (1 = Apprentice, 0 = Control) |
| `sMSIM_1` | Mentee-mentor perceived psychological similarity | Continuous (1.0 to 7.0 Likert) |
| `sMSAT_1` / `sMSAT_2` | Mentee satisfaction in Wave 1 and Wave 2 | Continuous (1.0 to 7.0 Likert; -99 in W2 = Attrited) |
| `gendermatch` | Surface gender match between mentor and mentee | Binary (1 = Match, 0 = Discordant) |
| `ethnicitymatch` | Surface ethnic match between mentor and mentee | Binary (1 = Match, 0 = Discordant) |
| `MentorPsychSocSupport_w1/w2` | Psychosocial and emotional guidance | Continuous (1.0 to 7.0 Likert) |
| `MentorInstrumentSupport_w1/w2`| Task coaching, skill-building, career aid | Continuous (1.0 to 7.0 Likert) |
| `MentorRoleModelingSupport_w1/w2`| Role modeling and professional emulation | Continuous (1.0 to 7.0 Likert) |
| `retained_w2` | Engineered survey completion target | Binary (1 = Completed W2, 0 = Attrited) |

---

## 🔬 Methodology & Analytical Pipeline

### 1. Statistical Hypothesis Testing (OLS)
We formulate multiple ordinary least squares (OLS) regressions via `statsmodels`:
$$\text{sMSAT\_1} = \beta_0 + \beta_1(\text{sMSIM\_1}) + \beta_2(\text{gendermatch}) + \beta_3(\text{ethnicitymatch}) + \beta_4(\text{Matching\_GPA\_1}) + \epsilon$$
The results confirm that relational alignment accounts for over $48\%$ of the variance in early mentoring satisfaction ($R^2_{\text{adj}} \approx 0.482$).

### 2. Machine Learning: Longitudinal Retention Classification
Using demographic, academic, and Wave 1 relational indicators, regularized Logistic Regression and Random Forest Classifiers are trained to predict the probability of a student completing Wave 2 follow-ups.

### 3. Unsupervised Profiling: Mentorship Archetypes
K-Means ($k=3$) over relational dimensions (`sMSIM_1`, `MentorPsychSocSupport_w1`, `MentorInstrumentSupport_w1`, `MentorRoleModelingSupport_w1`) maps student experiences into actionable administrative archetypes projected on 2D Principal Component axes.

---

## 💻 Installation & Environment Setup

### 1. System Requirements
- Python 3.10 or higher
- PowerShell, Command Prompt, or Bash

### 2. Clone & Setup Virtual Environment (Windows PowerShell)
```powershell
# Navigate to project directory
cd "path\to\mentorship-synergy-analytics"

# Create a Python 3.10 virtual environment
py -3.10 -m venv venv

# Activate virtual environment
.\venv\Scripts\Activate.ps1

# Upgrade package installers
python -m pip install --upgrade pip setuptools wheel

# Install dependencies
pip install -r requirements.txt
```

---

## 🚀 Running the Streamlit Dashboard

Launch the four-tab analytical dashboard:
```powershell
streamlit run app.py
```
Open your browser at `http://localhost:8501` to view:
* **Tab 1: Executive Overview:** High-level metrics, satisfaction distributions, and group comparisons.
* **Tab 2: Statistical Synergy Drivers:** OLS regression parameters and correlation matrix heatmaps.
* **Tab 3: Mentorship Archetypes:** K-Means PCA clustering projections with profile benchmarks.
* **Tab 4: Retention Risk Predictor:** Live scenario modeling for early intervention.

---

## 📄 Compiling the Formal Word Report (.docx)

To generate the complete, publication-style project report in Microsoft Word format:
```powershell
python generate_report_docx.py
```
This generates `Mentorship_Synergy_Project_Report.docx` in the root folder, complete with executive summaries, methodology sections, formal statistical tables, model performance evaluations, and institutional recommendations.