# Study notes · Data Analysis and Visualization

Quick reference to review the course. Where a concept was applied in the notebooks, it is linked to the deliverable.

## Probability

| Concept | Formula / idea | Applied in |
|---------|----------------|-----------|
| Conditional probability | P(A \| B) = P(A ∩ B) / P(B) | Deliverable 1: P(salary > mean \| language) |
| Bayes' rule | P(A \| B) = P(B \| A) P(A) / P(B) | |
| Independence | P(A ∩ B) = P(A) P(B). If P(A \| B) ≠ P(A), they are not independent | Deliverable 1: education vs. salary |
| Law of large numbers | The sample mean converges to the expected value | |

## Estimation and tests

| Concept | Idea | Applied in |
|---------|------|-----------|
| Point estimate | A single value for the parameter (e.g., difference of means) | Deliverable 2, ex. 1 |
| Confidence interval | Range of plausible values at level 1 − α. If it excludes 0, reject H₀: μ_A − μ_B = 0 | Deliverable 2, ex. 1 |
| Welch t-test | Two independent samples, unequal variances; degrees of freedom by Welch-Satterthwaite | Deliverable 2, ex. 2 |
| p-value | Probability of a result at least this extreme if H₀ is true | Deliverable 2, ex. 2 |
| Power | Probability of detecting a real effect (1 − type II error). Depends on effect size, α and n | Deliverable 2, ex. 2.3 |
| Central limit theorem | Sample means are approximately normal for large n | Deliverable 2, ex. 1 |
| Log transform | Lognormal data → log is normal, so t-tests apply | Deliverable 2, ex. 2 |

## Visualization

- Histogram and KDE: shape of one distribution. Boxplot (notched): medians and spread across groups.
- Scatterplot with transparency and `hue`: two numeric variables plus a category; transparency reveals overplotting.
- Bar charts for categorical counts, with an explicit baseline.
- Group rare categories before plotting, and say how they were grouped.
- Match the chart to the message and to the audience.

## To review

- [ ] Bayes' rule with a worked example
- [ ] Maximum likelihood estimator for a normal and a binomial
- [ ] Interpretation of p-value vs. confidence interval (common mistakes)
