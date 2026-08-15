### Pension Fund Risk Forecasting Using Machine Learning

**Author:** Kanwarjit Singh Dhariwal
**Program:** UC Berkeley Professional Certificate in Machine Learning and Artificial Intelligence — Capstone Project

---

#### Executive Summary

California's public pension funds took a real hit during the 2008 financial crisis and, on average, still haven't fully recovered. That's the starting point for this project. I wanted to know whether that risk is something we can actually measure and forecast, using the kind of financial data these funds already report to the state every year.

The short version: predicting the exact funded ratio a pension fund will report next year is hard. I tried ten different modeling approaches — everything from plain linear regression to a neural network — and almost none of them could beat the simplest possible guess (assume nothing changes from last year). What did work was reframing the question. Instead of asking "what number will this fund report," I asked "is this fund low, medium, or high risk," and that turned out to be a solvable problem, with accuracy up around 90% using a model simple enough that anyone could read its logic directly.

I used the City of Fresno's two pension funds as a running example throughout, since abstract statewide numbers don't mean much to most people. Both of Fresno's funds are currently healthier than the state average. But one of them — the Employees' Retirement System — is on a downward trend that, if it continues, would put it below full funding sometime around 2027. I checked whether that's a coincidence by running the same analysis against every other reliable pension fund in the state, and it isn't. It's a genuinely unusual pattern.

---

#### Rationale

When a pension fund doesn't have enough money to cover what it owes, that gap gets closed somehow — usually through higher taxes, cuts to city services, or, in the worst cases, reduced benefits for retirees who already planned their lives around what they were promised. The problem is that funding gaps build up slowly, over many years, and the reports describing them are written in dense actuarial language that most people don't have the background to parse. By the time a shortfall becomes obvious, it's often already expensive to deal with. If there's a way to flag this earlier, using data that's already public, it seems worth building.

---

#### Research Question

Are California's public pension funds becoming more financially risky over time, and can that risk — along with what's actually driving it — be forecasted from historical financial and actuarial data?

---

#### Business Understanding

Public pensions in California represent a real, ongoing financial commitment to millions of city, county, and state employees. Teachers, firefighters, police officers — their retirements depend on these funds staying solvent. When a fund's assets fall short of its obligations, someone eventually pays for that gap, and it's rarely a quick or painless fix.

This project set out to do three things: figure out whether pension risk in California is actually increasing, identify what factors are behind that risk, and build something that could give an honest, early warning — tested first on Fresno specifically, then checked to see if it holds up on other cities and counties too.

---

#### Data Sources

All data comes from the California State Controller's Office, published through their "By the Numbers" open data platform (https://bythenumbers.sco.ca.gov). It covers every public retirement system in California, from fiscal year 2002-03 through 2023-24. Seven files in total:

1. [Funding Position, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/v3/views/xd4u-ydgs/export.csv?accessType=DOWNLOAD)
2. [Funding Position, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/yp57-bgvx/rows.csv?accessType=DOWNLOAD)
3. [Retirement Systems — Additions](https://bythenumbers.sco.ca.gov/api/views/w4mn-kbdb/rows.csv?accessType=DOWNLOAD)
4. [Retirement Systems — Deductions](https://bythenumbers.sco.ca.gov/api/views/tghp-v9pz/rows.csv?accessType=DOWNLOAD)
5. [Rate of Return, FY2002-03 to 2015-16](https://bythenumbers.sco.ca.gov/api/views/22d8-yd9n/rows.csv?accessType=DOWNLOAD)
6. [Rate of Return, FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/kkkd-2prb/rows.csv?accessType=DOWNLOAD)
7. [Contribution Amounts (ADC), FY2016-17 to 2023-24](https://bythenumbers.sco.ca.gov/api/views/5kqd-v7x4/rows.csv?accessType=DOWNLOAD)

I also pulled in two outside economic reference series — annual S&P 500 returns and 10-year Treasury yields, FY2003-2024 — to test whether broader market conditions helped explain what was happening inside the pension data. That comparison shows up in the economic-context and stress-testing sections.

---

#### Methodology

I approached the cleaning process in two passes: first a quick, uniform check across all seven raw files just to see what I was dealing with, then a much deeper dive into the two or three files that actually mattered most to the research question. Along the way I checked whether values in the data related to each other the way they should mathematically — and that check actually caught a real error in one fund's officially published numbers, which I wasn't expecting to find.

Once the data was clean, I engineered a handful of features: net cash flow, contribution as a percentage of payroll, year-over-year change in funded ratio, and later, a market-return feature that I added specifically because an earlier finding suggested it might matter.

For modeling, I tried ten approaches total — Linear Regression, Ridge, LASSO, Random Forest, AdaBoost, Gradient Boosting, and a Neural Network for the regression side; Logistic Regression, XGBoost, a Decision Tree, and a Neural Network for classification. Every one of them was cross-validated and tuned with grid search, not just fit once and reported. I also ran two statistical significance tests along the way, since a few of my findings needed more than a chart to back them up.

The last piece was turning all of this into something reusable — a single function that can run the same trend analysis on any California pension fund, which I tested on three real funds I hadn't looked at before, chosen specifically because they're different from each other and from Fresno.

---

#### Results

Statewide, the average funded ratio dropped below 100% around 2008 and has hovered in the 71-82% range ever since. That's the headline number, but the more interesting finding is what actually predicts a fund's risk level: overwhelmingly, it's the fund's own recent history. That showed up consistently across correlation checks, model coefficients, and a SHAP analysis — different methods, same answer.

The regression side of this project didn't go the way I expected at first. Seven different approaches, all properly tuned, and none of them beat a naive "assume no change" guess. I could have stopped there, but I went back to an earlier finding — that market returns, smoothed over five years, correlated modestly with funded ratio changes — and tested it as an actual model feature instead of just noting it in passing. That closed most of the gap, and a significance test (p = 0.00003) confirmed the improvement was real, not noise.

Classification worked much better from the start. Four different models, all in the 87-90% accuracy range, with a plain Decision Tree coming out on top at 90.2%. What I liked about that result is that the tree's own decision thresholds landed almost exactly on the 90% and 70% cutoffs I'd picked by hand earlier — a nice, honest confirmation that those numbers weren't arbitrary.

On Fresno specifically: both of its funds have stayed healthier than the state average for the entire 22-year period I looked at. Both are also declining, statistically speaking (p = 0.00006 for the Employees' fund, p = 0.026 for Fire and Police), but from different starting points — only the Employees' fund is projected to actually cross below full funding, and that's forecast to happen around 2027. I also ran a stress test using Fresno's real 2008 shock as a template: if something that size hit again starting today, the fund could drop to roughly 65% funded within five years.

To make sure the Fresno finding wasn't a fluke, I built a reusable version of the same analysis and ran it on three other funds — Los Angeles County (large, showing a real decline), Golden Gate Transit District (small, showing a much steeper decline), and Orange County (essentially flat, no significant trend at all). Then I checked the full statewide picture: among every reliable fund in California, Fresno's Employees' Retirement System is the only one currently healthy but heading toward trouble. That's not something I expected going in.

One more thing worth mentioning, even though it wasn't part of the original question: while checking whether the numbers in the data were internally consistent, I found that the City of Oakland's Fire and Police Retirement System has a units error in its officially published figures — off by exactly 1000x in two separate years.

---

#### Actionable Recommendations

- Fresno's city officials should know about the Employees' Retirement System's trend sooner rather than later. A small contribution adjustment now is a much easier fix than a bigger one later.
- The same goes for Los Angeles County and Golden Gate Transit District — both showed real, statistically significant declines during pipeline testing and probably deserve a closer look.
- If this kind of analysis ever gets turned into an actual monitoring tool, I'd lean toward presenting risk as a simple category (low/medium/high) rather than a precise number, since the Decision Tree model backing that approach is both the most accurate one I tested and the easiest to explain to someone without a data background.
- The Oakland data error should probably get reported back to the State Controller's Office, since other people relying on that dataset might not realize it's wrong.

---

#### Next Steps

A few directions I'd take this further if I kept working on it: apply the market-data feature to the classification models too, since it helped regression significantly; run the reusable pipeline across all ninety-some California funds instead of just the handful I tested, to build something closer to a real dashboard; see if the contribution-deficiency data (one of the stronger signals I found) can be traced back further than its current 2017 starting point; and build a simple calculator that translates a fund's projected shortfall into an actual dollar figure a city could budget for.

---

#### Outline of Project

This capstone is split across four notebooks, each one building on the last:

- [1_Data_Cleaning_and_EDA.ipynb](./1_Data_Cleaning_and_EDA.ipynb) — loading and cleaning all seven raw files, feature engineering, outlier handling, and exploratory analysis
- [2_Modeling_and_Evaluation.ipynb](./2_Modeling_and_Evaluation.ipynb) — all ten models, compared and evaluated
- [3_Fresno_Case_Study_and_Findings.ipynb](./3_Fresno_Case_Study_and_Findings.ipynb) — the Fresno-specific forecast, stress test, and significance testing
- [4_Reusable_Pipeline_and_Findings.ipynb](./4_Reusable_Pipeline_and_Findings.ipynb) — the generalized pipeline, tested on other cities, plus the overall findings

Each notebook saves its results to a file that the next one loads — `model_ready_panel_clean.csv` out of Notebook 1, `best_classifier.pkl` and `model_comparison.csv` out of Notebook 2, `fresno_findings.csv` out of Notebook 3. That way each notebook can be read on its own without having to run everything before it, and it's clear at the top of each one what it needs and what it hands off.

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

Clone the repo, install the dependencies below, then run the four notebooks in order — each one depends on files the previous one creates.

```
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap tensorflow scipy joblib
```

---

##### Contact

Kanwarjit Singh Dhariwal
ksdhariwal@gmail.com

