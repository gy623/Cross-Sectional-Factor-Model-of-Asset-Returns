# Cross-Sectional-Factor-Model-of-Asset-Returns

A beginner-to-intermediate project implementing a cross-sectional
factor model with PCA-based statistical factor extraction,
built in Python using a Jupyter notebook.

---

## 🧠 What This Project Does

This project builds a cross-sectional factor model that explains
variation in asset returns across stocks at each point in time.

The pipeline covers:
- Constructing a clean asset return panel
- Building and normalising fundamental factor signals
- Running period-by-period WLS cross-sectional regressions
- Applying PCA to extract statistical factors and clean
  the covariance matrix
- Validating the model using IC, ICIR, and residual diagnostics

The core model equation for asset i at time t is:

  r_{i,t} = Σ_k  β_{i,k} · f_{k,t}  +  ε_{i,t}

---

## 📁 Repo Structure

cross-sectional-factor-model/
│
├── factor_model.ipynb     ← Main notebook — run this
├── data/
│   ├── raw/               ← Raw price/fundamental data
│   └── processed/         ← Cleaned return panel & signals
├── outputs/
│   ├── figures/           ← Saved plots
│   └── results/           ← Factor returns, loadings (CSV)
├── requirements.txt       ← Python dependencies
├── .gitignore             ← Git exclusions
└── README.md              ← You are here

---

## ⚙️ Setup Instructions

### 1. Clone the repo
Open your terminal and run:

  git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
  cd YOUR_REPO_NAME

### 2. Create a virtual environment (recommended)

  python -m venv venv
  source venv/bin/activate        # Mac/Linux
  venv\Scripts\activate           # Windows

### 3. Install dependencies

  pip install -r requirements.txt

### 4. Launch Jupyter

  jupyter notebook

Then open factor_model.ipynb and run cells top to bottom.

---

## 📦 Data

Price data is pulled automatically via `yfinance` inside the
notebook — no manual downloads required.

If you want to use your own data, place CSV files in data/raw/
and update the loading cell in Section [1] of the notebook.

---

## 📐 Methodology Notes

### Fundamental Factors (Stage 3a)
Period-by-period WLS regression of returns on pre-built
exposure signals (value, momentum, size, quality).
Weights are proportional to sqrt(market cap).

### PCA Statistical Factors (Stage 3b)
PCA is applied to the standardised return panel.
The number of factors K is selected using:
  - Scree plot visual inspection
  - Marchenko-Pastur upper bound (Random Matrix Theory)

The resulting loadings B and factor scores f_t are used
to construct the cleaned covariance matrix:

  Σ_r = B · Σ_f · Bᵀ + Δ

### Validation
- Information Coefficient (IC): Spearman rank correlation
  between predicted and realised returns
- ICIR = mean(IC) / std(IC)  — analogous to a Sharpe ratio
  for signal quality
- Residual PCA: checks for omitted factor structure

---

## 🗺️ Roadmap / Extensions

- [ ] Add Fama-MacBeth standard errors
- [ ] Incorporate sector neutralisation
- [ ] Add DCC-GARCH dynamic covariance
- [ ] Build a simple long/short backtest
- [ ] Migrate reusable functions to a /src module

---

## 📚 References

- Barra USE4 Factor Model Documentation
- Fama & MacBeth (1973) — Risk, Return and Equilibrium
- Ledoit & Wolf (2004) — Honey, I Shrunk the Sample Covariance Matrix
- Marchenko & Pastur (1967) — Random Matrix Theory

---

## 🙋 Author

Godwin Yuen
github.com/gy623