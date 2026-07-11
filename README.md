# 💊 Pharma Sales Analytics Dashboard

> **A comprehensive end-to-end pharmaceutical sales analytics project combining Python (EDA + SQL) and Power BI to uncover revenue trends, doctor performance, regional patterns, and product category insights across a global sales dataset.**

---

## 📌 Project Summary

Built a full-stack pharma sales analytics solution using Python (pandas, matplotlib, SQLite) for data preprocessing and exploratory analysis, and Power BI for interactive multi-page dashboards. The project covers 10,000 sales transactions across 8 global regions (2020–2025), 100 doctors, 15 sales reps, and 5 product categories. Delivered actionable KPIs including total revenue of ₹232M+, revenue per call, top doctor and rep rankings, and category-level contribution analysis — all surfaced through filterable, drill-down Power BI dashboards.

---

## 📖 Table of Contents

1. [Project Overview](#-project-overview)
2. [Business Problem Statement](#-business-problem-statement)
3. [Project Objectives](#-project-objectives)
4. [Technologies Used](#️-technologies-used)
5. [Dataset Description](#-dataset-description)
6. [Data Cleaning & Preprocessing](#-data-cleaning--preprocessing)
7. [Data Model & Relationships](#-data-model--relationships)
8. [Dashboard Features](#-dashboard-features)
9. [KPIs Used](#-kpis-used)
10. [Power BI Concepts Used](#️-power-bi-concepts-used)
11. [Python Analysis Performed](#-python-analysis-performed)
12. [Key Business Insights](#-key-business-insights)
13. [Dashboard Pages Explanation](#-dashboard-pages-explanation)
14. [Screenshots](#-screenshots)
15. [Folder Structure](#-folder-structure)
16. [How to Run the Project](#️-how-to-run-the-project)
17. [Future Improvements](#-future-improvements)
18. [Conclusion](#-conclusion)
19. [Author](#-author)

---

## 🔍 Project Overview

This project analyzes a simulated pharmaceutical company's global sales operations. It covers doctor-level sales performance, regional revenue distribution, product category contribution, sales representative efficiency, and year-over-year revenue trends. The analysis is delivered through:

- **Python (Google Colab)** — data ingestion, cleaning, EDA, visualization, and SQL-based analysis
- **Power BI** — three interactive dashboard pages for Overview, Sales Analysis, and Doctor Insights

The project demonstrates proficiency in the full analytics pipeline: raw data → preprocessing → EDA → SQL querying → business intelligence reporting.

Unlike FMCG or retail, pharmaceutical companies generally do not sell prescription medicines directly to patients — sales flow from **Company → Sales Representative → Doctor → Patient**. Representatives promote medicines to doctors, and doctors decide what to prescribe. That's why the project centers on doctor-level and rep-level performance rather than direct consumer sales.

---

## 🧩 Business Problem Statement

A pharmaceutical company operating across 8 global regions is experiencing stagnant sales due to inefficient doctor targeting, uneven sales force performance, and suboptimal territory allocation. The project needs to:

- Understand **which doctors, regions, and product categories** are driving the most revenue
- Identify **sales representative efficiency** (revenue per call, revenue per meeting)
- Track **annual and monthly revenue trends** to detect performance drops
- Enable **data-driven decisions** for territory management and resource allocation

Without a consolidated analytics view, the business is unable to pinpoint underperformance, reward top performers, or proactively respond to revenue dips.

**Note on scope:** The Kaggle-based dataset used here does not include a field indicating whether a doctor converted into an active prescriber, so "Doctor Conversion Rate" is intentionally excluded as a KPI. Metrics were limited to what could be calculated reliably from the available data (Revenue per Call, Average Calls per Doctor, Revenue by Territory, Top Performing Doctors, etc.) rather than estimating or inventing a conversion metric.

---

## 🎯 Project Objectives

- ✅ Aggregate and clean multi-table sales data across doctors, reps, and territories
- ✅ Perform exploratory data analysis (EDA) to understand revenue distributions and top performers
- ✅ Use SQL window functions (RANK, ROW_NUMBER, NTILE, CTEs) to derive ranked insights
- ✅ Build a 3-page interactive Power BI dashboard with slicers for Region, Category, Year, Month, and Specialization
- ✅ Surface actionable KPIs: Total Revenue, Revenue per Call, Avg Calls per Doctor, and Units Sold
- ✅ Identify revenue anomalies (e.g., the 2022 revenue drop) and seasonal patterns

---

## 🛠️ Technologies Used

| Tool / Technology    | Purpose                                             |
| --------------------- | --------------------------------------------------- |
| **Python 3**         | Data loading, cleaning, EDA, visualization          |
| **pandas**           | Data manipulation and merging                       |
| **matplotlib**       | Revenue distribution and bar chart visualizations   |
| **SQLite3**          | In-memory relational database for SQL analysis      |
| **SQL**              | Window functions, CTEs, subqueries, ranked analysis |
| **Power BI Desktop** | Interactive dashboard development                   |
| **DAX**              | KPI measures                                        |
| **Google Colab**     | Notebook environment                                |
| **Excel (.xlsx)**    | Data cleaning and initial inspection                |

---

## 📊 Dataset Description

The project uses **four relational tables**, each representing a distinct business entity, derived from a primary Excel file and exported as CSVs. Keeping them separate (rather than one flat sheet) reduced redundancy, made joins easier in SQL, and supported a relational data model in Power BI.

### 1. `sales_dataset.csv` — 10,000 rows × 16 columns

| Column                  | Description                                                               |
| ------------------------ | --------------------------------------------------------------------------- |
| `date`                  | Transaction date (DD-MM-YYYY)                                             |
| `year`                  | Year of transaction (2020–2025)                                           |
| `month`                 | Month number                                                              |
| `day`                   | Day number                                                                |
| `region`                | Sales region (8 global regions)                                           |
| `country`               | Country within the region                                                 |
| `category`              | Product category (Chronic, Vitamin, Antipyretic, Cough_Cold, Antibiotic) |
| `medicine`              | Specific medicine name                                                    |
| `age_group`             | Patient age group (0-12, 13-25, 26-45, 46-65)                             |
| `units_sold`            | Number of units sold in the transaction                                   |
| `unit_price`            | Price per unit                                                            |
| `stock_level`           | Remaining stock level                                                     |
| `expiry_days_remaining` | Days until product expiry                                                 |
| `covid_flag`            | Binary flag indicating COVID-era transaction                              |
| `Revenue`               | Total transaction revenue (units_sold × unit_price)                     |
| `DOCTOR_ID`             | Foreign key linking to the doctor table                                   |

### 2. `doctor_dataset.csv` — 100 rows × 3 columns

| Column           | Description                                                                       |
| ----------------- | ----------------------------------------------------------------------------------- |
| `Doctor_ID`      | Unique doctor identifier (D001–D100)                                              |
| `Region`         | Doctor's operating region (8 regions)                                             |
| `Specialization` | Medical specialization (Cardiologist, Neurologist, General Physician, Orthopedic) |

### 3. `rep_dataset.csv` — 15 rows × 4 columns

| Column       | Description                     |
| ------------- | --------------------------------- |
| `Rep_ID`     | Sales representative identifier |
| `Region`     | Territory/region assigned       |
| `Calls_Made` | Total sales calls made          |
| `Meetings`   | Total meetings conducted        |

### 4. `territory_dataset.csv` — 8 rows × 2 columns

| Column    | Description               |
| ---------- | --------------------------- |
| `Region`  | Region name               |
| `Manager` | Assigned regional manager |

**Key Statistics:**

- 📅 Date Range: 2020 – 2025
- 💰 Total Revenue: **₹232.24M**
- 📦 Total Units Sold: **4,470,825**
- 👨‍⚕️ Total Doctors: **100**
- 🌍 Regions: **8** (Africa, East Asia, Europe, Middle East, North America, Oceania, South America, South Asia)
- 💊 Product Categories: **5**

---

## 🧹 Data Cleaning / Preprocessing

All preprocessing was performed in Python (Google Colab):

1. **Date Parsing:** Converted `date` column from string to `datetime` using `pd.to_datetime()` with `dayfirst=True` to handle DD-MM-YYYY format
2. **Duplicate Check:** Validated zero duplicates across all four tables using `.duplicated().sum()`
3. **Revenue Validation:** Cross-verified the `Revenue` column by computing `units_sold × unit_price` as `calculated_revenue` and confirmed alignment with the existing `Revenue` field
4. **Table Merging:**
   - `sales` LEFT JOIN `doctor` on `DOCTOR_ID` → enriched sales with doctor region and specialization
   - `merged` LEFT JOIN `rep` on `Region` → enriched with sales rep call and meeting data (the sales table has no `Rep_ID`, so reps are joined at the region level rather than the individual transaction level)
5. **Column Cleanup:** Dropped redundant `Doctor_ID` column post-merge to avoid duplication
6. **Database Loading:** All four cleaned tables were loaded into an in-memory **SQLite database** (`pharma.db`) for SQL-based analysis

Initial cleaning (handling missing values, removing duplicates, correcting formatting inconsistencies) was done in Excel since the dataset size was manageable. Python was then used for exploratory analysis, and Power BI for data modeling, DAX calculations, and dashboard development rather than data prep.

---

## 🔗 Data Model & Relationships

The Power BI data model consists of **four related tables** — `sales_data`, `doctor_data`, `rep_data`, and `territory_data` — connected through a **relational data model**, not a star schema. Given the project only involved four datasets, the relationships were kept simple rather than redesigning the data into a formal star schema with separate fact/dimension layers.

- `sales_data` is the central transactional table, containing revenue, units sold, and sales-related information.
- `doctor_data` connects to `sales_data` via `Doctor_ID` in a **one-to-many** relationship (one doctor → many sales transactions).
- `territory_data` connects via the `Region` field, enabling regional revenue analysis.
- Since `sales_data` has no `Rep_ID` column, `rep_data` is connected through the common `Region` field rather than a direct transactional key.

This approach reduces data duplication, maintains data integrity, and lets Power BI combine information across tables efficiently for interactive analysis — without the overhead of a full star-schema redesign that wasn't necessary at this data scale.

---

## 📋 Dashboard Features

### Interactive Filters / Slicers

- **Region** — filter by any of the 8 global regions
- **Category** — filter by product category (Antibiotic, Antipyretic, Chronic, Cough_Cold, Vitamin)
- **Year** — filter by year (2020, 2021, 2022, 2023, 2024, 2025)
- **Month** — filter by calendar month (Overview/Sales pages)
- **Specialization** — filter by doctor specialization (Doctor Insights page)

An **interactive slicer** goes beyond basic filtering — selections dynamically update multiple visuals, KPI cards, and charts across the report through Power BI's filter context and visual interactions.

### Chart Types Used

- KPI Cards (with trend indicators vs prior period / YoY)
- Horizontal Bar Charts (top doctors, top reps, regional revenue, category revenue)
- Stacked Bar Chart (revenue mix by region and category)
- Scatter Plot (units sold vs revenue correlation per doctor, color-coded by region)
- Line Chart (annual and monthly revenue trends with average reference lines)

---

## 📐 KPIs Used

| KPI                        | Formula                                              | Value (All-time) | Trend                  |
| ---------------------------- | ------------------------------------------------------- | ------------------- | ------------------------- |
| **Total Revenue**          | SUM(Revenue)                                          | ₹232.24M         | ↑ 5.1% YoY             |
| **Units Sold**              | SUM(Units Sold)                                       | 4M                | ↑ 2.3% vs prior period |
| **Total Calls**             | SUM(Calls Made)                                       | 2K                | → Flat                 |
| **Revenue per Call**       | Total Revenue ÷ Total Calls                          | ₹140.75K         | ↑ 1.8% vs prior period |
| **Avg Calls per Doctor**   | Total Calls ÷ Number of Doctors                      | 16.50             | → Stable               |
| **Avg Revenue per Doctor** | Total Revenue ÷ Number of Doctors                    | ₹2.32M           | ↑ 3.2%                 |
| **Revenue Growth (YoY)**   | ((Current Revenue − Previous Revenue) ÷ Previous Revenue) × 100 | —                | Used for trend indicators above |
| **Revenue per Territory**  | Total Revenue grouped by Region                       | —                | See Regional Revenue Distribution visual |
| **Total Doctors**          | COUNT(DISTINCT Doctor_ID)                             | 100               | ↑ 4 vs prior period    |

**Most important KPIs:** Total Revenue is treated as the primary outcome metric since it directly measures overall business performance; the remaining KPIs explain the factors driving it. Revenue per Call is highlighted as the key efficiency metric — it evaluates how much value each sales interaction generates rather than just counting activity. Revenue Growth is tracked to catch improving or declining trends early.

---

## ⚙️ Power BI Concepts Used

| Concept                             | Application                                                                                                           |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Data Modeling**                   | Relational model: four related tables (`sales_data`, `doctor_data`, `rep_data`, `territory_data`) connected via `Doctor_ID` and `Region` — not a star schema |
| **DAX Measures**                    | Total Revenue, Avg Revenue per Doctor, Revenue per Call, Avg Calls per Doctor, YoY % change, vs Prior Period % change — used as measures (not calculated columns) so KPIs update dynamically with filter context |
| **KPI Cards**                       | Dynamic cards showing current value + trend indicator (↑/↓/→) vs prior period                                         |
| **Slicers**                         | Region, Category, Year, Month, Specialization with tile-style display                                                 |
| **Cross-filtering**                 | Charts interact with slicers and each other for dynamic filtering                                                    |
| **Reference Lines**                 | Avg Units Sold and Avg Revenue lines on scatter plots                                                                 |
| **Conditional Formatting**          | Trend colors (green = positive, neutral = flat) on KPI cards                                                          |
| **Tooltips**                        | Hover-over details on charts                                                                                          |
| **Multi-page Report**               | Three distinct pages: Overview, Sales Analysis, Doctor Insights                                                       |

---

## 🐍 Python Analysis Performed

### Exploratory Data Analysis (EDA)

- Revenue distribution histograms (full range and filtered <100K)
- Top 10 doctors by total revenue (`groupby + sort_values`)
- Revenue by region (bar chart)
- Top 10 medicines by revenue (bar chart)
- Revenue by specialization
- Revenue by region + specialization (cross-tabulation)
- Revenue by product category
- Average revenue by region

### Sales Representative Analysis

- Rep performance by total revenue
- Rep efficiency metrics: `Revenue_per_Call` and `Revenue_per_Meeting` computed via `.agg()`

### SQL Analysis (SQLite)

All four tables were loaded into SQLite and the following SQL queries were executed:

| Query Type                      | Description                                        |
| ---------------------------------- | ----------------------------------------------------- |
| Basic JOIN                      | Revenue by doctor + specialization using LEFT JOIN |
| `RANK()` Window Function        | Rank doctors by total revenue                      |
| `ROW_NUMBER()`                  | Sequential row numbering by revenue                |
| `CTE` (Common Table Expression) | Top 10 doctors via WITH clause                     |
| `CTE + RANK()`                  | Doctor revenue ranking using CTE                   |
| `NTILE(10)`                     | Segment doctors into revenue-based deciles         |
| `RANK() PARTITION BY`           | Top 3 doctors per region using partitioned ranking |
| Subquery                        | % revenue contribution per region vs total         |
| Subquery with CTE               | Doctors with revenue above average                 |

`RANK()` was used (rather than `DENSE_RANK()`) so doctors with tied revenue share the same rank while the next rank is skipped, reflecting that a position was shared.

---

## 💡 Key Business Insights

1. **Revenue Peak and Drop:** Annual revenue peaked in **2021** before declining sharply by approximately **38.5%** in 2022, then stabilizing through 2023–2025 with a recovery trend.

2. **Top Performing Doctors:** Doctors **D007** and **D052** consistently generate the highest revenue (₹3.1M+ each), far above the average of ₹2.32M per doctor.

3. **Regional Performance:** **South America, Middle East, and Africa** are the top three revenue-generating regions, each contributing approximately ₹30–31M.

4. **Specialization Insights:** **Orthopedic and Cardiologist** specializations deliver the highest average revenue per doctor, followed by Neurologist and General Physician.

5. **Category Contribution:** Revenue is broadly distributed across categories — **Antipyretic (₹49M)** leads slightly, followed by **Chronic (₹48M)** and **Cough_Cold (₹46M)**.

6. **Revenue–Units Correlation:** A strong positive correlation exists between units sold per doctor and revenue generated — doctors exceeding 55K units/year consistently achieve above-average revenue (₹2M+).

7. **Revenue per Call:** Top doctors maintain revenue efficiency of **₹1,842–₹1,885 per call**, significantly above average, indicating high conversion quality.

8. **Sales Rep Efficiency:** Rep **R003** leads with 150 calls, followed by R006 with 140. A higher call count reflects activity level, not necessarily performance — rep efficiency (revenue per call/meeting) varies meaningfully, suggesting unequal territory opportunity.

9. **Monthly Seasonality:** Revenue declines mid-year (July–September trough) before recovering toward year-end, with an annual monthly average of ₹19.4M.

10. **Category Mix Consistency:** The revenue mix across all 8 regions is remarkably similar — category contribution patterns remain consistent globally, suggesting uniform product demand.

---

## 📑 Dashboard Pages Explanation

### Page 1 — Overview

**Purpose:** High-level executive summary of overall business performance.

**Slicers:** Region, Category, Year

**Visuals:**

- 4 KPI Cards: Total Revenue (₹232.24M, ↑5.1% YoY), Units Sold (4M, ↑2.3%), Total Calls (2K, Flat), Revenue per Call (₹140.75K, ↑1.8%)
- **Regional Revenue Distribution** — horizontal bar chart showing South America, Middle East, and Africa as top contributors (~₹30–31M each)
- **Top Performing Doctors by Revenue** — D007 and D052 at ₹3.1M each
- **Top Sales Representatives by Calls** — R003 (150 calls) and R006 (140 calls)
- **Revenue Contribution by Category** — Antipyretic leads at ₹49M
- **Annual Revenue Trend** — line chart showing peak in 2021, sharp 38.5% drop in 2022, and gradual recovery; annotated with a warning callout

---

### Page 2 — Sales Analysis

**Purpose:** Deeper product, regional, and temporal sales breakdown.

**Slicers:** Region, Category, Month

**Visuals:**

- 4 KPI Cards (same overall metrics as Overview page)
- **Revenue Contribution by Category** — bar chart showing near-equal distribution across Antipyretic, Chronic, Cough_Cold, Vitamin, and Antibiotic
- **Regional Revenue Distribution** — South America leads
- **Revenue Mix Across Regions and Categories** — stacked horizontal bar chart showing proportional category split per region
- **Revenue vs Units Sold Across Regions** — scatter plot with average reference lines; each region color-coded, showing positive correlation
- **Monthly Revenue Trend** — line chart with seasonal dip mid-year and recovery toward December; annual average of ₹19.4M annotated

---

### Page 3 — Doctor Insights

**Purpose:** Doctor-level performance analysis by revenue efficiency, specialization, and call correlation.

**Slicers:** Region, Specialization

**Visuals:**

- 4 KPI Cards: Total Doctors (100, ↑4), Avg Revenue per Doctor (₹2.32M, ↑3.2%), Avg Calls per Doctor (16.50, Stable), Revenue per Call (₹140.75K, ↑1.8%)
- **Revenue Efficiency of Top Doctors** — horizontal bar chart ranking D007 (1,885), D052 (1,879), D090 (1,848), D053 (1,842) by revenue per call
- **Revenue Performance by Specialization** — bar chart showing Orthopedic and Cardiologist as the highest average revenue specializations (~₹2M)
- **Correlation Between Units Sold and Revenue** — scatter plot (30K–65K units range) with all 100 doctors plotted, color-coded by region, showing clear positive trend with average reference lines

---

## 📸 Screenshots

*(See the repository's `screenshots/` folder / dashboard images for the Overview, Sales Analysis, and Doctor Insights pages.)*

---

## 📁 Folder Structure

```
pharma-sales-analytics/
│
├── 📊 data/
│   ├── pharma_sales_dataset.xlsx       # Primary Excel data source
│   ├── sales_dataset.csv               # Sales transactions (10,000 rows)
│   ├── doctor_dataset.csv              # Doctor master data (100 rows)
│   ├── rep_dataset.csv                 # Sales rep data (15 rows)
│   └── territory_dataset.csv           # Territory-manager mapping (8 rows)
│
├── 📓 notebooks/
│   └── Pharma_sales_project.ipynb      # Python EDA + SQL analysis (Google Colab)
│
├── 📈 dashboard/
│   └── Pharma_Sales_Dashboard.pbix     # Power BI dashboard file (3 pages)
│
├── 🖼️ screenshots/
│   ├── OVERVIEW_PAGE__pharma_.jpeg     # Overview page screenshot
│   ├── SALES_ANALYSIS__pharma_.jpeg    # Sales analysis page screenshot
│   └── DOCTOR_INSIGHTS__pharma_.jpeg   # Doctor insights page screenshot
│
└── 📄 README.md
```

---

## ▶️ How to Run the Project

### Python Notebook

1. Upload the following CSV files to your Google Colab session (or local environment):
   - `sales_dataset.csv`
   - `doctor_dataset.csv`
   - `rep_dataset.csv`
   - `territory_dataset.csv`
2. Open `Pharma_sales_project.ipynb` in [Google Colab](https://colab.research.google.com/)
3. Run all cells sequentially (Runtime → Run All)
4. The notebook will:
   - Load and validate all datasets
   - Perform EDA with matplotlib visualizations
   - Create a SQLite database (`pharma.db`) and run advanced SQL queries

**Requirements:**

```
pandas
matplotlib
sqlite3  # built-in Python module
```

### Power BI Dashboard

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Open `Pharma_Sales_Dashboard.pbix`
3. If prompted to refresh data, point the data source to the local path of the CSV/Excel files
4. Use the slicers on each page to filter by Region, Category, Year, Month, or Specialization

---

## 🚀 Future Improvements

- [ ] **Forecasting:** Integrate Python forecasting models (Prophet or ARIMA) to project revenue for 2026–2027
- [ ] **Doctor Segmentation:** Apply K-Means clustering to segment doctors into High/Mid/Low performers based on revenue, call frequency, and units sold
- [ ] **Rep Territory Optimization:** Analyze whether regional rep call volumes correlate with regional revenue — recommend territory rebalancing
- [ ] **Product Expiry Risk Analysis:** Use `expiry_days_remaining` and `stock_level` columns (currently unused in Power BI) to build an inventory risk view
- [ ] **COVID Impact Deep Dive:** Leverage the `covid_flag` column to quantify the revenue impact of COVID-era transactions vs. non-COVID
- [ ] **Power BI Service Deployment:** Publish the report to Power BI Service with row-level security (RLS) per region manager
- [ ] **Automated Data Refresh:** Connect Power BI to a cloud database (Azure SQL / Google BigQuery) for scheduled refresh
- [ ] **Age Group Analysis:** Incorporate `age_group` segmentation to identify which demographics drive category-level demand
- [ ] **Doctor Conversion Rate:** Source or derive a prescriber-conversion field to add this KPI, which the current dataset doesn't support

---

## ✅ Conclusion

This project demonstrates a complete, production-ready analytics workflow for pharmaceutical sales data:

- **Data Engineering:** Multi-table relational modeling with Python pandas and SQLite
- **Advanced SQL:** Window functions (RANK, ROW_NUMBER, NTILE), CTEs, subqueries, and partitioned ranking
- **Business Intelligence:** A three-page Power BI dashboard with KPI cards, trend analysis, and interactive slicers
- **Actionable Insights:** Identified revenue concentration in top doctors (D007, D052), a significant 2022 revenue dip, mid-year seasonality, and consistent category mix across global regions

The end-to-end approach — from raw CSV ingestion to an executive-ready dashboard — makes this project directly applicable to real-world pharmaceutical sales analytics roles.

---

> ⭐ If you found this project useful, please consider starring the repository!

---

*Built with ❤️ using Python, SQL, and Power BI*
