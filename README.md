```markdown
# 📊 Customer Shopping Behavior Analysis (End-to-End Data Analytics)

An end-to-end data analytics project focused on optimizing customer retention, subscription conversion, and targeted marketing strategy for a retail dataset of 3,900 unique customer transactions.

This project demonstrates a production-grade data pipeline workflow: moving from raw transactional data to relational staging, validation, feature engineering with **MySQL Workbench**, and delivering interactive, business-critical insights via **Power BI**.

---

## 🛠️ Tech Stack & Skills Demonstrated
* **Database Management & Processing:** MySQL Workbench
* **Business Intelligence & Visualization:** Power BI Desktop
* **Advanced SQL Concepts:** Common Table Expressions (CTEs), Window Functions, Schema Alteration, Conditional Feature Engineering (`CASE WHEN`), Data Auditing.
* **Core Analytics Skills:** Data Cleaning, Structural Integrity Validation, Customer Lifecycle Segmentation, Behavioral Cohort Analysis, Descriptive Metrics, Business Strategy Formulation.

---

## 🚀 Interactive Business Dashboard

*Below is a preview of the interactive customer behavior analytics dashboard built in Power BI:*

![Power BI Dashboard Preview]([LINK_TO_YOUR_DASHBOARD_SCREENSHOT_HERE])

### Key Dashboard High-Level Metrics:
* **Total Customer Volume:** 3.86K unique records
* **Average Ticket Size:** \$59.70 per transaction
* **Global Review Average:** 3.75 Stars
* **Subscription Split:** 27% Subscribed vs. 73% Non-Subscribed

---

## 📑 Data Pipeline Workflow

### 1. Staging, Profiling & Structural Repair
To maintain data engineering best practices and keep the production source untainted, a dedicated data analytics staging table was mirrored and populated:
```sql
CREATE TABLE csb LIKE customer_shopping_behavior;
INSERT INTO csb SELECT * FROM customer_shopping_behavior;

```

* **Encoding / Corruption Repair:** Fixed standard character set ingestion corruptions by cleanly renaming the primary identifier column:

```sql
ALTER TABLE csb RENAME COLUMN `ï»¿Customer ID` TO `Customer_id`;

```

### 2. Rigorous Data Quality & Integrity Auditing

* **Duplicate Validation:** Implemented a Common Table Expression (CTE) combined with a window partitioning function (`ROW_NUMBER`) over the unique identifier sequence to guarantee data uniqueness.

```sql
WITH cte AS (
    SELECT *, ROW_NUMBER() OVER(PARTITION BY Customer_id ORDER BY Customer_id) AS row_num
    FROM csb
)
SELECT * FROM cte WHERE row_num > 1; -- Result: Zero duplicate records found.

```

* **Exhaustive Null & Space Auditing:** Written explicit conditional strings matching both standard relational `IS NULL` markers and text-trimming validations (`TRIM(column) = ''`) across all 18 transactional metrics to ensure zero structural blanks or empty input spaces.

### 3. Feature Engineering & Schema Optimization

* **Demographic Binner (Age Stratification):** Generated a new `age_group` attribute dynamically via deterministic condition boundaries:

```sql
ALTER TABLE csb ADD COLUMN age_group VARCHAR(100);

UPDATE csb SET age_group = CASE
    WHEN Age < 30 THEN 'young_adult'
    WHEN Age BETWEEN 30 AND 44 THEN 'adult'
    WHEN Age BETWEEN 45 AND 64 THEN 'middle_age'
    ELSE 'senior'
END;

```

* **Temporal Frequency Encoder:** Standardized highly qualitative delivery attributes into numeric time-step day variables (`frequency_num`) using conditional mapping logic (e.g., Weekly mapped to 7 days, Fortnightly/Bi-Weekly mapped to 14 days) to better quantify baseline transaction patterns.
* **Redundancy Cleanup:** Optimized database storage efficiency and layout clarity for reporting integration by safely dropping the redundant `Promo Code Used` column since it heavily overlapped with active `Discount Applied` tracking data.

---

## 📈 Key Business Insights Extracted

Through relational analysis queries, the following metrics were surfaced to direct immediate marketing initiatives:

1. **Demographic Revenue Distribution:** Male shoppers drive the vast majority of platform sales, accounting for **$157,890** in aggregate volume compared to **$75,191** from female buyers.
2. **Subscription Unit Economics:** While non-subscribers brought in the bulk of overall revenue (**$170,436**) due to sheer volume, both subscribed and non-subscribed cohorts maintain an identical transaction average of **~$59 per basket**. This indicates that user volume and acquisition conversion (not spending thresholds) are our main revenue growth levers.
3. **Discount Co-dependencies:** Flagged that products like **Hats (50.0%)** and **Sneakers (49.66%)** rely heavily on promotional pricing adjustments, marking critical areas to implement margin guardrails.
4. **Lifecycle Segments:** Evaluated and classified historical platform shoppers into distinct operational tiers:
* **Loyal Tiers:** 3,116 Profiles
* **Returning Tiers:** 701 Profiles
* **New Tiers:** 83 Profiles



---

## 💡 Strategic Business Recommendations

Based on the empirical insights uncovered through SQL query analysis and visual Power BI patterns, I formulated the following four operational recommendations for stakeholders:

* **Targeted Subscription Upselling:** Deploy hyper-focused incentive tracks aimed precisely at the 73% non-subscriber base. Since average spend sizes are equal, converting these users to premium tiers will predictably stabilize monthly revenue.
* **Customer Lifecycle Retention:** Construct milestone reward pipelines for the "Returning" tier (701 accounts) to secure them and convert them permanently into high-value "Loyal" segments.
* **Promo Policy Safeguards:** Readjust the pricing structure and limit automatic markdown policies on high-velocity items like hats and sneakers to avoid unnecessary margin dilution.
* **High-Value Cohort Allocation:** Reallocate standard digital marketing budgets toward the highest-revenue demographic clusters—specifically focusing on young adults and middle-aged buyers using express shipping channels.

---

## 📂 Repository Structure

```text
├── Data/                       # Raw and engineered dataset schemas 
├── SQL_Scripts/                # Cleaning, validation, and analytics query files
├── Dashboards/                 # Power BI template files (.pbix)
└── README.md                   # Project summary and insights report

```

---

💡 **Looking for a Data Analyst who can turn raw, complex database files into actionable business profits? Let's connect!**

```

```
