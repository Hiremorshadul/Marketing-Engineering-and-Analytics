# 🛒 Marketing Analytics: Customer Segmentation & Predictive Modeling

> **End-to-end data science project** applying RFM analysis, K-Means clustering, and neural network classification to 20,000 retail customers — translating model outputs into actionable marketing strategy and measurable business ROI.

---

## 📊 Key Results at a Glance

| Metric | Value |
|--------|-------|
| Dataset size | 20,000 customers |
| Segmentation method | K-Means (K=3, Silhouette = 0.283) |
| Best predictive model | Neural Network (AUC = **0.851**) |
| Targeted campaign ROI | **€3,733** net profit (top 20% of customers) |
| Mass campaign result | **−€1,720** net loss |
| **Value of using the model** | **€5,500+ per campaign** |

---

## 🧠 Business Problem

**NEW SMI** is a Portuguese supermarket chain with 50+ years of operation that had been relying entirely on undifferentiated mass marketing. The goal of this project was to:

1. **Segment** 20,000 customers into distinct behavioral profiles to enable personalized marketing
2. **Predict** which customers are most likely to subscribe to a new home delivery service
3. **Quantify** the financial impact of data-driven targeting vs. random outreach

---

## 🔧 Tech Stack

- **KNIME Analytics Platform** — full pipeline from EDA to model evaluation
- **Methods:** K-Means Clustering, Decision Trees, Neural Networks (RProp MLP)
- **Techniques:** RFM Analysis, Silhouette Analysis, ROC/AUC, Lift Charts, Profit Curve Analysis
- **Feature Engineering:** Outlier capping (IQR), missing value imputation, percentage-based category variables, incoherence detection

---

## 📁 Repository Structure

```
marketing-analytics-customer-segmentation/
│
├── README.md                                        ← You are here
│
├── report/
│   └── customer-segmentation-predictive-modeling-report.pdf
│
├── workflow/
│   └── NEW_SMI_Analytics.knwf                       ← Full KNIME pipeline
│
├── assets/
│   ├── Customer Distribution by Cluster.png
│   ├── lift chart.png
│   └── profit and lost analysis.png
│
└── data/
    └── README.md                                    ← Schema & variable descriptions
                                                       (data not included — school-provided)
```

---

## 🔍 Methodology

### 1. Data Exploration & Preparation
- Explored 16 variables across 4 dimensions: demographics, RFM behavior, product categories, channel & satisfaction
- Identified and treated **income outliers** via IQR capping (capped at €95,000)
- Resolved **negative product values** (returns recorded as negatives) using `max(value, 0)` floor function
- Imputed **35 missing income values** using overall mean; **27 zero-income entries** imputed using median by education level
- Detected a **system error** where online-only customers showed Frequency=0 despite positive spend — corrected to Frequency=1
- Built **percentage-based category variables** (e.g., `Perishables_Pct`) to capture product preference independent of spending power

### 2. RFM Segmentation (Pre-clustering)
Scored all 20K customers 1–5 on Recency, Frequency, and Monetary to build 6 strategic segments:

| Segment | % of Base | Priority |
|---------|-----------|----------|
| At Risk | 32.5% | Win-back campaigns (urgent) |
| Others | 30.5% | Mid-tier engagement |
| Loyal | 14.5% | Retention & rewards |
| Champions | 9.25% | VIP program |
| Lost | 8.0% | Low priority |
| New | 5.25% | Onboarding campaigns |

### 3. K-Means Customer Segmentation

Tested K=2 through K=6 using Mean Silhouette Coefficient. Selected **K=3** for business interpretability.

![Customer Distribution by Cluster](assets/Customer%20Distribution%20by%20Cluster.png)

**Three customer segments identified:**

