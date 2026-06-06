# Dataset

The dataset used in this project was provided by NOVA IMS for academic purposes and is **not included in this repository**.

If you want to replicate this analysis, you can use any retail customer dataset with a similar structure. Below is the full variable schema.

---

## Variable Schema

| Variable | Dimension | Description | Type | Measurement |
|----------|-----------|-------------|------|-------------|
| CustID | — | Unique customer identifier | String | Nominal |
| Education | Demographic | Academic degree level | String | Ordinal |
| Marital_Status | Demographic | Marital status | String | Nominal |
| Age | Demographic | Age in years | Integer | Continuous |
| Gender | Demographic | Gender | String | Nominal |
| Dependents | Demographic | Has dependents in household | Boolean | Binary |
| Income | Demographic | Household net income (yearly, €) | Double | Continuous |
| Frequency | RFM | Number of purchases (invoices) in 2025 | Integer | Discrete |
| Monetary | RFM | Total amount spent in 2025 (€) | Double | Continuous |
| Recency | RFM | Days since last purchase | Integer | Continuous |
| Perishables | Product Category | Spend on perishables in 2025 (€) | Double | Continuous |
| Beverages | Product Category | Spend on beverages in 2025 (€) | Double | Continuous |
| Frozen | Product Category | Spend on frozen goods in 2025 (€) | Double | Continuous |
| Canned | Product Category | Spend on canned goods in 2025 (€) | Double | Continuous |
| Others | Product Category | Spend on other categories in 2025 (€) | Double | Continuous |
| Internet | Channel & Satisfaction | % of purchases made online | Double | Continuous |
| NPS | Channel & Satisfaction | Adapted Net Promoter Score (1–5) | Integer | Ordinal |
| Subs | Target (predictive model) | Likely to subscribe to delivery service (0/1) | Boolean | Binary |

---

## Key Data Quality Issues Found

- **35 missing Income values** → imputed with overall mean
- **27 customers with Income = 0** → imputed with median income by education level
- **489 missing Marital_Status** → filled with "Single"
- **169 missing Gender** → filled with "Other"
- **222 missing Education** → assigned "Primary Education" based on age cohort analysis
- **322 customers with negative product category values** (−€2 to €0) → root-caused as returns/refunds, corrected to 0 using `max(value, 0)`
- **10 customers with Frequency = 0 but positive Monetary** → system error (online orders not counted); corrected to Frequency = 1
- **Income outliers above €95,000** → capped using IQR method

---

## Similar Public Datasets

If you want to practice this analysis, these public datasets have a similar structure:

- [UCI Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/online+retail)
- [Kaggle: Mall Customer Segmentation](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python)
- [Kaggle: E-Commerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)
