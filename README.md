# Openborders-Dashboards

Hi Everyone, I am glad to be associated as data science and analytics internships at OpenBorders LLC. I will be sharing here some of the dashboards I created for Openborders:
# 1. 📊 Net Dollar Retention (NDR) Analysis by Brand using Sigma Dashboard:
To gain insights into how different brands perform over time in terms of revenue retention and growth, I utilized Sigma Computing to build a comprehensive, interactive dashboard centered around Net Dollar Retention (NDR). Sigma's flexibility with custom SQL, top-down filtering, and date-driven analysis allowed me to explore brand performance trends across various time intervals (daily, monthly, quarterly, yearly).

🔍 Dashboard Filters and Controls
The top section of the dashboard includes dynamic filters:

Brand – Allows multi-select to filter for one or more brands.

Date – Enables selection of specific time ranges.

Country Code – Filters based on shipping or customer country.

Aggregation – Lets users choose how the periods are grouped (Quarters, Months, etc.).

Billable – A boolean filter to only include billable transactions when set to True.

These controls enable the user to slice the data at any desired granularity and dimension.

📈 Table 1: %NDR by Brands and Day of Period
This table is designed to analyze Net Dollar Retention (NDR) for each brand across selected time intervals. Key metrics shown:

Gmv (Gross Merchandise Value) – Revenue generated in the current period.

Ly Gmv (Last Year GMV) – Revenue for the same period in the previous year.

Ndr (%) – Computed as (Gmv - Ly Gmv) / Ly Gmv * 100, indicating retention or expansion.

Each brand's performance is shown per date period (e.g., 2024-10-01, 2025-01-01), including a Grand Total summarizing performance across the entire selected range. Conditional formatting visually emphasizes performance spikes or dips.

📊 Table 2: Estimation of GMV and LY GMV by Brand and Period
This second table offers a clean snapshot of GMV and LY GMV over each period for each brand. Unlike the first table, this one does not include NDR directly, making it ideal for tracking raw revenue trends over time.

Period – Date (based on aggregation control).

Gmv – Total current revenue for the brand.

Ly Gmv – Revenue from the same period one year prior.

This table is particularly useful for validating the trends observed in the NDR table and offers additional support when diagnosing why NDR percentages rise or fall.
![Screenshot 2025-06-19 195815](https://github.com/user-attachments/assets/38ab81cc-22a4-4f5b-984e-f71b7f4af553)
![Screenshot 2025-06-19 195743](https://github.com/user-attachments/assets/1198dd49-b156-465f-bd9d-5fe10302db99)

# 2. Client's Revenue ETD Overview
📄 Query Explanation

This SQL query pulls net revenue and tax details by country from the table ob_analytics.production.net_revenue_report.

🔎 Steps Performed

Filtering Data

🏷️ By BRAND ({{Brand}})

🌍 By COUNTRY_CODE ({{Country-Code}})

✅ By BILLABLE flag ({{Billable}})

📅 By REVENUE_REPORT_DATE between given start and end dates (defaults: 2020-01-01 → current date)

Calculations (per country)

💵 TRANSACTION_AMOUNT_USD → Total transaction amount

🧾 INVOICED_DUTIES_AND_TAXES_USD → Duties & taxes invoiced

📊 TOTAL_DUTIES_AND_TAXES_USD → Duties & taxes charged

⚖️ EXCESS_TAXES_AND_DUTIES → Difference between charged vs invoiced taxes

👉 COALESCE is used so that missing values are replaced with 0.

Grouping & Sorting

📌 Groups results by COUNTRY_CODE

🔠 Orders results alphabetically by COUNTRY_CODE

🎯 Purpose

This query helps identify over- or under-collected taxes and duties per country.
It is useful for finance, compliance, and operations teams to reconcile revenue and tax reports across brands and regions.

<img width="1325" height="553" alt="image" src="https://github.com/user-attachments/assets/14c776a9-74d7-4d6e-8080-2227443902c1" />

