<div align="center">

# 🛒 QVI Retail Analytics
### Data Analytics Project

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![Pandas](https://img.shields.io/badge/Pandas-1.3+-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![Seaborn](https://img.shields.io/badge/Seaborn-Visualisation-4C72B0?style=for-the-badge&logo=python&logoColor=white)](https://seaborn.pydata.org)
[![Status](https://img.shields.io/badge/Status-Complete-4CAF50?style=for-the-badge&logo=checkmarx&logoColor=white)]()

<br/>

> **End-to-end data analytics project** analysing **264,836 retail chip transactions** across 272 stores —
> covering data cleaning, EDA, sales analysis, product insights, customer segmentation, and business recommendations.

<br/>

| 📦 264,836 Transactions | 🏪 272 Stores | 👥 72,637 Customers | 🏷️ 25+ Brands | 📅 Jul 2018 – Jun 2019 |
|:-:|:-:|:-:|:-:|:-:|

</div>

---

## 📌 Table of Contents

- [📖 Project Overview](#-project-overview)
- [📂 Dataset](#-dataset)
- [📁 Project Structure](#-project-structure)
- [🔍 Notebook Sections](#-notebook-sections)
- [📈 Key Findings](#-key-findings)
- [🛠️ Tech Stack](#️-tech-stack)
- [▶️ How to Run](#️-how-to-run)
- [🔭 Next Steps](#-next-steps)
- [👤 Author](#-author)

---

## 📖 Project Overview

This project performs a **comprehensive data analysis** of QVI retail transaction data (Quantium Virtual Internship). The analysis focuses on understanding customer purchasing behaviour in the chips category across life stages and premium tiers, identifying top-performing products and brands, and delivering actionable business recommendations.

### 🔄 Analytics Pipeline

```
Raw Data  ──►  Cleaning  ──►  EDA  ──►  Sales Analysis  ──►  Product Analysis  ──►  Customer Segmentation  ──►  Insights
```

### ❓ Business Questions Answered

- 📊 Which **customer segments** (life stage × premium tier) drive the most revenue?
- 🏷️ Which **brands and pack sizes** perform best by revenue, volume, and loyalty?
- 🏪 How do **stores** compare in performance and revenue trends?
- ⚠️ Which customers are **Champions, At-Risk, or Lost** based on RFM scoring?
- 💡 What **business actions** can improve retention and margin?

---

## 📂 Dataset

| File | Rows | Description |
|------|:----:|-------------|
| `QVI_transaction_data.xlsx` | 264,836 | Store transactions — product, quantity & sales |
| `QVI_purchase_behaviour.csv` | 72,637 | Customer loyalty card with life stage & premium tier |

<details>
<summary><b>📋 Transaction Data — Column Reference</b></summary>

<br/>

| Column | Type | Description |
|--------|------|-------------|
| `DATE` | `int` | Excel serial date → converted to `datetime` |
| `STORE_NBR` | `int` | Store identifier (272 unique stores) |
| `LYLTY_CARD_NBR` | `int` | Customer loyalty card — **join key** |
| `TXN_ID` | `int` | Unique transaction ID |
| `PROD_NBR` | `int` | Product number |
| `PROD_NAME` | `str` | Full product name — brand & pack size extracted from here |
| `PROD_QTY` | `int` | Units purchased per transaction |
| `TOT_SALES` | `float` | Total transaction value `$` |

</details>

<details>
<summary><b>📋 Purchase Behaviour — Column Reference</b></summary>

<br/>

| Column | Type | Description |
|--------|------|-------------|
| `LYLTY_CARD_NBR` | `int` | Join key to transaction data |
| `LIFESTAGE` | `str` | 7 groups: Young Singles/Couples, Young Families, Midage Singles/Couples, New Families, Older Families, Older Singles/Couples, Retirees |
| `PREMIUM_CUSTOMER` | `str` | `Budget` · `Mainstream` · `Premium` |

</details>

---

## 📁 Project Structure

```
qvi-retail-analytics/
│
├── 📓 QVI_Data_Analytics.ipynb        ← Main analysis notebook (10 sections)
├── 📊 QVI_transaction_data.xlsx       ← Raw transaction data
├── 📋 QVI_purchase_behaviour.csv      ← Customer behaviour data
└── 📄 README.md                       ← You are here
```

---

## 🔍 Notebook Sections

### 0 · ⚙️ Setup & Imports
Auto-installs required libraries and configures matplotlib/seaborn plot styling.

---

### 1 · 📥 Data Loading
Loads both datasets, previews shape, column types, and sample rows.

---

### 2 · 🧹 Data Cleaning & Feature Engineering

- Converted Excel serial dates → proper `datetime` format
- Extracted **brand names** from product name strings
- Extracted **pack sizes** (grams) using regex
- Derived `UNIT_PRICE` = `TOT_SALES / PROD_QTY`
- Added calendar features: `MONTH`, `WEEK`, `QUARTER`, `DAY_OF_WEEK`, `IS_WEEKEND`
- Merged transaction + behaviour on `LYLTY_CARD_NBR`
- Removed bulk-purchase outliers (`PROD_QTY > 100`)

```python
# Key cleaning steps
txn['DATE']       = pd.to_datetime(txn['DATE'], unit='D', origin='1899-12-30')
txn['BRAND']      = txn['PROD_NAME'].str.split().str[0].str.title()
txn['PACK_SIZE']  = txn['PROD_NAME'].str.extract(r'(\d+)[Gg]$').astype(float)
txn['UNIT_PRICE'] = txn['TOT_SALES'] / txn['PROD_QTY']
```

---

### 3 · 📊 Exploratory Data Analysis (EDA)

| Analysis | Chart Type |
|----------|-----------|
| Transaction value distribution | Histogram with mean & median lines |
| Product quantity per transaction | Bar chart |
| Unit price distribution | Histogram |
| Missing values & data quality audit | Summary table |

---

### 4 · 💰 Sales & Revenue Analysis

| Section | What It Covers |
|---------|---------------|
| **4.1 Monthly Trend** | Dual-axis chart — revenue ($k) + transaction volume over 12 months |
| **4.2 Quarterly Performance** | Revenue, transactions, customers & avg basket by quarter |
| **4.3 Day-of-Week & Weekend** | Revenue and transaction patterns — weekday vs weekend highlighted |

---

### 5 · 🏷️ Product Analysis

| Section | What It Covers |
|---------|---------------|
| **5.1 Top Brands** | Top 12 by revenue, units sold, and avg unit price |
| **5.2 Top 10 Products** | Revenue and units sold per individual product |
| **5.3 Pack Size Deep Dive** | Revenue, volume, avg price, and scatter by gram size |
| **5.4 Brand Loyalty** | Repeat purchase rate per brand (top 15) |

---

### 6 · 👥 Customer Analysis

| Section | What It Covers |
|---------|---------------|
| **6.1 Segment Distribution** | Life stage bar chart + premium tier pie chart |
| **6.2 Revenue by Segment** | Absolute revenue + stacked % share (Life Stage × Premium Tier) |
| **6.3 Basket Heatmaps** | Avg spend, avg qty, and unique customers across all segments |
| **6.4 Purchase Frequency** | Transactions, spend, and brands per customer distributions |
| **6.5 Top vs Bottom 10%** | High-value vs low-value customer comparison table |

---

### 7 · 🏪 Store Analysis

| Section | What It Covers |
|---------|---------------|
| **7.1 Store Performance** | Revenue distribution, transaction histogram, avg basket scatter |
| **7.2 Top 5 Stores Trend** | Monthly revenue line chart for top 5 performing stores |

---

### 8 · 🎯 Customer Segmentation (RFM Analysis)

Every customer is scored across three dimensions:

| Dimension | Meaning | Scoring |
|-----------|---------|---------|
| **R**ecency | Days since last purchase | Lower days → Higher score (1–5) |
| **F**requency | Number of transactions | More transactions → Higher score (1–5) |
| **M**onetary | Total spend | Higher spend → Higher score (1–5) |

**Customer Segments:**

| Segment | Description |
|---------|-------------|
| 🏆 **Champions** | High R + High F + High M — best customers |
| 💛 **Loyal Customers** | Frequent buyers with good spend |
| 🌱 **Potential Loyalists** | Recent but infrequent |
| ⚠️ **At Risk** | High historical frequency, gone quiet recently |
| 😴 **Needs Attention** | Below average across all dimensions |
| ❌ **Lost** | Very low recency and frequency |

```python
# RFM Scoring
rfm['R_Score'] = pd.qcut(rfm['Recency'],   5, labels=[5,4,3,2,1]).astype(int)
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 5, labels=[1,2,3,4,5]).astype(int)
rfm['M_Score'] = pd.qcut(rfm['Monetary'],  5, labels=[1,2,3,4,5]).astype(int)
```

Visualised as: segment share pie · avg spend bar · recency vs monetary scatter · RFM × Life Stage stacked bar

---

### 9 · 🔬 Segment Deep Dive

- Top brand per **Life Stage** group
- Top brand per **Premium Tier**
- Heatmap of top 8 brands' revenue share across all life stage segments

---

### 10 · 💡 Key Findings & Business Recommendations

| # | 🔍 Finding | 💡 Recommendation |
|:-:|-----------|------------------|
| 1 | **Older Families & Retirees** generate the highest absolute revenue | Bulk deals, loyalty rewards, and family-size pack promotions |
| 2 | **Mainstream Young Singles/Couples** have the highest transaction frequency | Subscription or stamp-card loyalty programme |
| 3 | **Kettle, Smiths & Pringles** dominate revenue and repeat purchases | Guarantee shelf presence; bundle with dips and drinks |
| 4 | **175g pack** leads revenue; **330g** commands higher avg unit price | Bundle 330g in promotions to lift margin |
| 5 | **~15–20%** of customers are At Risk or Lost per RFM | Re-engagement: targeted discount vouchers via loyalty card |
| 6 | **Weekend** transactions are higher but weekday avg basket is larger | Weekday targeted promotions to boost basket size |
| 7 | Revenue peaks in **Q2 (Oct–Dec)** — holiday season effect | Stock up and run promotions ahead of the Dec–Jan window |

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---------|:-------:|---------|
| `pandas` | ≥ 1.3 | Data loading, cleaning, and aggregation |
| `numpy` | ≥ 1.21 | Numerical operations |
| `matplotlib` | ≥ 3.4 | Core chart rendering |
| `seaborn` | ≥ 0.11 | Heatmaps, statistical plots |
| `openpyxl` | ≥ 3.0 | Reading `.xlsx` Excel files |

---

## ▶️ How to Run

### Prerequisites
- Python `3.8+`
- Jupyter Notebook or JupyterLab

### Steps

**1 · Clone the repository**
```bash
git clone https://github.com/yourusername/qvi-retail-analytics.git
cd qvi-retail-analytics
```

**2 · Install dependencies**
```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

**3 · Launch Jupyter**
```bash
jupyter notebook QVI_Data_Analytics.ipynb
```

**4 · Run all cells**

Go to **Kernel → Restart & Run All**

> 💡 The first cell auto-installs all required packages — no manual setup needed.


---

## 👤 Author

**Ganesh Allu**
📧 ganeshallu258@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/ganesh-allu-b234jh)
🐙 [GitHub](https://github.com/Ganesh147497)




<div align="center">

⭐ **If you found this project useful, please star the repository!** ⭐

</div>
