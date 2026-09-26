# Trade Openness & Economic Growth in India (2000–2024)

A statistical study testing whether India's growing integration with the global economy — measured by trade openness — has driven its GDP growth in the post-liberalization era.

## Overview

| | |
|---|---|
| **Research question** | Does trade openness have a statistically significant impact on India's GDP growth? |
| **Period** | 2000–2024 (25 years, post-1991 liberalization era) |
| **Data source** | [World Bank World Development Indicators](https://data.worldbank.org/country/india) |
| **Method** | Simple & multiple linear regression, correlation analysis, in R |
| **Authors** | Lakshita Jayal, Radhika Gupta |

## Definitions

- **Trade Openness** — (Exports + Imports) / GDP, a standard measure of an economy's integration with global trade (World Bank series `NE.TRD.GNFS.ZS`)
- **Economic Growth** — Annual percentage growth of real GDP (World Bank series `NY.GDP.MKTP.KD.ZG`)

## Hypotheses

- **H₀:** Trade openness has no significant impact on GDP growth
- **H₁:** Trade openness has a positive and significant impact on GDP growth

## Data

25 annual observations (2000–2024) pulled from the World Bank API. Full dataset: [`data/india_trade_gdp_2000_2024.csv`](data/india_trade_gdp_2000_2024.csv).

| Statistic | GDP Growth (%) | Trade Openness (% of GDP) |
|---|---|---|
| Min | -5.78 | 25.99 |
| 1st Quartile | 5.24 | 39.91 |
| Median | 7.21 | 45.42 |
| Mean | 6.20 | 43.17 |
| 3rd Quartile | 7.92 | 48.92 |
| Max | 9.69 | 55.79 |

## Exploratory Analysis

**Distributions:** GDP growth is left-skewed with a sharp negative outlier in 2020 (COVID-19 contraction). Trade openness is more evenly spread, rising through the 2000s, peaking around 2011–2013, and settling in the 40–50% range in recent years.

![Histograms](outputs/histograms.png)

**Spread & outliers:** the boxplots confirm the 2020 shock as GDP growth's sole outlier, and trade openness' low outlier from the early 2000s before liberalization effects fully took hold.

![Boxplots](outputs/boxplots.png)

## Relationship Between Trade Openness and GDP Growth

![Scatter plot of Trade Openness vs GDP Growth](outputs/scatter_trade_vs_gdp.png)

| Metric | Value | Interpretation |
|---|---|---|
| Correlation (r) | 0.216 | Weak positive relationship |
| Regression coefficient (β) | +0.078 | Positive direction |
| p-value | 0.300 | Not significant (> 0.05) |
| R² | 4.7% | Very low explanatory power |

## Regression Models

**Simple regression:** `GDP_Growth ~ Trade_Openness`

```
Coefficients:
                            Estimate  Std. Error  t value  Pr(>|t|)
(Intercept)                 2.8381     3.2272      0.879    0.388
Trade_Openness_Percent_GDP  0.0779     0.0734      1.061    0.300

Multiple R-squared: 0.0467   Adjusted R-squared: 0.0052
F-statistic: 1.126 on 1 and 23 DF, p-value: 0.2997
```

**Multiple regression:** `GDP_Growth ~ Trade_Openness + Year` (controlling for a time trend)

```
Coefficients:
                            Estimate   Std. Error  t value  Pr(>|t|)
(Intercept)                128.3748   190.6007     0.674    0.508
Trade_Openness_Percent_GDP   0.1030     0.0836     1.233    0.231
Year                         -0.0629     0.0955    -0.659    0.517

Multiple R-squared: 0.0651   Adjusted R-squared: -0.0199
F-statistic: 0.7659 on 2 and 22 DF, p-value: 0.4769
```

Full console output: [`outputs/regression_output.txt`](outputs/regression_output.txt).

## Findings

- Trade openness and GDP growth show only a **weak, positive correlation (r = 0.22)**.
- Neither the simple nor the multiple regression coefficient on trade openness is **statistically significant** (p > 0.05 in both models).
- The models explain very little of the variance in GDP growth (R² of 4.7–6.5%), meaning other factors dominate.
- The time trend (Year) adds no explanatory power once trade openness is in the model.
- **2020 (COVID-19)** is a clear structural outlier that pulls the relationship around; even so, the underlying link stays weak across the full 25-year window.

## Conclusion

**We fail to reject H₀.** Trade openness shows a positive but statistically insignificant association with India's GDP growth over 2000–2024. This does not mean trade is unimportant to the Indian economy — it means that, on its own, the trade-to-GDP ratio is too blunt a measure to explain year-to-year growth swings. Growth is more plausibly driven by factors such as domestic investment, inflation, government policy, and productivity, which this single-variable framework does not capture. A stronger next step would be a multivariate model incorporating these controls, or examining trade's *composition* (e.g., high-value exports) rather than its aggregate share of GDP.

## Repository Structure

```
.
├── data/
│   └── india_trade_gdp_2000_2024.csv     # GDP growth & trade openness, World Bank
├── scripts/
│   └── trade_openness_analysis.R          # Full analysis: regressions, correlation, charts
├── outputs/
│   ├── scatter_trade_vs_gdp.png
│   ├── histograms.png
│   ├── boxplots.png
│   └── regression_output.txt
└── README.md
```

## Reproducing the Analysis

```bash
git clone https://github.com/<your-username>/trade-openness-india.git
cd trade-openness-india
Rscript scripts/trade_openness_analysis.R
```

Requires base R (no extra packages needed). Charts and console output are written to `outputs/`.

## Tech Stack

R (base `stats`, `graphics`) · World Bank Open Data API

## Authors

- **Lakshita Jayal**
- **Radhika Gupta**
