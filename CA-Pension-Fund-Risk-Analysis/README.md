### Pension Fund Risk Forecasting Using Machine Learning

**Author:** Kanwarjit Singh Dhariwal

#### Executive summary

This project investigates whether California's public pension funds are becoming more financially risky over time, and whether that risk can be forecasted using historical financial and actuarial data. Using seven government datasets spanning 22 years and roughly 130 California pension systems, I found that the statewide average fund fell below full funding after the 2008 financial crisis and has not fully recovered. Forecasting a fund's exact future funded ratio turned out to be a genuinely hard problem — every regression model I tried, including regularized and ensemble methods, failed to beat a simple "assume no change" baseline. However, classifying a fund's risk level (Low/Medium/High) worked well, reaching roughly 90% accuracy. Using the City of Fresno's two pension funds as a detailed case study, I found both are healthier than the statewide average, but one — the Employees' Retirement System — is on a trend that could take it below full funding around 2027, a pattern confirmed as genuinely rare when checked against every other reliable fund in the state.

#### Rationale

Public pension underfunding is not an abstract accounting issue. When a fund's assets fall short of what it owes, the shortfall is typically closed through higher taxpayer-funded contributions, reduced government services, or, in the worst cases, reduced benefits for retirees who planned their lives around promised income. Because pension funding gaps develop slowly and are described in dense actuarial language, they are easy for policymakers, city officials, and the public to overlook until the problem becomes expensive and urgent. An early-warning approach — grounded in real data rather than assumption — gives decision-makers a chance to act while adjustments are still small and manageable.

#### Research Question

Are California's public pension funds becoming more financially risky over time, and can that risk — along with the key factors driving it — be forecasted using historical actuarial and financial data?

#### Data Sources

Seven datasets published by the California State Controller's Office ("By the Numbers" open data platform, https://bythenumbers.sco.ca.gov), covering all California public retirement systems, FY2002-03 to 2023-24:

1. [Funding Position, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/v3/views/xd4u-ydgs/export.csv?accessType=DOWNLOAD)
2. [Funding Position, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/yp57-bgvx/rows.csv?accessType=DOWNLOAD)
3. [Retirement Systems – Additions](https://bythenumbers.sco.ca.gov/api/views/w4mn-kbdb/rows.csv?accessType=DOWNLOAD)
4. [Retirement Systems – Deductions](https://bythenumbers.sco.ca.gov/api/views/tghp-v9pz/rows.csv?accessType=DOWNLOAD)
5. [Rate of Return, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/views/22d8-yd9n/rows.csv?accessType=DOWNLOAD)
6. [Rate of Return, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/kkkd-2prb/rows.csv?accessType=DOWNLOAD)
7. [Contribution Amounts (ADC), FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/5kqd-v7x4/rows.csv?accessType=DOWNLOAD)

Supplemented with two publicly sourced economic reference series (S&P 500 annual returns and 10-year Treasury yields, FY2003-2024), used in the economic-context and stress-testing analysis.

#### Methodology

- **Two-pass data cleaning:** a fast, uniform audit across all seven raw files (column types, missing values, duplicates), followed by a deep-dive investigation into the two files most central to the research question
- **Data validation:** checking whether values relate to each other the way they mathematically should, which surfaced a real, previously unknown data-entry error in one fund's officially published numbers
- **Outlier analysis:** identifying and documenting mathematically unstable values before they could distort any chart or model
- **Feature engineering:** net cash flow (with a documented imputation decision), contribution-to-payroll ratio, and year-over-year funded ratio change
- **Baseline regression model:** Linear Regression with a time-based train/test split, evaluated using MAE and benchmarked honestly against a naive persistence baseline
- **Economic context and stress testing:** correlating market performance against funded-ratio change, and simulating a repeat of the 2008 financial crisis using the City of Fresno's own historical shock magnitude

#### Results

- The statewide average pension funded ratio fell below 100% around FY2008-09 and has remained in the 71-82% range for over a decade since.
- A fund's own recent funded-ratio trend is the strongest predictor of its near-term future -- confirmed independently across multiple parts of the analysis.
- Every regression approach tested failed to beat a simple "no change from last year" baseline, indicating that funded ratio is highly self-predictive and resistant to added model complexity with the available features.
- Raw annual cash flow and raw same-year market returns showed almost no relationship with funded-ratio change; a 5-year-smoothed market return showed a real, if modest, relationship -- explained by the actuarial smoothing built into how fund assets are reported.
- The City of Fresno's two pension funds have outperformed the statewide average throughout the full period studied. The Employees' Retirement System is on a declining trend that, if it continues, would take it below full funding around 2027 -- a pattern found to be rare when checked against every other California fund with reliable data.
- A stress test using Fresno's own real 2008 shock magnitude shows that a repeat of that shock, starting today, would take the fund from its current healthy position to roughly 65% funded within five years.

#### Next steps

- Expand the regression baseline with regularization, ensemble methods, boosting, and a neural network -- already tested, with the same conclusion holding across all approaches
- Build a formal classification model (Low/Medium/High risk tiers) with explainability (SHAP) to identify which factors most influence predicted risk
- Extend the reusable forecasting pipeline to run systematically across all California funds with reliable data, with safeguards for stale or volatile reporting histories
- Investigate whether the Contribution Deficiency evidence (currently only available from FY2017 onward) can be reliably extended to earlier years
- Report the identified data-entry error back to the California State Controller's Office

#### Outline of project

- [Pension_Fund_Risk_EDA_Final.ipynb](./Pension_Fund_Risk_EDA_Final.ipynb) -- full technical notebook: data acquisition, cleaning, exploratory analysis, feature engineering, outlier analysis, baseline model, economic context, and stress testing

##### Contact and Further Information

Kanwarjit Singh Dhariwal
ksdhariwal@gmail.com
