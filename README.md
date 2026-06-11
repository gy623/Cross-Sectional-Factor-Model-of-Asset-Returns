# Cross-Sectional Factor Model of Asset Returns

A beginner-to-intermediate project implementing a cross-sectional factor model with PCA-based statistical factor extraction, built in Python using Jupyter notebooks.

---

## 🧠 What This Project Does

This project builds a cross-sectional factor model that explains variation in asset returns across stocks at each point in time.

The pipeline covers:

* Constructing a clean asset return panel
* Building and normalising fundamental factor signals
* Running period-by-period WLS cross-sectional regressions
* Applying PCA to extract statistical factors and clean the covariance matrix
* Validating the model using IC, ICIR, and residual diagnostics

The core model is:

$r_{i,t} = \sum_k \beta_{i,k} \cdot f_{k,t} + \epsilon_{i,t}$

---

## 📁 Repo Structure

```text

cross-sectional-factor-model/
│
├── factor_model_1.ipynb          ← Original exploration notebook (WLS + PCA combined attempt)
├── factor_model_WLS.ipynb        ← Dedicated WLS cross-sectional regression implementation
├── factor_model_PCA.ipynb        ← Dedicated PCA-based statistical factor model
│
├── data/
│   ├── raw/                      ← Raw price and fundamental data
│   └── processed/               ← Cleaned return panel and factor signals
│
├── outputs/
│   ├── figures/                 ← Saved plots
│   └── results/                ← Factor returns, loadings, diagnostics (CSV)
│
├── requirements.txt             ← Python dependencies
├── .gitignore                   ← Git exclusions
└── README.md                    ← Project documentation
```

All notebooks share the same `data/` and `outputs/` directories to ensure consistent preprocessing, feature engineering, and evaluation across methods.

---

## 📦 Data

Price data is pulled automatically via `yfinance` inside the notebooks, so no manual downloads are required.

If you want to use your own dataset, place CSV files in `data/raw/` and update the relevant loading cell in each notebook.

---

## 📐 Methodology Notes

### 1. Cross-Sectional WLS Model

The WLS implementation estimates factor exposures via period-by-period weighted least squares regression across assets.

Weights are proportional to $\sqrt{\text{market cap}}$, reducing the influence of micro-cap noise.

A key design decision in this project is that the WLS pipeline operates on **raw (non-log, non-cross-sectionally normalised) returns**, since empirical testing showed that additional cross-sectional normalisation reduced stability and weakened factor interpretability in this specification.

---

### 2. PCA Statistical Factor Model

The PCA pipeline operates on a **standardised cross-sectional return panel**, where returns are normalised at each time step before decomposition.

PCA is applied to extract orthogonal statistical factors from the covariance structure of returns.

The number of factors $K$ is selected using:

* Scree plot inspection
* Marchenko–Pastur upper bound from random matrix theory

The resulting decomposition is:

$\Sigma_r = B \Sigma_f B^\top + \Delta$

where:

* $B$ is the loading matrix
* $\Sigma_f$ is the factor covariance matrix
* $\Delta$ is idiosyncratic noise

---

### 3. Model Validation

Performance is evaluated using:

* Information Coefficient (IC): Spearman rank correlation between predicted and realised returns
* ICIR: $\text{ICIR} = \frac{\mathbb{E}[\text{IC}]}{\text{Std}(\text{IC})}$, analogous to a Sharpe ratio for signal quality
* Residual PCA: checks for remaining latent structure not captured by the model

---

## 🧪 Key Design Insight

A central observation in this project is that:

* WLS factor estimation is more stable on raw return space
* PCA factor extraction benefits from cross-sectional normalisation

As a result, the two pipelines are intentionally separated rather than forced into a single unified preprocessing framework.

This separation improves interpretability and avoids conflicting statistical assumptions.

---

## 🗺️ Roadmap / Extensions

* [ ] Add Fama–MacBeth standard errors
* [ ] Incorporate sector neutralisation
* [ ] Add DCC-GARCH dynamic covariance modelling
* [ ] Build a long/short backtest layer
* [ ] Refactor reusable components into a `/src` module

---

## 📚 References

* Barra USE4 Factor Model Documentation
* Fama & MacBeth (1973) — Risk, Return, and Equilibrium
* Ledoit & Wolf (2004) — Shrinkage Estimators for Covariance Matrices
* Marchenko & Pastur (1967) — Random Matrix Theory

---

## 🙋 Author

Godwin Yuen
github.com/gy623
