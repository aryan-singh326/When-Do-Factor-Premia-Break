# When Do Factor Premia Break?

### A Pre-Registered Early-Warning Study of Structural Instability in Equity Factor Returns

This repository contains the complete code, data-processing pipeline, research paper, and reproducible analysis for my undergraduate research project:

> **When Do Factor Premia Break? A Pre-Registered Early-Warning Study of Structural Instability in Equity Factor Returns**

The project investigates whether changes in the higher moments of a factor's own return distribution provide advance warning of future deterioration in that factor's premium.

Unlike most factor-timing research, this project is **not a trading strategy or backtest**. Instead, it treats collapse prediction as a **statistical event-detection problem**, evaluated using precision, recall, lead time, and statistical validation.

---

# Research Question

Can a purely causal statistical detector identify periods when an equity factor is entering a fragile regime before a formally defined collapse occurs?

More specifically:

* Can rolling higher-moment statistics detect instability before a collapse?
* Does a detector developed on one historical event generalize to other factors?
* Does the detector outperform simple market-stress measures such as the VIX?

---

# Key Features

* Pre-registered experimental design
* Strict separation of development and holdout data
* Causal (look-ahead free) computations
* Objective, rule-based collapse definition
* Multiple candidate diagnostics evaluated transparently
* Frozen detector evaluated only once on holdout data
* Bootstrap confidence intervals
* Permutation-based statistical validation
* Comparison against a VIX baseline
* Complete reproducibility through Jupyter notebooks

---

# Repository Structure

```text
.
├── notebooks/
│   ├── 01_data_acquisition.ipynb
│   ├── 02_collapse_definition.ipynb
│   ├── 03_detector_development.ipynb
│   ├── 04_holdout_evaluation.ipynb
│   ├── 05_statistical_validation.ipynb
│   ├── 06_vix_baseline_and_robustness.ipynb
│   └── 07_figures_and_tables.ipynb
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── results/
│
├── figures/
├── tables/
├── paper/
├── pre_registration/
└── README.md
```

---

# Methodology

## 1. Data

Daily U.S. equity factor returns:

* Market (MKT–RF)
* SMB
* HML
* RMW
* CMA
* Momentum (MOM)

Auxiliary data:

* CBOE VIX

---

## 2. Objective Collapse Definition

Factor collapses are identified mechanically rather than manually.

A collapse begins when the trailing 252-trading-day annualized Sharpe ratio falls more than two historical standard deviations below its own trailing five-year distribution for five consecutive trading days.

This produces:

* **87 total collapse episodes**
* **1 development event**
* **86 holdout events**

---

## 3. Detector Development

Only **one** historical event was used for model development:

* Momentum
* October 2009 collapse

Three candidate diagnostics were evaluated:

* Rolling skewness
* Rolling excess kurtosis
* Rolling squared-return autocorrelation

Only excess kurtosis generated an advance warning under multiple neighboring thresholds.

The frozen detector uses:

* 63-day rolling excess kurtosis
* Historical z-score using the prior 252 observations
* Warning threshold = 2.0
* Five-day warning reset
* 126-trading-day evaluation horizon

After freezing these parameters, **no further tuning was performed**.

---

# Holdout Results

Across the 86 holdout collapse events:

| Metric            |              Result |
| ----------------- | ------------------: |
| Holdout events    |                  86 |
| Detected events   |                  42 |
| Recall            |           **48.8%** |
| Warning precision |            **7.8%** |
| Median lead time  | **66 trading days** |

Bootstrap confidence intervals:

* Recall: **38.4–59.3%**
* Precision: **5.6–10.1%**
* Median lead time: **46–91 trading days**

---

# Statistical Validation

The detector was evaluated against a circular-shift null model that preserves warning frequency and temporal clustering.

Results:

* Observed recall exceeded the null mean only modestly.
* One-sided permutation p-value: **0.164**
* No individual factor remained significant after multiple-testing correction.

The statistical evidence therefore supports excess kurtosis as a **plausible but noisy fragility indicator**, rather than a validated standalone collapse classifier.

---

# VIX Baseline

The frozen detector was also compared with a simple causal VIX z-score baseline over the common post-1990 sample.

| Detector        | Recall | Precision |
| --------------- | -----: | --------: |
| Excess kurtosis |  56.4% |     10.4% |
| VIX             |  63.6% |      6.5% |

The VIX achieved higher recall but generated substantially more warnings.

The two detectors also identified different subsets of collapse events, suggesting they capture partially distinct information.

---

# Reproducibility

Every stage of the analysis is reproducible from the notebooks.

Pipeline:

1. Download and clean data
2. Define collapse events
3. Develop detector
4. Freeze parameters
5. Evaluate holdout events
6. Statistical validation
7. Generate figures and tables
8. Compile the paper

No notebook modifies earlier results after the detector has been frozen.

---

# Research Contributions

This project contributes:

* an objective definition of factor-premium collapse,
* a causal-only early-warning pipeline,
* a pre-specified evaluation protocol,
* explicit controls against look-ahead bias,
* holdout validation,
* bootstrap and permutation inference,
* comparison with a naive VIX benchmark.

---

# Technologies

* Python
* Jupyter Notebook
* pandas
* NumPy
* Matplotlib
* SciPy
* LaTeX
* Git
* Google Colab

---

# Citation

If you reference this repository, please cite the accompanying paper:

> Singh, A. (2026). *When Do Factor Premia Break? A Pre-Registered Early-Warning Study of Structural Instability in Equity Factor Returns.*

---

# License

This repository is released under the MIT License.

---

# Disclaimer

This project is an academic research study.

It is **not** an investment strategy, trading system, or financial recommendation. The detector is evaluated solely as a statistical event classifier. Moderate recall, low precision, and statistically inconclusive performance mean it should not be interpreted as a validated forecasting model for investment decisions.

