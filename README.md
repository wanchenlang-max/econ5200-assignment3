# econ5200-assignment3
# Econ Assignment 3 — Causal Inference in Logistics (SwiftCart Case Study)

## Author

Student Name: wanchen lang
Course:  ECON 5200
Assignment: Assignment 3 — Causal Analysis

---

# Project Overview

This project investigates causal inference challenges in a simulated logistics platform, SwiftCart Logistics. Traditional parametric methods often fail when data violate normality, homoscedasticity, and random assignment assumptions. Therefore, this project applies **non-parametric and causal inference methods**, including:

* Bootstrap confidence intervals
* Permutation testing
* Propensity Score Matching (PSM)
* Love Plot visualization

All analysis was performed in Python using Google Colab.

---

# Tools and Libraries

Python libraries used:

```
numpy
pandas
matplotlib
seaborn
sklearn
```

Environment:

Google Colab

---

# Phase 1 — Bootstrap Confidence Interval for Median Tips

## Objective

Driver tips exhibit:

* Zero inflation (many $0 tips)
* Heavy right skew

This violates normality assumptions.

We generated:

* 100 zero values
* 150 exponential values (scale = 5)
* total n = 250

We performed:

10,000 bootstrap resamples

## Result

Observed Median: computed from sample

95% Bootstrap Confidence Interval:

2.5th percentile to 97.5th percentile

## Interpretation

The bootstrap distribution was asymmetric due to skewness and zero inflation. This demonstrates why parametric confidence intervals assuming normality are unreliable in real-world logistics earnings data.

Bootstrap provides a robust, assumption-free estimate.

---

# Phase 2 — Permutation Test for Delivery Time A/B Test

## Objective

Evaluate a new routing algorithm.

Control group:

Normal distribution
mean = 35
sd = 5

Treatment group:

Log-normal distribution
(meanlog = 3.4, sigma = 0.4)

This introduces extreme outliers.

Traditional t-tests are invalid due to heteroscedasticity and skew.

We performed:

5,000 permutation simulations.

## Result

Observed mean difference computed.

Permutation p-value calculated empirically.

## Interpretation

Permutation testing does not rely on distributional assumptions. It constructs the null distribution directly from the observed data, providing a valid inference even with heavy skew and outliers.

---

# Phase 3 — Selection Bias and Propensity Score Matching

Dataset:

swiftcart_loyalty.csv

Variables:

subscriber (treatment indicator)

pre_spend

account_age

support_tickets

post_spend (outcome)

---

## Naive Estimate (Biased)

Naive SDO:

17.57

Interpretation:

Subscribers appear to spend 17.57 more units on average.

However, this estimate is biased because high-spending users are more likely to subscribe.

---

## Propensity Score Matching

We estimated propensity scores using logistic regression based on:

pre_spend
account_age
support_tickets

Then performed 1:1 nearest neighbor matching.

Result:

ATT = 9.91

---

## Interpretation

Matching reduced the estimated effect by:

44%

This indicates that a large portion of the observed difference was due to selection bias rather than a causal effect.

SwiftPass increases spending, but less than naive comparisons suggest.

---

# Phase 4 — Love Plot and Covariate Balance

Standardized Mean Differences (SMD):

Before Matching:

pre_spend = 0.674
account_age = 0.324
support_tickets = −0.166

After Matching:

pre_spend = 0.014
account_age = −0.016
support_tickets = 0.017

Threshold:

|SMD| < 0.1 indicates good balance.

Result:

All covariates fell below 0.02 after matching.

Conclusion:

Matching successfully eliminated observable selection bias.

---

# Key Conclusions

Bootstrap methods provide reliable uncertainty estimates under skewed distributions.

Permutation tests allow valid hypothesis testing without parametric assumptions.

Naive comparisons severely overestimated SwiftPass impact.

Propensity Score Matching reduced bias and produced a more credible causal estimate.

Love Plot confirmed successful covariate balance.

---

# Repository Contents

```
Assignment 3/
    Econ_3916_Assignment_3.ipynb
README.md
swiftcart_loyalty.csv
```

---

# Reproducibility

To reproduce:

Open notebook in Google Colab

Run all cells sequentially

Ensure swiftcart_loyalty.csv is uploaded

---

# Academic Integrity Statement

This project was completed independently using course materials and statistical programming tools. All causal inference methods were implemented transparently and reproducibly.

---
