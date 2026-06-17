# Cross-Sectional Factor Model of Asset Returns

A beginner-to-intermediate project implementing cross-sectional and statistical factor models for asset returns, combining economic intuition (fundamental factors) with PCA-based risk modelling.

Built in Python using Jupyter notebooks.

---

## 🧠 What This Project Does

This project builds and compares multiple factor modelling frameworks to explain and forecast cross-sectional variation in asset returns.

The pipeline includes:

* Construction of a clean asset return panel
* Implementation of a dynamic cross-sectional factor model
* PCA-based statistical covariance (risk) modelling
* Risk-adjusted portfolio construction
* Backtesting and benchmark evaluation

Core modelling structure:

$$
r_{i,t} = \sum_k \beta_{i,k,t} f_{k,t} + \epsilon_{i,t}
$$

---

## 📁 Repo Structure

```text
cross-sectional-factor-model/
│
├── factor_model_dynamic.ipynb
├── factor_model_pca_risk.ipynb
├── factor_model_old.ipynb
├── portfolio_evaluation.ipynb
├── data/
├── outputs/
└── README.md
```

All notebooks share a unified `data/` and `outputs/` structure for consistent preprocessing and evaluation.

---

# 🧠 Methodology

## 1. Dynamic Cross-Sectional Factor Model

This module builds **5 interpretable economic factors**:

* Market beta
* Momentum
* Size (log market cap)
* Low volatility
* Long-term reversal

### Pipeline

* Cross-sectional winsorisation and standardisation (z-score at each time $t$)
* Sequential orthogonalisation (residualisation)
* Rolling Information Coefficient (IC)

### Signal construction

* IC is used to generate time-varying factor weights
* Factor forecasts are dynamically combined using IC-weighting

---

## 2. PCA Risk Engine

This module builds a statistical covariance estimator + portfolio optimiser.

### 2.1 PCA covariance estimation

* Rolling window: **36 periods**
* PCA decomposition of returns
* Number of factors $k$ chosen such that:

$$
\sum_{i=1}^{k} \text{explained variance}_i \geq 0.80
$$

---

### Covariance reconstruction

$$
\Sigma_{\text{PCA}} = B^\top F B
$$

where:

* $B$: eigenvector (loading) matrix
* $F$: diagonal matrix of eigenvalues

---

### 2.2 Idiosyncratic risk

$$
D = \Sigma_{\text{sample}} - \Sigma_{\text{PCA}}
$$

Diagonal noise approximation is used for stability.

---

### 2.3 Risk-adjusted portfolio construction

Let $s_t$ be factor model scores.

#### Step 1: normalise signals

* Cross-sectional normalisation:

  * $\sum s_t = 1$

#### Step 2: risk adjustment

$$
w_t = D^{-1} s_t
$$

#### Step 3: portfolio normalisation

* Enforce:

  * $\sum |w_t| = 1$
  * $\sum w_t = 0$

---

### 2.4 Mean-variance optimisation (planned)

To be implemented using CVXPY:

* maximise return / risk trade-off
* incorporate constraints from PCA risk model

---

## 3. Legacy Model (`factor_model_old`)

Original exploratory implementation combining:

* WLS cross-sectional regression
* PCA factor extraction attempt
* Early signal testing framework

Kept for comparison and reference.

---

## 4. Portfolio Evaluation

This notebook evaluates all strategies:

* Portfolio PnL construction
* Benchmark comparison
* Risk and return metrics
* IC / ICIR analysis
* Drawdown and stability diagnostics

---

## 📐 Key Design Insights

* Dynamic factor model improves interpretability via economically motivated signals
* PCA risk model improves covariance stability in high-dimensional settings
* Separation of:

  * alpha model (signal generation)
  * risk model (PCA covariance)
    improves robustness and avoids conflicting assumptions

---

## 🗺️ Roadmap

* [ ] Mean-variance optimisation (CVXPY implementation)
* [ ] Sector neutrality constraints
* [ ] Transaction cost modelling
* [ ] DCC-GARCH dynamic covariance extension
* [ ] Modular `/src` refactor for productionisation
* [ ] Live portfolio simulation engine

---

## 📚 References

* Fama & MacBeth (1973) — Cross-sectional asset pricing
* Barra Factor Models — Equity risk decomposition
* Ledoit & Wolf (2004) — Covariance shrinkage
* Marchenko & Pastur (1967) — Random matrix theory
* Jolliffe (2002) — Principal Component Analysis

---

## 🙋 Author

Godwin Yuen
GitHub: github.com/gy623
