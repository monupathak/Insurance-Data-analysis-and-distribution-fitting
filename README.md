# Insurance Claim Severity Analysis and Distribution Fitting

> This project was developed for **IE 802: OR Applications in Infrastructure and Services Sector**. It focuses on statistical modeling, tail-risk assessment, and data-driven underwriting and policy design suggestions.

## Overview

This project analyzes insurance claim amounts and evaluates probability distributions for modeling claim severity. The analysis focuses on the strongly right-skewed nature of insurance losses and the need to capture extreme claims when assessing risk.

The objective of this investigation is to understand how claim severity behaves in a real insurance dataset, identify the most appropriate statistical distribution for the observed losses, and quantify the tail risk that drives a disproportionate share of total claim value.

## Investigation Objective

The investigation (referenced in the `assignment` branch) explores the following questions:

* How are insurance claim amounts distributed across the portfolio?
* Is the claim distribution approximately symmetric, or is it strongly right-skewed?
* Which probability distributions provide the best fit for the overall claim severity data?
* Do standard single-distribution models fail to capture the extreme tail of large losses?
* Can a two-component body-tail model provide a better approximation of both ordinary and catastrophic claims?
* How do risk measures such as VaR and TVaR change when the upper tail is modeled explicitly?

In other words, this repository is a practical investigation into insurance claim severity modeling: from exploratory analysis and distribution fitting to tail-risk quantification and actuarial interpretation.

Accurate claim-severity modeling supports:

* Risk assessment and premium development
* Loss-reserve and capital analysis
* Quantification of extreme-loss exposure
* Evaluation of deductibles, policy limits, and reinsurance thresholds
* Simulation and stress testing of future claim portfolios

> **Note:** This repository models claim **severity** (the size of a claim). A complete actuarial pricing model would also model claim **frequency** and exposure.

## Dataset

* **Number of claims:** 4,954
* **Mean claim amount:** $43,578.88
* **Observed range:** approximately $1,947 to more than $245,000
* **Distribution shape:** strongly right-skewed with a heavy upper tail

Most observations are concentrated at lower claim amounts, while a relatively small number of large claims contribute a substantial share of the aggregate loss.

## Methodology

### 1. Exploratory Data Analysis

The initial analysis examines:

* Summary statistics and quantiles
* Histograms and empirical cumulative distribution functions
* Box plots and log-scale visualizations
* Skewness and the concentration of losses in the upper tail

The raw claim amounts are strongly right-skewed, so the mean alone does not adequately describe the underlying risk.

### 2. Log Transformation

A natural logarithm transformation is applied to the positive claim amounts:

```python
log_claim = np.log(claim_amount)
```

This compresses large observations, makes the body of the data more symmetric, and improves visualization and diagnostics. A normal fit to log-claims corresponds to a lognormal model for claim amounts.

The transformation does not, by itself, establish lognormality; distributional fit must still be evaluated using diagnostic plots and goodness-of-fit measures.

### 3. Candidate Distribution Fitting

The following candidate distributions are fitted to the claim data where appropriate:

* Normal
* Lognormal
* Exponential
* Gamma
* Weibull
* Pareto
* Generalized Pareto distribution (GPD)

Model performance is assessed using:

* **Kolmogorov-Smirnov statistic:** Measures the maximum distance between the empirical and fitted cumulative distribution functions
* **Akaike Information Criterion (AIC):** Compares fit while penalizing the number of estimated parameters
* **Bayesian Information Criterion (BIC):** Applies a stronger complexity penalty than AIC as sample size increases
* **Graphical diagnostics:** Histograms, fitted density curves, empirical CDFs, and Q-Q or P-P plots

Lower AIC and BIC values indicate a better fit among models evaluated on the same observations and scale. Because parameters are estimated from the data, formal KS p-values should be interpreted cautiously.

### 4. Two-Segment Body-Tail Model

No single candidate distribution adequately captures both the central mass and the extreme upper tail. The data is therefore modeled using two components:

| Segment | Approximate share | Model |
| ------- | ----------------: | ------------------------ |
| Body | Lower 80% of claims | Lognormal |
| Tail | Upper 20% of claims | Pareto-type distribution |

The split is based on a selected percentile threshold. The body model represents ordinary claim severity, while the tail model captures the slower decay of extreme losses.

This segmented specification produces a substantially better fit than the tested single-distribution alternatives, with an observed AIC improvement of approximately 35% in the current analysis.

For rigorous use, the threshold should be validated through sensitivity analysis, mean-excess plots, parameter-stability plots, and out-of-sample performance. The body and tail components should align smoothly without creating artificial jumps in the estimated density or distribution function.

### 5. Risk Metrics

The fitted models are used to estimate:

* **Value at Risk (VaR):** The claim amount not expected to be exceeded at a selected confidence level
* **Tail Value at Risk (TVaR):** The expected claim amount conditional on exceeding VaR
* **Tail concentration:** The share of total claim value contributed by the largest claims
* **Tail index:** A measure of upper-tail heaviness and the likelihood of extreme observations

The analysis evaluates VaR and TVaR at high confidence levels, including 95% and 99%, and finds that the largest 10% of claims account for more than half of the observed aggregate claim value.

