# Canadian Public Service - Workforce Analytics & Staffing Trends

<img width="769" height="431" alt="01_Titlepage" src="https://github.com/user-attachments/assets/d09919b7-bbb6-41fd-bf92-b5ce489cbfc6" />

A comprehensive Business Intelligence dashboard analyzing historical workforce entry and exit patterns within the Canadian Federal Public Service across 16 fiscal years. 

This project demonstrates an end-to-end analytics workflow: ingesting open government data, building an optimized Star Schema model, performing data cleansing and ETL transformations, and delivering executive insights on staffing stability, recruitment trends, and departure risks.

---

## Data Source & Governance

* **Data Source:** [Open Government Portal - Government of Canada](https://open.canada.ca/data/en/dataset/26ffad36-ca9b-431c-8f6d-a6df02665e2c)
* **Datasets Used:** Historical **Inflow** (Appointments) and **Outflow** (Departures) dataset files.
* **Data Scope & Integrity:** Covers micro-level staffing transaction records across ~94 federal departments over the past 16 fiscal years. All data is anonymized, aggregated public administrative data.

---

## Data Architecture & ETL Methodology

[ Inflow & Outflow CSVs ] ➡️ [ Power Query ETL ] ➡️ [ Star Schema Data Model ] ➡️ [ Power BI Dashboard ]

### 1. Extraction & Consolidation
* Ingested raw **Inflow** and **Outflow** CSV files.
* Applied **Append Queries** in Power Query to consolidate entry and exit records into a unified data pipeline without data loss.

### 2. Cleansing & Transformation Rules
* **Double-Counting Prevention:** Filtered out pre-calculated subtotals, group averages, and aggregate summary rows from the raw files to ensure calculations reflect true raw transactional counts.
* **Null Handling:** Standardized missing and null numeric values by imputing `0` to ensure calculation stability and scalability for future reporting scope expansion.
* **Deduplication:** Removed redundant records across source files to establish clean primary keys across dimension entities.

### 3. Data Modeling (Star Schema)
Engineered a dimensional Star Schema with dedicated Dimension Tables linked via Index Primary/Foreign Key (FK) relationships to the Fact Table:

<img width="589" height="122" alt="image" src="https://github.com/user-attachments/assets/82b03ffe-2ee3-437c-8c1e-4f66a022f454" />

* **`fact_workforceMovements`:** Core transactional fact table storing movement volume metrics.
* **`dim_category`:** Classifies `movement_type` (Appointments vs. Departures) and specific `category_name` groupings:
  * **Appointments:** Focus on *New Term* (temporary/contract) vs. *New Indeterminate* (permanent) hires, and *Term-to-Indeterminate* conversion trends over time.
  * **Departures:** Exit reasons including *Retirements*, *Resignations*, *Promotions*, and *Lateral Movements*.
* **`dim_department`:** Stores ~94 federal departments (top 10 prioritized in dashboard visuals to avoid visual clutter and enhance executive readability).
* **`dim_calendar`:** Covers 16 fiscal years for historical time-series analytics.
---

## Dashboard Preview & Analytical Views

### 1. Staffing Overview
<img width="770" height="434" alt="02_Overview" src="https://github.com/user-attachments/assets/c61ffcbe-4cd2-4aee-ac0d-bfca7b7900a6" />

*Macro view of total recorded movements, appointments, departures, and net movement balance across fiscal years.*

### 2. Appointments & Recruitment Patterns
<img width="769" height="434" alt="03_Appointments" src="https://github.com/user-attachments/assets/2882c0f5-737e-4c49-a02e-ddab7dc8c387" />

*Deep dive into temporary (Term) vs. permanent (Indeterminate) appointments and term-to-indeterminate conversion efficiency.*

### 3. Departures & Risk Analysis
<img width="771" height="432" alt="04_Departures" src="https://github.com/user-attachments/assets/6b2ef3cc-f26c-4490-896e-d52e1025c60a" />

*Detailed breakdown of departure causes, isolating retirement spikes and lateral movements across top departments.*

---

## Key Business Insights
<img width="769" height="433" alt="05_Summary" src="https://github.com/user-attachments/assets/7322de7a-4924-4c51-ba1b-23064d1da34b" />

*Executive takeaways, business implications across staffing patterns, and dataset scope notes.*

* **Staffing Trends:** Recorded appointments consistently exceeded departures, though activity levels varied significantly across fiscal years.
* *Implication:* Helps HR leadership monitor overall transactional volume to optimize workforce planning and resource allocation.
* **Appointment Patterns:** New Indeterminate (permanent) appointments grew in frequency over time, though distribution varies by entity. *Implication:* Requires evaluating department-level hiring models to support long-term workforce stability.
* **Departmental Conversions:** Term to Indeterminate conversions are highly concentrated in specific departments.
* *Implication:* Provides visibility into how effectively temporary positions serve as talent pipelines and retention drivers.
* **Departure Drivers:** Retirement and Lateral/Downward movements account for over 50% of exits in key departments analyzed.
* *Implication:* Highlights critical risk areas to prepare for knowledge transfer, replacement costs, and potential staffing gaps.

---

## Tech Stack

* **Data Cleansing & Transformation (ETL):** Power Query (M Language)
* **Data Modeling:** Dimensional Modeling (Star Schema design, PK/FK indexing & relationships, append logic)
* **Analytics & Visualization:** Power BI, DAX
