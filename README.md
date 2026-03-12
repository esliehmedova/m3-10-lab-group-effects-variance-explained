![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Group Effects and Variance Explained

## Overview

When you want to know whether a categorical variable (like treatment group, region, or product tier) has a meaningful impact on a numeric outcome, ANOVA is the classical tool for the job. But running a single F-test is only the beginning — you also need to verify that the test's assumptions hold, figure out *which* groups actually differ, and quantify *how much* of the outcome's variability the grouping explains.

In this lab you will walk through the full ANOVA workflow: from assumption checks through post-hoc comparisons to effect-size reporting. You will work with a real dataset that has at least three groups, diagnose when the standard one-way ANOVA is trustworthy, and learn what to do when it is not. Along the way, you will compute eta-squared and Cohen's d to move beyond "statistically significant" toward "practically meaningful."

The lab closes by connecting the ANOVA framework to regression. You will see that the R² from a simple linear model with a dummy-coded grouping variable is algebraically the same as eta-squared, reinforcing the idea that ANOVA and regression are two views of the same underlying model.

## Learning Goals

By the end of this lab, you should be able to:

- Conduct a one-way ANOVA and interpret the F-statistic and p-value
- Check ANOVA assumptions using Levene's test and normality diagnostics
- Perform Tukey HSD post-hoc tests to identify which group pairs differ
- Compute eta-squared (η²) and Cohen's d and explain their practical meaning
- Visualize group distributions to support statistical conclusions
- Demonstrate that eta-squared equals R² from a dummy-coded regression

## Setup and Context

You will work inside a single Jupyter notebook (`m3-10-group-effects-variance-explained.ipynb`). All code, plots, and written interpretations belong in this notebook. Use markdown cells generously — the goal is not just to produce numbers but to explain what they mean.

For the dataset, you need a continuous outcome variable and a categorical grouping variable with **at least three levels**. Good options include:

- **Penguins** (`seaborn.load_dataset("penguins")`) — compare `body_mass_g` across species.
- **Tips** (`seaborn.load_dataset("tips")`) — compare `total_bill` across day of the week.
- **PlantGrowth** (from R datasets, available via CSV) — compare yield across treatment groups.

Your instructor may specify a dataset. If not, Penguins is recommended because it offers clean groups, slight assumption violations worth diagnosing, and large enough effect sizes to produce meaningful results.

## Requirements

### Tools and Libraries

- Python 3.9+
- pandas, numpy
- scipy (`scipy.stats.f_oneway`, `scipy.stats.levene`, `scipy.stats.shapiro`)
- statsmodels (`statsmodels.stats.multicomp.pairwise_tukeyhsd`)
- scikit-learn (`sklearn.linear_model.LinearRegression`, `sklearn.metrics.r2_score`)
- matplotlib, seaborn
- Jupyter Notebook or JupyterLab

### Installation

If any packages are missing, install them in your environment:

```bash
pip install pandas numpy scipy statsmodels scikit-learn matplotlib seaborn
```

### Repository Setup

1. Fork this repository to your GitHub account.
2. Clone your fork to your local machine:

```bash
git clone https://github.com/<your-username>/m3-10-lab-group-effects-variance-explained.git
cd m3-10-lab-group-effects-variance-explained
```

3. Open the notebook `m3-10-group-effects-variance-explained.ipynb` and work through the tasks below.

## Getting Started

1. Import all required libraries at the top of your notebook.
2. Load your chosen dataset and inspect it (`.info()`, `.describe()`, value counts for the grouping variable).
3. Drop or impute missing values, documenting your decision.
4. Identify the **outcome variable** (continuous) and the **grouping variable** (categorical, ≥ 3 levels).

## Tasks

### Task 1: One-Way ANOVA

1. Split the outcome variable into separate arrays (or use a groupby), one per level of the grouping variable.
2. Run a one-way ANOVA using `scipy.stats.f_oneway`.
3. Record the F-statistic and p-value.
4. In a markdown cell, interpret the result:
   - State the null and alternative hypotheses in plain language.
   - Based on the p-value (use α = 0.05), do you reject or fail to reject the null?
   - What does the F-statistic tell you about the ratio of between-group to within-group variance?

### Task 2: Check ANOVA Assumptions

ANOVA results are only reliable when two key assumptions hold. Test both here.

**Homogeneity of variance (Levene's test):**

1. Run `scipy.stats.levene` on the same groups.
2. Report the test statistic and p-value.
3. Interpret: is the equal-variance assumption reasonable?

**Normality of residuals:**

1. For each group, run the Shapiro-Wilk test (`scipy.stats.shapiro`).
2. Alternatively, compute the ANOVA residuals (observed − group mean) and run Shapiro-Wilk on the pooled residuals.
3. Create a Q-Q plot of the residuals (use `scipy.stats.probplot` or `statsmodels.graphics.gofplots.qqplot`).
4. In a markdown cell, summarize:
   - Do the residuals look approximately normal?
   - If either assumption is violated, what alternatives could you use (e.g., Welch's ANOVA, Kruskal-Wallis)?

### Task 3: Tukey HSD Post-Hoc Comparisons

The overall ANOVA F-test tells you that *at least one* group differs, but not *which* groups differ. Post-hoc tests fill that gap.

1. Run Tukey's HSD test using `statsmodels.stats.multicomp.pairwise_tukeyhsd`.
2. Print the results table showing each pairwise comparison, the mean difference, the confidence interval, and the reject/fail-to-reject decision.
3. Create a **mean plot with confidence intervals** (Tukey's HSD provides these) to visualize which groups overlap.
4. In a markdown cell, answer:
   - Which specific group pairs have significantly different means?
   - Which pairs are *not* significantly different?
   - Do the confidence intervals help you understand the practical size of the differences?

## Submission

### What to Submit

- Your completed Jupyter notebook `m3-10-group-effects-variance-explained.ipynb` containing all code, visualizations, and markdown explanations.

### Definition of Done

- [ ] One-way ANOVA is performed with F-statistic and p-value reported
- [ ] Levene's test and Shapiro-Wilk / Q-Q plot check ANOVA assumptions
- [ ] Tukey HSD post-hoc results identify which group pairs differ
- [ ] All markdown cells contain clear, concise interpretations

### How to Submit

1. Save your notebook and make sure all cells have been run (Kernel → Restart & Run All).
2. Stage and commit your changes:

```bash
git add .
git commit -m "Completed group effects and variance explained lab"
```

3. Push to your fork:

```bash
git push origin main
```

4. Create a Pull Request from your fork to the original repository.
5. Submit the Pull Request link on the student portal.