## Key Findings

1. **Claim amounts are strongly right-skewed.** Most claims are comparatively moderate, while a small number of large claims drive a substantial portion of total losses.

2. **A lognormal model describes the body reasonably well.** The log-transformed body is more symmetric, although lognormality must be verified rather than assumed.

3. **The upper tail decays more slowly than the body model predicts.** A Pareto-type tail model better represents the observed extreme claims.

4. **Losses are highly concentrated.** Approximately 10% of claims contribute more than 50% of the total observed claim value.

5. **A segmented model outperforms a single distribution.** Modeling the body and tail separately improves the representation of both routine and extreme claims.

6. **The mean is insufficient for risk decisions.** Quantiles, VaR, TVaR, and exceedance probabilities provide additional information that is essential for assessing tail exposure.

## Business Applications

### Pricing

The fitted severity distribution can support premium development when combined with claim frequency, exposure, expenses, and a risk margin. Tail-sensitive metrics can help prevent systematic underpricing in the presence of rare but highly damaging claims.

### Reserving and Capital

VaR, TVaR, and simulated aggregate-loss distributions can inform reserve adequacy, economic capital, and solvency analysis. The exact confidence level and risk measure depend on the applicable regulatory framework and risk appetite.

### Underwriting

When policy and customer-level covariates are available, the analysis can be extended to identify characteristics associated with higher expected severity or tail risk. Claim amounts alone are not sufficient for underwriting decisions, but they reveal the underlying risk profile.

### Reinsurance

Tail models can support the evaluation of attachment points, limits, expected ceded losses, and the cost-benefit trade-off of excess-of-loss reinsurance.

### Product Design

The fitted distribution can be used to estimate the financial effects of deductibles, policy limits, co-insurance, and exclusions.

## Repository Structure

```text
.
├── detailed_claim_severity_modeling_and_interpretation.ipynb  # In-depth severity modeling, tail analysis, and actuarial interpretation
├── Insurance_claim_data.ipynb                                 # Exploratory analysis, distribution fitting, and diagnostics
└── README.md                                                  # Comprehensive project documentation
```

> **Note:** The original course assignment prompt (`Assignment_3 (3).pdf`) is preserved in the [`assignment`](https://github.com/monupathak/Insurance-Data-analysis-and-distribution-fitting/tree/assignment) branch.

## Getting Started

### Requirements

* Python 3.9 or later
* Jupyter Notebook or JupyterLab

### Main Libraries

* `pandas`
* `numpy`
* `scipy`
* `matplotlib`
* `seaborn`

### Installation

```bash
git clone <repository-url>
cd <repository-name>
pip install pandas numpy scipy matplotlib seaborn jupyter
jupyter notebook Insurance_claim_data.ipynb
```

Replace `<repository-url>` and `<repository-name>` with the actual repository details.

## Reproducing the Analysis

1. Open `Insurance_claim_data.ipynb`.
2. Load and inspect the claim dataset.
3. Run the exploratory analysis and visual diagnostics.
4. Fit the candidate severity distributions.
5. Compare KS statistics, AIC, BIC, and diagnostic plots.
6. Fit and evaluate the two-segment body-tail model.
7. Calculate VaR, TVaR, tail concentration, and tail-index estimates.

## Limitations

* The results are based on one dataset containing 4,954 observed claims.
* The current analysis models severity but not claim frequency or exposure.
* A percentile threshold is a modeling choice and should be tested for stability.
* In-sample fit does not guarantee out-of-sample predictive accuracy.
* AIC and BIC should only be compared when models use the same observations and likelihood scale.
* Inflation, policy limits, deductibles, truncation, and censoring may affect the observed distribution.
* Regulatory capital conclusions are jurisdiction-specific and require additional assumptions.

## Future Work

### Model Validation

* Perform train-test or rolling-window validation
* Backtest VaR exceedance rates and assess TVaR stability
* Use bootstrap procedures to quantify parameter and risk-metric uncertainty
* Compare threshold choices using EVT diagnostics

### Covariate-Based Severity Modeling

* Fit GLMs or GAMs using policy type, geography, demographics, and claim characteristics
* Compare classical actuarial models with gradient-boosting and random-forest models
* Develop conditional severity estimates for customer or policy segments

### Extreme Value Theory

* Apply peaks-over-threshold modeling using the GPD
* Use mean-excess and parameter-stability plots for threshold selection
* Estimate return levels and rare-event probabilities with confidence intervals

### Frequency-Severity and Portfolio Simulation

* Model claim counts using Poisson or negative-binomial distributions
* Combine frequency and severity models to simulate aggregate losses
* Stress-test inflation, catastrophe, and loss-development scenarios
* Evaluate reinsurance structures through Monte Carlo simulation

### Monitoring and Deployment

* Refit models as new claims become available
* Detect changes in claim severity and tail behavior
* Integrate validated models into pricing, reserving, and risk-monitoring workflows

## Disclaimer

This project is intended for educational and analytical purposes. The results should not be used directly for pricing, reserving, underwriting, or regulatory decisions without independent actuarial review and domain-specific validation.

## Author

**Monu Pathak**
Actuarial Risk Modeling and Insurance Analytics
Last updated: 2026
