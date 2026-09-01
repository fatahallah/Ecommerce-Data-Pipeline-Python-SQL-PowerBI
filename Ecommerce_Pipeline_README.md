# 🛒 End-to-End E-Commerce Data Pipeline & Analytics

An enterprise-style data pipeline built to clean raw transactional data, structure it in SQL, segment customers by behavior, and deliver insights through an interactive Power BI dashboard.

## 🛠️ Tech Stack & Architecture
- **Data Pipeline & Cleaning:** Python (Pandas, NumPy)
- **Database Management & SQL:** SQLite (Window Functions, CTEs, Subqueries)
- **Data Visualization:** Power BI (DAX, Interactive Dashboards)
- **Environment:** JupyterLab

---

## 🔄 Pipeline Workflow

### 1. Data Ingestion & Profiling (Python)
- Processed the UCI Online Retail dataset containing **541,909 raw records**.
- Identified data quality issues: duplicate transactions, missing Customer IDs, and negative quantities/prices (returns and data entry errors).

### 2. Data Cleaning & Transformation (Python)
- Removed duplicate rows and transactions missing a Customer ID.
- Excluded negative/zero quantities and prices (returns and invalid entries).
- Corrected data types (CustomerID to string, InvoiceDate to datetime) and standardized product description text.
- Created a calculated feature: `Total_Amount = Quantity * UnitPrice`.
- Result: **392,692 valid transactions** retained for analysis, covering 4,338 unique customers.

### 3. RFM Customer Segmentation (Python)
- Calculated Recency, Frequency, and Monetary value per customer using `groupby` and a snapshot date (the latest invoice date in the dataset).
- Scored each customer into quartiles (`pd.qcut`) across all three dimensions, then classified each into a behavioral segment: Champions, Loyal Customers, Needs Attention, or At Risk.
- Uploaded the segmented dataset to SQL (`customer_rfm_segments` table) and cross-validated the segment counts directly via a SQL query, confirming an exact match with the Python-side results.

### 4. SQL Analytics Layer
- Structured cleaned data into a SQLite database (`ecommerce_db.db`, table `sales_data`).
- Queried top-spending customers and computed average spend per RFM segment directly from SQL.
- Ranked top 3 products by revenue within each of the top 3 countries using `DENSE_RANK()` with a CTE.

### 5. Power BI Dashboard
- Built dynamic KPI cards using DAX (`Total Revenue`, `Total Orders`, `Total Customers`).
- Designed visuals tracking revenue trends over time, top countries by order volume, and top products by revenue.

---

## 📈 Key Business Insights
- **Total Revenue:** $8.89M generated across 18.53K orders from 4,338 customers.
- **RFM segmentation reveals a 14.7x spending gap:** "Champions" customers spend an average of $7,501.84, compared to just $508.82 for customers classified as "At Risk" — a clear, quantified target for retention efforts.
- **Geographic concentration:** the United Kingdom accounts for the large majority of total sales volume.
- **Seasonal peak:** sales peaked sharply in Q4 (November 2011), reaching $1.16M in a single month.

## 📁 Files
- `01_ECommerce_Data_Cleaning_Pipeline.ipynb` — data ingestion, profiling, and cleaning.
- `02_RFM_Customer_Segmentation.ipynb` — RFM calculation, scoring, segmentation, and SQL upload.
- Power BI dashboard export (see repository).

## 📊 Data Source
UCI Machine Learning Repository — Online Retail Dataset (raw data not included in this repository due to size; see cleaning notebook for the direct download source).
