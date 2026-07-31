# Pre-Registration: When Do Factor Premia Break?

**Pre-registration Study Title:** When Do Factor Premia Break? A Statistical Early-Warning Study of Structural Instability in Equity Factor Returns  
**Pre-registration Date:** July 31, 2026  
**Stage at registration:** No factor-return data has been analyzed and no rolling diagnostic values have been computed.

---

## 1. Research Objective

The project aims to investigate whether the distribution of recent factor returns can be an effective early-warning indicator of a forthcoming collapse of the factor’s premium. Our intention is to evaluate a detector as a classification and event-detection system, as opposed to a trading strategy. Its performance will be gauged using several measures, including lead time, precision, recall, and false-positive rate.

The central question guiding this research is:

> Can trailing higher-moment and autocorrelation diagnostics of a factor’s own return series effectively detect periods before the factor’s premium collapses?

A secondary question we will explore is:

> Does a diagnostic model, developed on one factor-collapse scenario, generalize to collapse situations in other factors?

---

## 2. Factors and Data

We will analyze a set of six daily U.S. Equity factors:

1. Market excess return
2. SMB
3. HML
4. Momentum
5. RMW
6. CMA

Our main source of factor-return data will be the Ken French Data Library, with source files meticulously stored in versioned form, including download dates and checksums. For a naive baseline comparison, we will use VIX or a similar measure of market volatility, provided VIX coverage is insufficient for some parts of the sample. Notably, these volatility measures will not be incorporated into our proposed factor-specific diagnostics.

---

## 3. Development and Holdout Design

Our study will feature a single development case:

* **Factor:** Momentum
* **Episode:** 2008–2009 momentum collapse

This is the only factor-collapse instance that we will consider when selecting diagnostic window lengths, threshold levels, and rules for combining diagnostics. The holdout set will comprise:

* HML
* Market excess return
* SMB
* RMW
* CMA
* Moments outside of the 2008–2009 development episode, including any clearly defined 2020 collapse

The determination of formal event dates will not be influenced by colloquial descriptions such as "value drawdown" or "momentum reversal." Collapse dates will exclusively be generated based on the objective criteria outlined below. Once the diagnostic development process begins, no factor or episode will be added to or removed from the holdout set unless necessitated by data limitations. Any such modification will be thoroughly documented.

---

## 4. Collapse Definition

A factor collapse will be identified solely based on the factor’s own historical return data. For each factor, we will calculate a trailing 252-trading-day annualized Sharpe ratio:

$$ SR_t = \sqrt{252} \frac{\bar{r}_{t,252}}{s_{t,252}} $$

Where:
* $\bar{r}_{t,252}$ is the average daily factor return over the 252 trading days concluding at time $t$.
* $s_{t,252}$ is the standard deviation of daily factor returns for the same 252-trading-day period.

The reference distribution will be derived from the preceding 1,260 trading days of the rolling Sharpe ratio series (approximately five years of data). A collapse will be considered to begin when:

$$ SR_t < \mu_{t,1260} - 2\sigma_{t,1260} $$

Here, $\mu_{t,1260}$ and $\sigma_{t,1260}$ represent the trailing mean and standard deviation of the factor’s rolling Sharpe ratio, respectively. 

For a collapse to be formally defined, this condition must persist for a minimum of five consecutive trading days, with the first day of this sequence being designated as the collapse start date. A collapse will be considered to have ended once the rolling Sharpe ratio exceeds the two-standard-deviation boundary and remains above it for at least five consecutive trading days. All computations will employ causal, trailing windows; centered windows or future observations will not be permitted.

---

## 5. Candidate Diagnostics

We will evaluate three candidate diagnostics for their predictive power:

1. Rolling skewness of daily factor returns
2. Rolling excess kurtosis of daily factor returns
3. First-order autocorrelation of squared daily factor returns

Each diagnostic will be calculated using only trailing windows. We will begin by testing windows of 63 trading days (approximately one trading quarter) and later examine sensitivity to window lengths approximately 20% shorter and longer. For the primary formulation, the window lengths will be:

* 50 trading days
* 63 trading days
* 76 trading days

The exact warning thresholds and any rules for combining diagnostics will be determined solely using the momentum 2008–2009 development case. These parameters will then be permanently fixed and recorded in a configuration file for holdout testing.

---

## 6. Warning Definition

