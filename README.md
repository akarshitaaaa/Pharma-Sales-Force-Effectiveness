# 💊 Pharma Sales Force Effectiveness — Analytics Dashboard

> **An end-to-end pharmaceutical sales analytics project combining Python (EDA + SQL) and Power BI to uncover revenue trends, doctor performance, regional patterns, and sales rep efficiency across a synthetic global sales dataset.**

---

## 📌 Project Summary

Built a full-stack pharma sales analytics solution using Python (pandas, matplotlib, SQLite) for data validation and exploratory analysis, and Power BI for a three-page interactive dashboard. The project covers 10,000 sales transactions across 8 global regions (2020–2025), 100 doctors, 15 sales representatives, and 5 product categories. Delivered KPIs include Total Revenue (₹232.24M), Revenue per Call, top doctor/rep rankings, and category-level contribution — all surfaced through filterable Power BI dashboards.

**Note:** this project uses a synthetic dataset (sourced from Kaggle) built for analytics practice, not real company financials. Insights and patterns reflect the generated data, not validated real-world pharma behavior.

---

## 📖 Table of Contents

1. [Business Problem](#-business-problem)
2. [Dataset](#-dataset)
3. [Data Model](#-data-model)
4. [Data Cleaning & Validation](#-data-cleaning--validation)
5. [KPIs](#-kpis)
6. [SQL Analysis](#-sql-analysis)
7. [Dashboard Pages](#-dashboard-pages)
8. [Screenshots](#-screenshots)
9. [Key Insights](#-key-insights)
10. [Known Limitations](#-known-limitations)
11. [Folder Structure](#-folder-structure)
12. [How to Run](#-how-to-run)
13. [Future Improvements](#-future-improvements)

---

## 🧩 Business Problem

Pharmaceutical companies sell through a **Company → Sales Rep → Doctor → Patient** chain — doctors, not patients, decide what gets prescribed. With 100 doctors and 15 reps, a company can't visit every doctor equally, so it needs to know:

- Which doctors, regions, and categories drive the most revenue?
- Which sales reps are most active, and is that activity translating into results?
- Where should territory/resourcing decisions focus?

---

## 📊 Dataset

Four relational tables:

| Table | Grain | Rows | Key Columns |
|---|---|---|---|
| `sales_dataset.csv` | one row per transaction | 10,000 | `DOCTOR_ID`, `region`, `category`, `Revenue`, `units_sold`, `unit_price` |
| `doctor_dataset.csv` | one row per doctor | 100 | `Doctor_ID`, `Region`, `Specialization` |
| `rep_dataset.csv` | one row per rep | 15 | `Rep_ID`, `Region`, `Calls_Made`, `Meetings` |
| `territory_dataset.csv` | one row per region | 8 | `Region`, `Manager` |

`sales_dataset` also includes `stock_level`, `expiry_days_remaining`, and `covid_flag` — present in the raw data but not currently used in any KPI or visual (see [Future Improvements](#-future-improvements)).

**Key stats:** ₹232.24M total revenue · 4.47M units sold · 100 doctors · 15 reps · 8 regions · 5 categories · 2020–2025.

---

## 🔗 Data Model

A **relational model built on natural keys** (`DOCTOR_ID`, `Region`) — not a formal star schema with designed surrogate keys. Relationships:

- `sales_data.DOCTOR_ID → doctor_data.Doctor_ID` — active, many-to-one
- `doctor_data.Region → territory_data.Region` — active, many-to-one
- `rep_data.Region → territory_data.Region` — active, many-to-one
- `sales_data.region → territory_data.Region` — **inactive** (Power BI auto-deactivates this to avoid an ambiguous second path between `sales_data` and `territory_data`)

**Known modeling gap:** `sales_data` has no direct key to `rep_data` — they only share a `Region` field, and with multiple reps per region, this isn't a safe join key for rep-level attribution. `Revenue per Call` is therefore a company-wide efficiency estimate, not a true per-rep metric. A proper fix would require a `Rep_ID` column on each transaction, which the source data doesn't include.

---

## 🧹 Data Cleaning & Validation

Performed in Python before SQL/Power BI work:

1. **Duplicate check** — `.duplicated().sum()` on all four tables → zero duplicates found.
2. **Date parsing** — `pd.to_datetime(date, dayfirst=True)` to correctly handle `DD-MM-YYYY` format and avoid silent day/month misreads.
3. **Revenue integrity check** — independently recalculated `units_sold × unit_price` and compared against the provided `Revenue` column to confirm consistency.

---

## 📐 KPIs

| KPI | Formula | Value |
|---|---|---|
| Total Revenue | `SUM(Revenue)` | ₹232.24M |
| Units Sold | `SUM(units_sold)` | 4.47M |
| Revenue per Call | `DIVIDE(SUM(Revenue), SUM(Calls_Made))` | ₹140.75K |
| Avg Revenue per Doctor | `DIVIDE(SUM(Revenue), DISTINCTCOUNT(DOCTOR_ID))` | ₹2.32M |
| Avg Calls per Doctor | `DIVIDE(SUM(Calls_Made), DISTINCTCOUNT(DOCTOR_ID))` | 16.50 |
| Revenue Change % | `DIVIDE(Current − PREVIOUSYEAR(Revenue), PREVIOUSYEAR(Revenue))` | varies by year selected |

---

## 🐍 SQL Analysis

Run against the four tables using CTEs and window functions:

- Doctor revenue by specialization (`LEFT JOIN`)
- Doctor ranking (`RANK() OVER (ORDER BY revenue DESC)`)
- Top 10 doctors (`CTE + LIMIT`)
- Revenue deciles (`NTILE(10)`)
- Top 3 doctors per region (`RANK() OVER (PARTITION BY region ...)`)

---

## 📑 Dashboard Pages

**1. Overview** — Total Revenue, Units Sold, Total Calls, Revenue per Call; Regional Revenue Distribution; Top Performing Doctors; Top Sales Reps by Calls; Revenue by Category; Annual Revenue Trend.

**2. Sales Analysis** — Category and regional breakdowns; Revenue Mix Across Regions & Categories; Revenue vs Units Sold scatter; Monthly Revenue Trend.

**3. Doctor Insights** — Total Doctors, Avg Revenue per Doctor, Avg Calls per Doctor; Revenue Efficiency of Top Doctors; Revenue by Specialization; Units Sold vs Revenue correlation.

---

## 📸 Screenshots

### Overview Page
[Overview Page](<img width="1330" height="798" alt="overview_page" src="https://github.com/user-attachments/assets/d9130655-47e9-49c7-b758-24404e4eb74b" />
)

### Sales Analysis Page
[Sales Analysis Page](<img width="1345" height="786" alt="sales_analysis_page" src="https://github.com/user-attachments/assets/6272e4e7-568a-4cb5-bc6f-73dec5301ec3" />
)

### Doctor Insights Page
[Doctor Insights Page](<img width="1371" height="731" alt="doctor_insights_page" src="https://github.com/user-attachments/assets/2a853d42-277e-4f83-be5b-ae0a768d5ad0" />
)

---

## 💡 Key Insights

1. **2022 revenue drop:** Annual revenue peaked in 2021 (₹52.3M) then fell ~38% in 2022 (₹32.3M), staying flat through 2025. The drop is broad-based — every region and category declined by a similar magnitude — driven by lower units sold per transaction, not price or call-volume changes. Given the uniformity, this more likely reflects a change in how the synthetic data was generated than a real business event.
2. **Regional and category revenue are fairly balanced** — the top and bottom regions/categories differ by roughly 10%, not a dominant outlier.
3. **Top doctors form a tight cluster**, not a single point of dependency — the top 4–5 doctors by revenue are within ~3% of each other.
4. **Revenue per Call is a company-wide estimate**, not a true per-rep metric (see [Data Model](#-data-model)).

---

## ⚠️ Known Limitations

- No **Doctor Conversion Rate** — the dataset has no field indicating whether an engagement led to an actual prescription; this KPI was deliberately excluded rather than estimated.
- No true **Rep-to-Sales attribution** — `rep_data` and `sales_data` only share a `Region` field, not a direct key.
- `stock_level`, `expiry_days_remaining`, and `covid_flag` exist in the raw data but are unused in the current dashboard.
- Small scale (10,000 rows, 100 doctors) — appropriate for a portfolio project, not representative of real pharma transaction volumes.
- Synthetic Kaggle dataset — patterns reflect generated data, not validated real-world behavior.

---

## 📁 Folder Structure

```
pharma-sales-force-effectiveness/
├── pharma_sales_dataset.xlsx
├── sales_dataset.csv
├── doctor_dataset.csv
├── rep_dataset.csv
├── territory_dataset.csv
├── Pharma_sales_project.ipynb
├── pharma_dashboard.pbix
├── screenshots/
│   ├── overview_page.png
│   ├── sales_analysis_page.png
│   └── doctor_insights_page.png
└── README.md
```

---

## ▶️ How to Run

**Python notebook:**
1. Upload the four CSVs to your Colab session (or local environment).
2. Open `Pharma_sales_project.ipynb`, run all cells.
3. Requires: `pandas`, `matplotlib` (`sqlite3` is built into Python).

**Power BI dashboard:**
1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free).
2. Open `pharma_dashboard.pbix`.
3. If prompted, point the data source to your local copy of `pharma_sales_dataset.xlsx`, then Refresh.

---

## 🚀 Future Improvements

- [ ] Add a `Rep_ID` field at the transaction level for true rep-level attribution.
- [ ] Use `stock_level` and `expiry_days_remaining` for an inventory-risk view.
- [ ] Use `covid_flag` to isolate and quantify any COVID-period effect.
- [ ] Investigate the 2022 revenue anomaly against source/generation metadata, if available.
- [ ] Doctor segmentation (e.g., clustering) beyond simple revenue ranking.

---

*Built with Python, SQL, and Power BI.*
