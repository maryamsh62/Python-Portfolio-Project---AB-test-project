# A/B Test Analysis

## Introduction
In this project, we will be working to understand the results of an A/B test run by an [e-commerce website](https://en.wikipedia.org/wiki/E-commerce). The company has developed a new web page to increase the number of users who “convert,” meaning users who decide to pay for the product.

## 1. Project Purpose
This project analyzes an A/B test to help the company decide whether to **ship the new landing page, keep the existing page**, or **collect more data**. The core business question is:

**Does the new landing page lead to a higher user conversion rate than the old page?** 

Analyze_ab_test_results_notebook

## 2. Background
The company ran an experiment where users were randomly shown either:
- **Old (control) page**
- **New (treatment) page**

Each user’s behavior was tracked with a binary outcome: **converted (1)** or **not converted (0)**. Our task was to statistically test whether the new page truly improves conversions or whether the observed differences could be due to chance. 

## 3. Analysis Steps

### 3.1 Exploratory Checks
- Verified that users were reasonably split between the two pages.
- Checked basic conversion rates for each group.
- Confirmed that the metric of interest was converted (proportion of users who converted). 

### 3.2 Hypothesis Testing (Part II)

We framed the experiment as a **one-sided test** because the company cares about improvement, not just difference.

- **Null hypothesis (H₀)**: The new landing page has the **same or lower** conversion rate than the old page.
- **Alternative hypothesis (H₁)**: The new landing page has a **higher** conversion rate than the old page.

We tested this in **two ways**:

1. **Simulation from the null** (bootstrapping-style approach)
2. **Analytical z-test** for difference in proportions

**Result:**
- p-value ≈ **0.906** → far above 0.05
- Therefore, we **failed to reject H₀** → we do **not** have evidence that the new page is better.
- The z-score led to the same conclusion. 

### 3.3 Logistic Regression (Part III)

To confirm the A/B test results and to check for possible **confounders**, we fit a logistic regression model:

- **Outcome**: converted
- **Predictors**: landing_page (old vs new), plus **country** (US, UK, CA, etc.)
- We also added **interaction terms** such as UK_new_page, CA_new_page to guard against **Simpson’s paradox** (i.e., the effect looking different when data is grouped by country). 


**What we found:**

- Only the **intercept** was statistically significant (p < 0.05).
- The variables for the new page, country, and their interactions all had p-values > 0.05.
- That means: **neither the landing page, nor the user’s country, nor their combination had a significant effect on the probability of conversion**.
- This agrees with the A/B test. 


## 4. Conclusion & Recommendation

- All three approaches — **simulation, z-test**, and **logistic regression** — tell a consistent story: **the new page does not outperform the old page**.
- Because we cannot show a statistically significant lift, the safest business action is to **keep the old landing page**.
- If the business still believes the new design has qualitative advantages, the correct next step is to **run a longer / higher-powered experiment** (i.e., collect more traffic) before adopting it. 









