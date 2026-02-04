
---

## 🎯 Objective

Identify the behavioral and contextual factors that predict whether a driver will accept a Coffee House coupon.  
The goal is to move beyond simple acceptance rates and uncover **multi‑condition behavioral profiles** that meaningfully increase acceptance likelihood.

---

## 🔍 Key Findings

### **Baseline**
- Coffee House coupon acceptance rate: **~50%**

### **Strongest Single Predictor**
- **Frequent CoffeeHouse visitors** → **66% acceptance**

### **Top Pairwise Combinations**
1. **Frequent CoffeeHouse + Coffee‑friendly time (10AM/2PM)** → **77%**
2. **Frequent CoffeeHouse + With Friends/Partner** → **77%**
3. **Frequent CoffeeHouse + Not going home** → **69%**

### **Final Multi‑Condition Profile**
- **Combined group acceptance:** **51%**  
- **Others:** **16%**  
Drivers meeting at least one behavioral condition are **3× more likely** to accept the coupon.

---

## 🧠 Behavioral Profile of High‑Acceptance Drivers

Drivers most likely to accept a Coffee House coupon tend to:

- Visit coffee houses regularly  
- Travel with friends or a partner  
- Drive during **10AM or 2PM**  
- Not be heading home  
- Visit inexpensive restaurants moderately often  
- Be younger (under 30) in some cases  

These patterns suggest that **context and habits** matter more than demographics.

---

## 📊 Methods Used

- Exploratory Data Analysis (EDA)
- Acceptance rate comparisons
- Bar charts and heatmaps
- Correlation analysis with encoded categorical variables
- Individual condition testing
- Pairwise condition testing
- Multi‑condition profiling
- Behavioral segmentation

---

## 📈 Visualizations

The notebook includes:

- Acceptance rate bar charts  
- Correlation heatmap  
- Combined vs. Others acceptance comparison  
- Condition‑based acceptance plots  

These visualizations help reveal patterns not obvious from raw numbers.

---

## 🧪 Technologies Used

- Python  
- pandas  
- numpy  
- seaborn  
- matplotlib  
- Jupyter Notebook  

---

## 📌 Limitations

- Categorical encoding may distort correlation strength  
- Self‑reported behavior introduces bias  
- No temporal context (weekday/weekend, seasonality)  
- Only simple combinations tested (no ML models)  
- Dataset may not generalize to real‑world populations  

---

## 🚀 Future Work

- Logistic regression or decision tree modeling  
- SHAP value analysis for feature importance  
- Time‑aware modeling if timestamps become available  
- A/B testing simulation for coupon targeting strategies  

---

## 📜 License

This project is provided for educational and analytical purposes.

---

## 🙌 Acknowledgments

Dataset provided as part of the coupon acceptance analysis assignment.  
Analysis, refinement, and profiling performed by **Kanwarjit Singh Dhariwal**.

