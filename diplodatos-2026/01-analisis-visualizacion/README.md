# 01 · Data Analysis and Visualization

*Análisis y Visualización de Datos* · Diplomatura en Ciencia de Datos (FAMAF-UNC), 2026

**Team (Group 5):** Daniela Riego, Gerónimo Castro, Constanza Bencharski, Gerónimo Sequeiros
**Teaching team:** Georgina Flesia, Karim Nemer

## What the course covers

- Frequentist basics: conditional probability, Bayes' rule, independence and conditional independence, law of large numbers
- Maximum likelihood, point and interval estimation, hypothesis testing, significance, reproducible research
- Perception and visual design: charts for quantitative information (contingency tables, bar charts, histograms), categorical vs. continuous data, multivariate visualization, other modalities

## Dataset

[sysarmy 2026 salary survey](https://github.com/DiploDatos/AnalisisyVisualizacion/blob/master/sysarmy_survey_2026_processed.csv) of IT professionals in Argentina (4,939 responses, 60 variables). Loaded directly from the URL in the notebooks.

## Deliverables

| Notebook | Question | Open in Colab |
|----------|----------|---------------|
| [`entregable-1-analisis-descriptivo.ipynb`](entregable-1-analisis-descriptivo.ipynb) | Which programming languages are associated with the best salaries? What tools help explore patterns and associations? | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cbencharski/diplodatos-2026/blob/main/01-analisis-visualizacion/entregable-1-analisis-descriptivo.ipynb) |
| [`entregable-2-inferencia-y-comunicacion.ipynb`](entregable-2-inferencia-y-comunicacion.ipynb) | Is the salary difference between men and women statistically significant? How do we communicate it? | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/cbencharski/diplodatos-2026/blob/main/01-analisis-visualizacion/entregable-2-inferencia-y-comunicacion.ipynb) |

### Deliverable 1 · Descriptive analysis

- **Cleaning:** compared an IQR outlier rule with a domain-based range (above the legal minimum wage, below ARS 20M). IQR kept near-zero salaries because of the long right tail, so the domain-based filter was used. Restricted to full-time workers (95.6% of the data).
- **Language selection:** kept the 8 most frequent languages (SQL, JavaScript, Python, HTML, TypeScript, Bash/Shell, CSS, Java), covering ~80% of mentions.
- **Comparison:** KDE curves and notched boxplots (non-overlapping notches as a rough check that medians differ), plus conditional probabilities P(salary > mean | language).
- **Result:** Java stands out, with ~47% probability of earning above the mean vs. a 38% baseline.
- **Multivariate exploration:** joint and conditional distributions, scatterplots with `hue`, grouping of rare categories (gender, province, education level). Gross vs. net salary correlate at 0.98 once inconsistent records are removed. P(salary > mean | university degree) is 42% vs. 22% for tertiary studies, so education and salary are not independent.

### Deliverable 2 · Inference and communication

Compared net salaries of cis men (n = 3,635) and cis women (n = 924) in the same survey.

| Result | Value |
|--------|-------|
| Difference in means (men − women) | ARS 666,234 |
| 95% confidence interval (Welch) | ARS 547,829 to 784,638 |
| Welch t-test p-value | ≈ 1.7 × 10⁻²⁷ |
| Same test on log-salaries (lognormal fit) | t = 9.78, p ≈ 6 × 10⁻²² |
| Sample size needed for power 0.80 / 0.95 | ~202 / ~333 per group (the sample is 15 to 24× larger) |

- Linked the confidence interval to the hypothesis test: an interval that excludes 0 corresponds to rejecting H₀ at the same level.
- Checked the normality assumption visually and repeated the test on log-transformed salaries.
- **Communication piece** (a popular-science style article): compared the gap overall, within the same role (Developer, QA/Tester, Manager/Director) and within the same education level. Among university graduates the gap is 25.5% (ARS 758,573); among non-graduates it is 17.1%. Women with a university degree earn on average 11.3% more than men without one, yet less than men with one.

> **Limitations:** this is a self-reported, non-random survey, so the results describe the sample and do not establish causes of the gap.

## What I took away

- Choosing a filtering criterion (statistical vs. domain-based) and justifying it.
- Reading a confidence interval, a p-value and statistical power together, instead of in isolation.
- Checking assumptions (normality, independence) and transforming variables when they fail.
- Picking the chart that fits the message, and writing the result for a non-technical audience.
