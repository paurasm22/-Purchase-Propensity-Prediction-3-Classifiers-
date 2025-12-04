

# 📘 **Purchase Propensity Prediction (Customer Churn Model Using RFM & Machine Learning)**

Predicting whether a customer will return to purchase again based on past purchase behavior.

---

## 📌 **Overview**

This project builds an **industry-level customer churn model** (also known as Purchase Propensity Prediction).
The goal is to predict whether a customer will **buy again soon**, using only their past transaction history.

We use **RFM features** (Recency, Frequency, Monetary) derived from raw transactional data and train **three ML models**:

* **Logistic Regression**
* **Random Forest**
* **XGBoost**

This project follows a *corrected pipeline* with **NO data leakage**, ensuring realistic and valid prediction performance.

---

## 🛒 **Business Problem**

Companies like Amazon, Flipkart, Swiggy, and BigBasket need to identify:

* Which customers are likely to buy again soon?
* Which customers are becoming inactive or churning?
* Which customers deserve special offers or retention campaigns?

Raw transactional data does *not* directly provide churn information.
Therefore, we convert it into customer-level behavioral features using the **RFM framework**.

---

## 📂 **Dataset**

**Source:** Online Retail II Dataset
[https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci)

This dataset contains:

* Invoice number
* Product description
* Quantity
* Price
* Timestamp
* Customer ID
* Country

The dataset does **not** contain a target variable.
We create the labels ourselves.

---

# 🎯 **Target Variable: BuyAgain**

We define a simple business rule:

```
BuyAgain = 1  → customer purchased within last 30 days  
BuyAgain = 0  → customer inactive recently  
```

This approximates **customer churn**.

✔ **Recency is used ONLY for target creation**
❌ **Recency is NOT used as a feature (fixed leakage)**

This ensures the ML model cannot cheat.

---

# 🧠 **Feature Engineering (RFM)**

The following customer-level features are extracted:

### ✔ **Recency**

Days since last purchase.
Used **only to build the target**, NOT as an ML feature.

### ✔ **Frequency**

Number of unique invoices → number of purchase occasions.

### ✔ **Monetary**

Total money spent by the customer:

```
Monetary = Σ (Quantity × UnitPrice)
```

These 3 features capture:

* Customer loyalty
* Spending power
* Likelihood of repeat purchase

---

# 🧩 **Final ML Features (Leakage-Free)**

Only:

```
Frequency, Monetary
```

are used as ML inputs.

Recency is EXCLUDED because it directly determines the target.

---

# 🛠️ **Modeling Pipeline**

### Models Trained Separately:

* Logistic Regression
* Random Forest Classifier
* XGBoost Classifier

### Steps:

1. Preprocessing using StandardScaler
2. Train/test split with stratification
3. Model training
4. Evaluation using AUC, classification report, confusion matrix

---

# 📊 **Model Performance (Leakage Fixed)**

| Model                   | AUC Score |
| ----------------------- | --------- |
| **Logistic Regression** | **0.737** |
| Random Forest           | 0.672     |
| **XGBoost**             | **0.742** |

These scores are **realistic and valid** for churn prediction with limited features.

---

# 📜 **Key Learning: Fixing Data Leakage**

Originally, Recency was used both:

* To create the target
* As an input feature

This caused **artificially perfect AUC (1.00)**.
We FIXED it by removing Recency from training features.

This brings the model to realistic industry performance and demonstrates correct ML practices.

---

# 📁 **Project Structure**

```
├── data/
│   └── online_retail_II.csv
├── notebooks/
│   ├── 01_RFM_feature_engineering.ipynb
│   ├── 02_churn_model_training.ipynb
├── src/
│   ├── create_rfm.py
│   ├── train_models.py
│   └── utils.py
├── README.md
└── requirements.txt
```

---

# ▶️ **How to Run**

```bash
pip install -r requirements.txt
python src/create_rfm.py
python src/train_models.py
```

or open the notebooks in Jupyter/Colab.

---

# 🚀 **Future Improvements**

Here are enhancements that can increase performance:

### ✔ Add Recency Groups (safe, no leakage)

Bin Recency into categories instead of using raw values.

### ✔ Use Interpurchase Gap Features

More powerful than simple RFM.

### ✔ Perform Hyperparameter Tuning

Especially for XGBoost & Random Forest.

### ✔ Add Customer-Level Features

* Number of unique products purchased
* Avg order value
* Days between purchases
* Country

### ✔ Deploy model as API (FastAPI/Flask)

---

# 🏁 **Conclusion**

This project demonstrates:

✔ End-to-end ML pipeline using transaction data
✔ Correct customer-level feature engineering (RFM)
✔ Proper target creation for churn prediction
✔ Avoidance of data leakage
✔ Comparison of 3 ML models
✔ Realistic business-grade evaluation