| Cluster | Size | Avg Age | Avg Income | Avg Spend/yr | Online % | Profile |
|---------|------|---------|------------|--------------|----------|---------|
| Loyal High-Spenders | 5,445 (27%) | 70 | €68,307 | €10,279 | **80%** | Premium digital shoppers |
| Occasional Low-Spenders | 8,230 (41%) | 35 | €28,870 | €1,138 | 44% | Price-sensitive, disengaged |
| Regular Mid-Spenders | 6,325 (32%) | 53 | €50,518 | €4,020 | 56% | Fresh produce loyalists |

> 💡 **Counterintuitive finding:** The oldest segment (avg. age 70) is the most digitally active — 80% of purchases online. Data beats assumptions every time.

### 4. Predictive Modeling

Binary classification to predict home delivery subscription likelihood (70/30 stratified split).

**Models compared:**

| Model | Accuracy | Recall | Precision | Kappa | AUC |
|-------|----------|--------|-----------|-------|-----|
| Decision Tree 1 | 0.910 | 0.323 | 0.625 | 0.382 | 0.677 |
| Decision Tree 2 | 0.918 | 0.435 | 0.659 | 0.482 | 0.749 |
| Decision Tree 3 | 0.903 | 0.355 | 0.550 | 0.381 | 0.655 |
| **Neural Network 1** ✅ | 0.892 | 0.419 | 0.473 | 0.385 | **0.851** |
| Neural Network 2 | 0.893 | 0.435 | 0.482 | 0.399 | 0.833 |

**Selected: Neural Network 1** — highest AUC (0.851). In campaign targeting, AUC is the critical metric as it measures how well the model ranks customers by subscription propensity.

![Lift Chart](assets/lift%20chart.png)

> Note: An earlier NN config (8 hidden layers, 5 neurons/layer) achieved AUC 0.882 but identified **zero subscribers** on validation — textbook overfitting. Architecture was revised to 3 layers × 11 neurons.

### 5. Profit Analysis

Applied to a validation set of 2,000 customers (profit = €35/subscription, contact cost = €4.50/customer, baseline = 10.4%):

![Profit and Cost Analysis](assets/profit%20and%20lost%20analysis.png)

| Strategy | Customers Contacted | Conversion Rate | Net Profit |
|----------|---------------------|-----------------|------------|
| Top 20% targeted | 400 | **~40%** (4× baseline) | **€3,733** |
| Full base (mass) | 2,000 | 10.4% | **−€1,720** |

**The model generates €5,500+ more value than mass outreach per campaign.**

---

## 📣 Marketing Actions by Segment

**Loyal High-Spenders** → Premium loyalty program + delivery subscription push via app/email (natural fit: 80% already buy online)

**Occasional Low-Spenders** → Time-limited reactivation vouchers + private label promotions via social media and push notifications

**Regular Mid-Spenders** → Weekly/bi-weekly fresh basket subscription + cross-sell bundles pairing perishables with frozen/canned items

---

## 📂 Dataset

The dataset was provided by NOVA IMS for academic purposes and **cannot be shared publicly**. See [`data/README.md`](data/README.md) for the full variable schema so you can replicate the analysis on similar retail data.

**Dataset summary:** 20,000 customers · 16 variables · demographics, RFM behavior, product category spending, online engagement, NPS

---

## 📄 Full Report

The complete 27-page academic report is available in [`report/`](report/customer-segmentation-predictive-modeling-report.pdf).

---

## 👤 Authors

**Morshadul Alam** — [LinkedIn](https://www.linkedin.com/in/morshadul/)

Group BN — NOVA IMS, Marketing Engineering and Analytics, 2025/26
> Joana Nazário · Laura Tavares · Mª Leonor Cunha · Morshadul Alam · Soraia Neves

---

## 🏫 Academic Context

**Course:** Marketing Engineering and Analytics  
**Institution:** NOVA IMS — Information Management School, Lisbon  
**Year:** 2025/26  
**Tools:** KNIME Analytics Platform

---

*Questions about the methodology? Open an issue or connect on LinkedIn.*
