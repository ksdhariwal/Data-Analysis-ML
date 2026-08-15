### Pension Fund Risk Forecasting Using Machine Learning

**Author:** Kanwarjit Singh Dhariwal
**Program:** UC Berkeley Professional Certificate in Machine Learning and Artificial Intelligence -- Capstone Project

---

#### Executive Summary

This project investigates whether California's public pension funds are becoming more financially risky over time, and whether that risk can be forecasted using historical financial and actuarial data. Using seven government datasets spanning 22 years and roughly 130 California pension systems, I found that the statewide average fund fell below full funding after the 2008 financial crisis and has not fully recovered. Forecasting a fund's exact future funded ratio turned out to be a genuinely hard problem -- ten different modeling approaches, from simple regression through a neural network, mostly failed to beat a simple "assume no change" baseline, until I added a market-context feature that closed most of that gap, with the improvement confirmed statistically significant. Classifying a fund's risk level (Low/Medium/High) worked well across every model I tried, reaching as high as 90% accuracy with a fully interpretable Decision Tree. Using the City of Fresno's two pension funds as a detailed case study, I found both are healthier than the statewide average, but one -- the Employees' Retirement System -- is on a statistically significant declining trend that could take it below full funding around 2027. I then built a reusable tool that runs this same analysis on any California fund, and demonstrated it live on three real, deliberately different cities and counties, confirming the approach genuinely generalizes beyond Fresno.

---

#### Rationale

Public pension underfunding is not an abstract accounting issue. When a fund's assets fall short of what it owes, the shortfall is typically closed through higher taxpayer-funded contributions, reduced government services, or, in the worst cases, reduced benefits for retirees who planned their lives around promised income. Because pension funding gaps develop slowly and are described in dense actuarial language, they are easy for policymakers, city officials, and the public to overlook until the problem becomes expensive and urgent. An early-warning approach, grounded in real data rather than assumption, gives decision-makers a chance to act while adjustments are still small and manageable.

---

#### Research Question

Are California's public pension funds becoming more financially risky over time, and can that risk -- along with the key factors driving it -- be forecasted using historical actuarial and financial data?

---

#### Business Understanding

California's public pension funds hold trillions of dollars in obligations to city, county, and state employees -- teachers, firefighters, police officers, and other public servants. When a fund's assets fall short of what it owes, the shortfall doesn't disappear; it eventually gets paid for, usually through some mix of higher taxpayer contributions, reduced government services, or reduced benefits for people who planned their retirement around what they were promised. Because pension funding gaps develop slowly and get described in dense actuarial language, they are genuinely easy to overlook until they become expensive and urgent.

This project set out to answer three practical questions: **(1)** are California's pension funds becoming riskier over time, **(2)** what factors actually drive that risk, and **(3)** can a tool be built that gives an honest early warning, before a crisis, using the City of Fresno's own two pension funds as a real, concrete test case -- and that generalizes to any other California fund, not just Fresno?

---

#### Data Sources

Seven datasets published by the California State Controller's Office ("By the Numbers" open data platform, https://bythenumbers.sco.ca.gov), covering all California public retirement systems, FY2002-03 to 2023-24:

1. [Funding Position, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/v3/views/xd4u-ydgs/export.csv?accessType=DOWNLOAD)
2. [Funding Position, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/yp57-bgvx/rows.csv?accessType=DOWNLOAD)
3. [Retirement Systems -- Additions](https://bythenumbers.sco.ca.gov/api/views/w4mn-kbdb/rows.csv?accessType=DOWNLOAD)
4. [Retirement Systems -- Deductions](https://bythenumbers.sco.ca.gov/api/views/tghp-v9pz/rows.csv?accessType=DOWNLOAD)
5. [Rate of Return, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/views/22d8-yd9n/rows.csv?accessType=DOWNLOAD)
6. [Rate of Return, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/kkkd-2prb/rows.csv?accessType=DOWNLOAD)
7. [Contribution Amounts (ADC), FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/5kqd-v7x4/rows.csv?accessType=DOWNLOAD)

Supplemented with two publicly sourced economic reference series (S&P 500 annual returns and 10-year Treasury yields, FY2003-2024), used in the economic-context and stress-testing analysis.

---

#### Methodology

- **Two-pass data cleaning:** a fast, uniform audit across all seven raw files, followed by a deep-dive investigation into the files most central to the research question
- **Data validation:** checking whether values relate to each other the way they mathematically should, which surfaced a real, previously unknown data-entry error in one fund's officially published numbers
- **Outlier analysis:** identifying and documenting mathematically unstable values before they could distort any chart or model
- **Feature engineering:** net cash flow, contribution-to-payroll ratio, year-over-year funded ratio change, and a market-context feature added and statistically validated mid-project
- **Ten modeling approaches, all cross-validated and grid-searched:** Linear Regression, Ridge, LASSO, Random Forest, AdaBoost, Gradient Boosting, and a Neural Network for regression; Logistic Regression, XGBoost, Decision Tree, and a Neural Network for classification
- **Statistical significance testing:** confirming both the Fresno trend and the market-feature model improvement are real effects, not chance, using paired t-tests and non-parametric checks
- **A reusable, generalized pipeline:** a single function that runs the full trend-forecast-and-reliability analysis on any California fund, demonstrated on three real, deliberately different cases beyond Fresno

---

#### Results

- The statewide average pension funded ratio fell below 100% around FY2008-09 and has remained in the 71-82% range for over a decade since.
- A fund's own recent funded-ratio trend is the strongest predictor of its near-term future, confirmed independently across correlation checks, regression coefficients, SHAP analysis, and the structure of the best-performing model.
- Regression modeling initially looked like a dead end -- seven approaches failed to beat a naive baseline -- until adding a market-context feature brought the error down substantially, a statistically significant improvement (p = 0.00003).
- Reframed as classification, the problem is genuinely solvable: four different classifiers reached 87.5%-90.2% accuracy, with a fully interpretable Decision Tree performing best.
- The City of Fresno's two pension funds have outperformed the statewide average throughout the period studied. Both show statistically significant declining trends; only the Employees' Retirement System is forecast to cross below full funding, around 2027.
- A stress test using Fresno's own real 2008 shock magnitude shows a repeat of that shock, starting today, would take the fund to roughly 65% funded within five years.
- The reusable pipeline was demonstrated on three real, deliberately different California funds -- a large county system, an already high-risk fund, and a statistically typical fund -- and correctly identified the risk level of each, confirming the approach generalizes beyond Fresno.
- Checked against every other reliable California fund, Fresno's Employees' Retirement System is the only one currently healthy but on a trajectory to cross below full funding by 2027 -- a genuinely rare, statistically checked pattern.
- A previously unknown data quality issue was identified along the way: the City of Oakland Fire and Police Retirement System's officially published data contains a units error, off by exactly 1000x in two separate years.

---

#### Actionable Recommendations

- **For Fresno specifically:** city officials should be made aware of the Employees' Retirement System's funding trend now, while a modest, proactive contribution adjustment is still a realistic option.
- **For the other funds flagged during pipeline validation:** Los Angeles County and Golden Gate Transit District both show statistically real declining trends worth the same kind of attention given to Fresno.
- **For any statewide early-warning system built from this work:** present fund risk as a category (Low/Medium/High) using the Decision Tree model, which is both the most accurate approach tested and fully explainable to a non-technical audience.
- **For the California State Controller's Office:** the identified Oakland data error should be corrected, since other researchers and policymakers may be relying on the same published numbers.

---

#### Next Steps

- Apply the market-data feature improvement to the classification models as well, to see if it helps there too
- Run the reusable pipeline systematically across all California funds with reliable data, to build a complete statewide risk dashboard
- Investigate whether the Contribution Deficiency evidence -- one of the strongest signals found -- can be reliably extended further back than its current starting point
- Build a lightweight, transparent budget calculator translating a fund's projected shortfall into a concrete required annual contribution
- Explore a larger, richer feature set to further close the gap between model performance and the naive baseline

---

#### Outline of Project

This capstone is delivered as a collection of four Jupyter Notebooks, each handing off its output to the next:

- [1_Data_Cleaning_and_EDA.ipynb](./1_Data_Cleaning_and_EDA.ipynb) -- loads and cleans all seven raw data files, engineers features, handles outliers, and explores the clean data
- [2_Modeling_and_Evaluation.ipynb](./2_Modeling_and_Evaluation.ipynb) -- builds and compares all ten models (regression, classification, and a neural network), all cross-validated and grid-searched
- [3_Fresno_Case_Study_and_Findings.ipynb](./3_Fresno_Case_Study_and_Findings.ipynb) -- a detailed look at the City of Fresno's two pension funds, including a forecast to 2027 and a statistical significance test
- [4_Reusable_Pipeline_and_Findings.ipynb](./4_Reusable_Pipeline_and_Findings.ipynb) -- a general-purpose function that runs this analysis on any California fund, demonstrated on real examples, plus the overall capstone findings

**Data flow between notebooks:** each notebook saves its output to a file that the next notebook loads at the start -- `model_ready_panel_clean.csv` (from Notebook 1), `best_classifier.pkl` and `model_comparison.csv` (from Notebook 2), and `fresno_findings.csv` (from Notebook 3) -- so each notebook can also be reviewed independently, with a clear statement at the top of what it needs and what it produces.

##### Repository Structure

```
├── 1_Data_Cleaning_and_EDA.ipynb
├── 2_Modeling_and_Evaluation.ipynb
├── 3_Fresno_Case_Study_and_Findings.ipynb
├── 4_Reusable_Pipeline_and_Findings.ipynb
├── README.md
└── data/
    ├── export.csv
    ├── Funding_Position_for_Fiscal_Years_2016-17_to_2023-24.csv
    ├── Retirement_Systems_-_Additions.csv
    ├── Retirement_Systems_-_Deductions.csv
    ├── Rate_of_Return_for_Fiscal_Years_2002-03_to_2015-16.csv
    ├── Rate_of_Return_for_Fiscal_Years_2016-17_to_2023-24.csv
    └── Contribution_Amounts_for_Fiscal_Years_2016-17_to_2023-24.csv
```

##### How to Run

1. Clone this repository
2. `pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap tensorflow scipy joblib`
3. Run the four notebooks in numerical order (1 through 4) -- each one saves the files the next one needs

---

##### Contact and Further Information

Kanwarjit Singh Dhariwal
ksdhariwal@gmail.com
