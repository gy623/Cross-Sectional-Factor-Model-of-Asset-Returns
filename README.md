# Dynamic Cross-Sectional Factor Model with Mean-Variance Optimisation

A quantitative equity investment framework combining interpretable cross-sectional factor modelling, PCA-based statistical risk estimation, and constrained mean-variance portfolio optimisation.

Built in Python using Jupyter notebooks.

---

## Overview

This project develops a complete systematic equity investment pipeline by separating the investment process into three modular components:

- **Alpha model:** dynamic cross-sectional factor signals
- **Risk model:** PCA-based covariance estimation
- **Portfolio construction:** risk-adjusted allocation and mean-variance optimisation

The framework combines economically motivated factors with statistical risk modelling to generate market-neutral portfolios while maintaining robustness in high-dimensional asset universes.

The underlying factor model is

\[
r_{i,t}=\sum_{k=1}^{K}\beta_{i,k,t}f_{k,t}+\epsilon_{i,t}
\]

where asset returns are decomposed into systematic factor exposures and idiosyncratic risk.

---

## Features

- Dynamic cross-sectional factor model
- Five interpretable equity factors
- Rolling Information Coefficient (IC) weighting
- Sequential factor orthogonalisation
- PCA covariance estimation
- Residual (idiosyncratic) risk modelling
- Long-short portfolio construction
- Risk-adjusted inverse covariance allocation
- Mean-variance optimisation using CVXPY
- Backtesting and performance evaluation

---

## Repository Structure

```text
cross-sectional-factor-model/
│
├── Dynamic_Factor_Model_with_Mean_Variance_Optimisation.pdf
├── Dynamic Cross-Sectional Factor Model.ipynb
├── PCA Risk Engine.ipynb
├── Portfolio Evaluation.ipynb
├── factor_model_old.ipynb
├── data/
├── outputs/
├── references/
└── README.md
```

---

# Methodology

## 1. Dynamic Cross-Sectional Factor Model

Five economically motivated factors are constructed for each asset:

- Market beta
- Momentum
- Size
- Low volatility
- Long-term reversal

### Signal Processing

Each factor undergoes

- Winsorisation (1st–99th percentile)
- Cross-sectional standardisation
- Sequential orthogonalisation

to reduce outlier influence, remove scale differences, and minimise multicollinearity.

### Dynamic Signal Weighting

Rather than assigning fixed factor weights, predictive strength is measured using rolling Information Coefficients (ICs).

Rolling ICs are averaged over a 24-period window to produce time-varying factor weights:

- Factors performing well receive larger weights
- Weak factors are automatically downweighted

This allows the alpha model to adapt to changing market regimes.

---

## 2. PCA Statistical Risk Model

A statistical covariance model is constructed using Principal Component Analysis.

### Covariance Estimation

Using a rolling 36-period window,

- compute the sample covariance matrix
- perform PCA decomposition
- retain the minimum number of principal components explaining at least 80% of total variance

The covariance estimator is reconstructed as

\[
\Sigma_{PCA}=B_k\Lambda_kB_k^T
\]

where

- \(B_k\) contains the retained eigenvectors
- \(\Lambda_k\) contains the corresponding eigenvalues

---

### Residual Risk

Idiosyncratic covariance is estimated via

\[
D=\Sigma_{sample}-\Sigma_{PCA}
\]

Only the diagonal of \(D\) is retained to improve estimation stability and reduce sampling noise.

The final covariance estimate becomes

\[
\hat{\Sigma}
=
\Sigma_{PCA}
+
\operatorname{diag}(D)
\]

---

## 3. Portfolio Construction

### Long–Short Factor Portfolio

Assets are ranked according to composite factor scores.

Each rebalance:

- Long top 20%
- Short bottom 20%

with

- dollar neutrality
- unit gross exposure

providing a simple benchmark strategy.

---

### Risk-Adjusted Portfolio

Factor scores are first normalised before inverse covariance scaling:

\[
w=\hat{\Sigma}^{-1}\tilde{f}
\]

Weights are then

- demeaned
- leverage normalised
- market neutral

to obtain a risk-adjusted portfolio.

---

### Mean-Variance Optimisation

The final portfolio solves the constrained optimisation problem

\[
\max_w
w^T\tilde{f}
-
\frac{\lambda}{2}
w^T\hat{\Sigma}w
\]

implemented using **CVXPY**.

The optimisation balances expected alpha against portfolio risk while satisfying leverage and market-neutrality constraints.

---

## Performance Evaluation

Strategies are evaluated through

- cumulative portfolio returns
- benchmark comparison
- Sharpe ratio
- maximum drawdown
- rolling Information Coefficients
- ICIR (Information Coefficient Information Ratio)
- rolling Information Ratio

---

## Design Philosophy

The framework deliberately separates

- **Alpha generation**
- **Risk estimation**
- **Portfolio optimisation**

allowing each component to be improved independently while avoiding conflicting modelling assumptions.

---

## Current Limitations

The current implementation assumes

- frictionless trading
- static investment universe
- no transaction costs
- fixed model hyperparameters

These simplifications make the framework suitable for research while leaving several opportunities for future development.

---

## References

- Fama, E. F., & French, K. R. (1992). *The Cross-Section of Expected Stock Returns*. **The Journal of Finance**, 47(2), 427–465.
- Jegadeesh, N. (1990). *Evidence of Predictable Behavior of Security Returns*. **The Journal of Finance**, 45(3), 881–898.
- Baytas, A., & Cakici, N. (1999). *Do Markets Overreact? International Evidence*. **Journal of Banking & Finance**, 23(7), 1121–1144.
- Hendershott, T., & Menkveld, A. J. (2014). *Price Pressures*. **Journal of Financial Economics**, 114(3), 405–423.
- Connor, G., & Korajczyk, R. A. (1988). *Risk and Return in an Equilibrium APT: Application of a New Test Methodology*. **Journal of Financial Economics**, 21(2), 255–289.
- Frazzini, A., & Pedersen, L. H. (2014). *Betting Against Beta*. **Journal of Financial Economics**, 111(1), 1–25.
- MSCI Barra. *Barra Risk Model Handbook*. Multiple editions.
---

## Author

**Godwin Yuen**

GitHub: https://github.com/gy623