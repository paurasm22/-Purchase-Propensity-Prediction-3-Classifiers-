

# 📘 **Purchase Propensity Prediction (Customer Churn Model)**

Predicting whether a customer is likely to purchase again using RFM-based Machine Learning.

---

## 📌 **Project Overview**

This project builds an **industry-level machine learning model** that predicts whether a customer will **buy again in the near future**.
It uses the **Online Retail II** dataset (UCI / Kaggle) and applies **RFM feature engineering**, a widely used technique in e-commerce and marketing analytics.

This model can help businesses:

- Identify customers who are likely to churn
- Improve retention
- Personalize marketing campaigns
- Predict future purchases

---

## 🛒 **Business Problem — Why Purchase Propensity?**

E-commerce companies (Amazon, Flipkart, BigBasket, Swiggy) track customer activity to understand:

- **Which customers will return soon?**
- **Which customers are inactive?**
- **Who needs marketing offers?**

Raw transaction data cannot answer these questions directly, so we:

1. Convert transaction-level data → customer-level features
2. Create **Recency, Frequency, Monetary (RFM)** features
3. Build a target variable: **BuyAgain = 1/0**
4. Train ML models to predict future purchases

---

## 📂 **Dataset**

**Source:** Online Retail II Dataset
[https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci)

### Columns in Raw Dataset:

- `InvoiceNo`
- `StockCode`
- `Description`
- `Quantity`
- `InvoiceDate`
- `UnitPrice`
- `CustomerID`
- `Country`

The dataset does **not** contain a target.
So we **create** our own target using business logic.

---

## 🧠 **Feature Engineering (RFM)**

We engineer **RFM features**, widely used in churn prediction and customer analytics.

| Feature       | Meaning                    | How Calculated                       |
| ------------- | -------------------------- | ------------------------------------ |
| **Recency**   | Days since last purchase   | `snapshot_date − last_purchase_date` |
| **Frequency** | Number of unique purchases | Count of unique invoices             |
| **Monetary**  | Total money spent          | Σ (Quantity × UnitPrice)             |

### 🔹 Why RFM?

It converts raw transaction data into **behavioral patterns**, which ML models can understand.

---

## 🎯 **Target Variable: BuyAgain**

We define:

```
BuyAgain = 1  → if customer bought in last 30 days
BuyAgain = 0  → otherwise
```

This is equivalent to a **churn model**, but positive class means “likely to purchase again”.

---

## 🏗️ **ML Pipeline**

We trained **three separate models**, compared them, and selected the best one.

### Models Used:

- **Logistic Regression** (Baseline)
- **Random Forest Classifier**
- **XGBoost Classifier** ⭐ _(Best performing)_

### Preprocessing:

- Scaling numerical features
- Train/test split
- Evaluation on AUC & classification metrics

---

## 📊 **Model Performance**

| Model               | Metric  | Score             |
| ------------------- | ------- | ----------------- |
| Logistic Regression | AUC     | ~0.70             |
| Random Forest       | AUC     | ~0.75             |
| **XGBoost**         | **AUC** | **Best (~0.80+)** |

XGBoost consistently outperformed others in:

- Recall
- Precision for active customers
- AUC score

---

## 🧪 **Code Structure**

```
├── data/
│   └── online_retail_II.csv
├── notebooks/
│   ├── RFM_feature_engineering.ipynb
│   ├── churn_model_training.ipynb
├── src/
│   ├── create_rfm.py
│   ├── train_models.py
│   └── evaluate.py
└── README.md
```

---

## 🧩 **Key Python Steps**

### 1. Create TotalPrice

```python
df['TotalPrice'] = df['Quantity'] * df['UnitPrice']
```

### 2. Create Recency

```python
snapshot_date = df['InvoiceDate'].max() + pd.Timedelta(days=1)
last_purchase = df.groupby('CustomerID')['InvoiceDate'].max()
recency = (snapshot_date - last_purchase).dt.days
```

### 3. Create Frequency

```python
frequency = df.groupby('CustomerID')['InvoiceNo'].nunique()
```

### 4. Create Monetary

```python
monetary = df.groupby('CustomerID')['TotalPrice'].sum()
```

### 5. Build Final Dataset

```python
rfm = pd.DataFrame({'Recency': recency,
                    'Frequency': frequency,
                    'Monetary': monetary})
```

### 6. Target Variable

```python
rfm['BuyAgain'] = (rfm['Recency'] < 30).astype(int)
```

### 7. Train ML Models

(Example: XGBoost)

```python
xgb = XGBClassifier(eval_metric='logloss')
xgb.fit(X_train, y_train)
```

---

## 🚀 **How to Run**

```bash
pip install -r requirements.txt
python src/create_rfm.py
python src/train_models.py
```

Or open the notebooks inside `/notebooks`.

---

## 🧩 **Potential Improvements**

- Hyperparameter tuning
- Try LightGBM
- Use customer segmentation (KMeans)
- Add rolling features or inter-purchase gaps
- Deploy as API (FastAPI / Flask)

---

## 🏁 **Conclusion**

This project demonstrates:

✔ Real-world **churn prediction** using RFM
✔ Transition from **raw transaction data → ML-ready dataset**
✔ Feature engineering that mirrors industry standards
✔ Comparison of 3 classification models
✔ Business insight into customer retention

!
