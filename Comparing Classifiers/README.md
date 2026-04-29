# Comparing Classifiers — Assignment 17.1

## **Overview**
This project evaluates multiple machine learning models to predict whether a customer will subscribe to a term deposit. Because the dataset is highly imbalanced, accuracy alone is not a reliable metric. The goal is to compare default and tuned versions of four classifiers, assess their performance using appropriate evaluation metrics, and recommend the most practical model for real-world deployment.

**Jupyter Notebook:**  
[Comparing_Classifiers.ipynb](Comparing%20Classifiers/notebook/Bank%20Marketing%20Dataset—Comparing_Classifiers.ipynb)

---

## **Dataset**

This project uses the **Bank Marketing Dataset** from the UCI Machine Learning Repository.  
The dataset contains information about direct marketing campaigns conducted by a Portuguese bank, with the goal of predicting whether a customer will subscribe to a term deposit.

**Dataset Source:**  
https://archive.ics.uci.edu/ml/datasets/bank+marketing

**Key Details:**
- 41,188 rows  
- 20 input features (categorical + numerical)  
- Target variable: `y` (yes/no subscription)  
- Highly imbalanced (majority = “no”)  

If included locally, the dataset is stored under:


---

## **Project Structure**
├── data/                 # Dataset

├── notebook/             # Jupyter Notebook (.ipynb)

└── README.md             # Project summary


---

## **Models Evaluated**

### **Default Models**
- Logistic Regression  
- K-Nearest Neighbors (KNN)  
- Decision Tree  
- Support Vector Machine (SVM)

### **Tuned Models (GridSearchCV)**
- Logistic Regression (C, penalty, solver)  
- KNN (k, weights, metric)  
- Decision Tree (depth, leaf size, split criteria)  
- SVM (C, kernel, gamma)

---

## **Evaluation Metrics**
Given the class imbalance, the following metrics were emphasized:

- Accuracy  
- Precision  
- Recall  
- F1-score  
- ROC AUC  
- Confusion Matrix  

Recall and F1-score were especially important for evaluating performance on the minority class (“yes”).

---

## **Executive Summary**

This project compared four machine learning classifiers—Logistic Regression, KNN, Decision Tree, and SVM—using both default and tuned configurations. The baseline model achieved high accuracy (88.73%) but failed to identify any actual subscribers, demonstrating that accuracy alone is misleading for imbalanced datasets.

Logistic Regression and SVM performed best in their default forms, achieving test accuracies around 0.915. KNN and Decision Tree required tuning to improve generalization, with the tuned Decision Tree ultimately achieving the highest accuracy (0.9172). However, tuning time varied dramatically: Logistic Regression trained in seconds, while SVM tuning required over 40 minutes.

Across all models, Logistic Regression delivered the best balance of accuracy, speed, stability, and interpretability. It performed nearly as well as the tuned Decision Tree while being significantly faster and easier to deploy. For these reasons, Logistic Regression is the recommended model for this task.

Future improvements include additional feature engineering, experimenting with ensemble methods, and focusing on recall-oriented metrics to better capture minority-class performance. A deeper error analysis—especially of false negatives—would further refine the model’s ability to identify potential subscribers.

---

## **Key Findings**

### **Baseline**
- Accuracy: **0.8873**
- Recall for subscribers: **0.00**
- Confirms accuracy is not sufficient for imbalanced data.

---

### **Best Default Model**
- **Logistic Regression** and **SVM** (~0.915 accuracy)
- Logistic Regression is faster and more stable.

---

### **Best Tuned Model**
- **Decision Tree (tuned)** achieved the highest accuracy: **0.9172**
- Tuning eliminated overfitting and improved generalization.

---

### **Speed vs. Accuracy**
- **Fastest:** Logistic Regression, Decision Tree  
- **Slowest:** SVM (~40 minutes), KNN (~16 minutes)  
- **Best trade-off:** Logistic Regression

---

### **Overfitting**
- Untuned Decision Tree overfit heavily (train = 1.0).  
- Tuning fixed this completely.

---

## **Final Recommendation**

### **Recommended Model: Logistic Regression (Tuned or Default)**

**Why?**
- High accuracy (~0.915)  
- Extremely fast training  
- Stable across folds  
- Easy to interpret  
- Minimal tuning required  
- Best balance of performance and practicality  

### **Secondary Option**
- **Tuned Decision Tree** if maximum accuracy and interpretability are priorities.

---

## **Next Steps**

1. **Feature Engineering**
   - Remove highly correlated features  
   - Engineer interaction terms  
   - Improve handling of “unknown” categories  

2. **Try Ensemble Methods**
   - Random Forest  
   - Gradient Boosting (XGBoost, LightGBM)  
   - AdaBoost / Bagging  

3. **Use Better Metrics**
   - ROC AUC  
   - Precision–Recall AUC  
   - F1-score  
   - Recall for minority class  

4. **Error Analysis**
   - Investigate false negatives (missed subscribers)  
   - Identify patterns in misclassified examples  

---

## **Author**
**Kanwarjit Singh Dhariwal**  