A diagnostic warning will be triggered when a fixed diagnostic crosses its pre-defined, fixed warning threshold. The precise method for selecting thresholds will be developed using only the momentum 2008–2009 development case. These thresholds must be presented in standardized or historical percentile form, enabling their application across all factors without factor-specific adjustments.

Only the first warning in a continuous warning sequence will be treated as a distinct warning event.

A new warning event can only be registered if the diagnostic has been in a non-warning state for at least five consecutive trading days prior to the new warning.

---

## 7. Evaluation Horizon and Metrics

A warning will be deemed relevant to a collapse if the collapse initiates within 126 trading days (approximately six months) of the warning date. We will report the following metrics for the development case and each holdout factor:

### Lead Time
The duration (in trading days) between the first valid warning and the onset of the collapse. A positive lead time signifies that the warning preceded the collapse.

### Precision
$$ \text{Precision} = \frac{\text{Number of warnings followed by a collapse within 126 days}}{\text{Total number of warning events}} $$

### Recall
$$ \text{Recall} = \frac{\text{Number of collapse events preceded by a warning within 126 days}}{\text{Total number of collapse events}} $$

### False-Positive Rate
The proportion of warning events that are not followed by a collapse within 126 trading days.

### Detection Rate
The proportion of collapse events that had at least one qualifying advance warning issued by the diagnostic.

All metrics will be calculated and reported, irrespective of whether the results support the study’s hypothesis.

---

## 8. Naive Baseline

The performance of our factor diagnostics will be benchmarked against a warning rule based on the VIX. This VIX baseline will utilize a trailing historical threshold established prior to holdout testing, evaluated against the same 126-trading-day event horizon and assessed using the same lead-time, precision, recall, detection-rate, and false-positive metrics. We will explicitly test and report whether the proposed diagnostics offer information beyond a general increase in market volatility.

---

## 9. Sensitivity Analysis

Following the primary holdout evaluation, we will assess the robustness of our findings by examining approximate 20% variations in:

* Diagnostic window length
* Warning threshold
* Collapse threshold
* Warning-to-collapse evaluation horizon

Sensitivity results will be presented independently of the primary results, which will be derived from a frozen methodology. Our approach will not be revised based on the results of the sensitivity analyses.

---

## 10. Multiple Testing

The total count of all diagnostic, factor, and parameter combinations evaluated will be recorded. The primary approach to correct for multiple testing will be the Benjamini-Hochberg false discovery rate procedure with a target false discovery rate of 5%. We will also report:

* Unadjusted $p$-values
* Benjamini-Hochberg adjusted $p$-values
* Bonferroni-adjusted significance thresholds
* The total number of statistical tests performed

No result will be declared statistically significant unless it meets this pre-defined primary correction standard.

---

## 11. Causal-Only Computation Rule

All statistical calculations at time $t$ must depend solely on data available at or before time $t$. The implementation must avoid the use of:

* Centered rolling windows
* Negative shifts that backdate future observations
* Full-sample standardization methods
* Thresholds derived from holdout outcomes
* Post-holdout parameter revisions based on results

The final code will include a leakage test in synthetic data that modifies the observations after date $t$ and checks that all diagnostic outputs by date $t$ stay unchanged.

---

## 12. Reporting Commitments

All reports will include:

* All diagnostics run
* All parameters frozen
* All holdout results
* Under-predicted events of collapses
* False positives
* Raw and adjusted statistics
* Sensitivity results
* Outlier exclusions
* Implementation changes
* Deviations from this pre-registration document

Collapsing diagnoses that do not replicate will not be excluded from the reported results. All deviations from this pre-registration document will be dated, will include an explanation, and will be noted as having occurred during an exploratory part of the analysis.

---

## 13. Expected Limitations

The analysis is expected to suffer from a lack of statistical power as defined by the scarcity of well-defined, collapsing factors. The definition of factor collapse may also be sensitive to:

1. Window chosen to calculate Sharpe ratios
2. The reference period used in the definition of each characteristic
3. Threshold values for collapses

The selection of factors may also present a bias because standard factors with long return histories were chosen. Positive results will therefore be treated as a rationale for further inquiry rather than definitive evidence that there exists a reliable early-warning system for crises.

---

## 14. Freeze Statement

This document details the planned analyses prior to analysis of factor-return data and establishes the pre-specification as of July 31, 2026. The case development, holdout factors, collapse definition rule, candidates, evaluation period, primary measures of interest, causality-only constraint, and multiple-test strategy are fixed on this date. Subsequent changes must be recorded as amendments without overwriting this original specification.
