# Cross-Sectional Factor Model of Asset Returns

A beginner-to-intermediate project implementing cross-sectional factor models using both fundamental factor regressions and PCA-based statistical factor extraction.

Built in Python using Jupyter notebooks.

---

## 🧠 Project Overview

This repository explores different approaches to modelling the cross-section of stock returns.

The project originally began as a single implementation combining:

* Fundamental factor modelling via Weighted Least Squares (WLS)
* Statistical factor extraction via Principal Component Analysis (PCA)

During development, it became apparent that the two methodologies require different return preprocessing assumptions and therefore do not reconcile naturally within a single framework.

In particular:

* The **WLS factor model** is implemented using raw returns.
* The **PCA factor model** uses cross-sectionally normalised (standardised) returns prior to factor extraction.

To reflect this distinction, the repository was split into three separate implementations.

---

## 📁 Repository Structure

```text
cross-sectional-factor-model/
│
├── factor_model_1/
│   ├── factor_model_1.ipynb
│   ├── data/
│   └── outputs/
│
├── factor_model_WLS/
│   ├── factor_model_WLS.ipynb
│   ├── data/
│   └── outputs/
│
├── factor_model_PCA/
│   ├── factor_model_PCA.ipynb
│   ├── data/
│   └── outputs/
│
└── README.md
```

---

## 📂 Project Descriptions

### factor_model_1

The original exploratory implementation.

This notebook contains both the WLS and PCA approaches and documents the development process that led to their separation.

A key finding was that the two methods require different return preprocessing assumptions:

* WLS performs best using raw returns.
* PCA requires cross-sectional standardisation of returns to extract meaningful latent factors.

As a result, the approaches were separated into dedicated notebooks.

---

### factor_model_WLS

A Barra-style cross-sectional factor model estimated using weighted least squares.

Features include:

* Value factor
* Momentum factor
* Size factor
* Quality factor
* Period-by-period cross-sectional regressions
* Market-cap-based weighting
* Factor return estimation
* Residual diagnostics

Returns are **not cross-sectionally normalised** before regression.

The model takes the form:

[
r_{i,t} = \sum_k \beta_{i,k} f_{k,t} + \epsilon_{i,t}
]

where:

* (r_{i,t}) is the return of asset (i)
* (\beta_{i,k}) is the exposure of asset (i) to factor (k)
* (f_{k,t}) is the factor return
* (\epsilon_{i,t}) is the idiosyncratic residual

---

### factor_model_PCA

A statistical factor model based on Principal Component Analysis.

Features include:

* Cross-sectional return standardisation
* PCA factor extraction
* Scree plot analysis
* Marchenko–Pastur factor selection
* Covariance matrix cleaning
* Residual structure diagnostics

The cleaned covariance matrix is constructed as:

[
\Sigma_r = B\Sigma_fB^\top + \Delta
]

where:

* (B) is the factor loading matrix
* (\Sigma_f) is the factor covariance matrix
* (\Delta) is the diagonal specific-risk matrix

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv venv

# Mac/Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Open the notebook corresponding to the implementation you wish to run.

---

## 📦 Data

Price data is retrieved automatically using `yfinance`.

No manual downloads are required.

Each model directory contains its own:

* `data/` folder for raw and processed datasets
* `outputs/` folder for figures, diagnostics, and exported results

To use custom data, place CSV files in the appropriate `data/` directory and modify the loading section of the notebook accordingly.

---

## 📊 Validation

The notebooks include a range of validation techniques, including:

* Information Coefficient (IC)
* Information Coefficient Information Ratio (ICIR)
* Factor return analysis
* Residual diagnostics
* Residual PCA
* Covariance reconstruction checks

---

## 🗺️ Future Extensions

* [ ] Add Fama–MacBeth standard errors
* [ ] Incorporate sector neutralisation
* [ ] Implement covariance shrinkage methods
* [ ] Add DCC-GARCH dynamic covariance modelling
* [ ] Build a long/short factor backtest
* [ ] Refactor reusable functions into a `src/` package

---

## 📚 References

* Barra USE4 Factor Model Documentation
* Fama & MacBeth (1973) — *Risk, Return and Equilibrium*
* Ledoit & Wolf (2004) — *Honey, I Shrunk the Sample Covariance Matrix*
* Marchenko & Pastur (1967) — *Random Matrix Theory*

---

## 🙋 Author

**Godwin Yuen**

GitHub: https://github.com/gy623
