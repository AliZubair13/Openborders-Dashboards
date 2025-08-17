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


#3. Data Differential - I
📊 Query Explanation

This SQL script compares data across different environments (LSALUNGA, PXUE, LVIERNES) against the Production environment for three different reports:

📦 Orders Core

💰 Net Revenue

🧾 Billing Report

It shows both the values and the differences between each environment and Production.

🔎 Step-by-Step

📦 Orders Core (orders_core CTE)

Pulls order-related metrics (total orders, charged amount, duties & taxes, shipping, platform costs) from each environment’s ORDERS_CORE table.

Cross joins with the same metrics from Production.

Calculates differences (e.g., "Diff Total Orders", "Diff Charged USD").

💰 Net Revenue (net_revenue CTE)

Pulls revenue-related metrics (transaction amount, duties & taxes, shipping fee, platform fee) from each environment’s NET_REVENUE_REPORT table.

Cross joins with Production NET_REVENUE_REPORT.

Calculates differences (e.g., "Diff Transaction Amount USD", "Diff Duties & Taxes USD").

🧾 Billing Report (billing_report CTE)

Pulls billing-related metrics (transaction amount, customer duties & taxes, customer shipping cost, platform fee) from each environment’s BILLING_REPORT table.

Cross joins with Production BILLING_REPORT.

Calculates differences (e.g., "Diff Customer Duties & Taxes USD", "Diff Customer Shipping USD").

🎛️ Control Switch

At the end, the query uses a control parameter {{New-Control-1}} to decide which dataset to return:

If {{New-Control-1}} = 'Orders Core' → returns 📦 Orders Core results.

If {{New-Control-1}} = 'Net Revenue' → returns 💰 Net Revenue results.

If {{New-Control-1}} = 'Billing Report' → returns 🧾 Billing Report results.

📌 Output

Environment (LSALUNGA, PXUE, LVIERNES) vs. Production

Each metric (orders, revenue, billing fields)

The difference between environment values and Production values

Results sorted by environment

🎯 Purpose

This query is designed to:

✅ Validate that staging/test environments match Production data.

📉 Highlight differences across orders, revenue, and billing.

🔍 Help analysts quickly identify inconsistencies between environments.


# 4. Data Differential - II
🔍 Query Explanation – One Table Discrepancy Report

This SQL script compares Production vs Staging records for three reports:

📦 Orders Core

💰 Net Revenue

🧾 Billing Report

The goal is to detect mismatches or missing records between environments and output them in one unified table with clear labels.

🏗️ How It Works

📊 Production Data (CTE: production_data)

Pulls selected columns (col1 – col4) from Production tables depending on the report type (Net Revenue, Orders Core, or Billing Report).

🧪 Staging Data (CTE: staging_data)

Pulls the same set of columns (col1 – col4) from Staging schemas (PXUE, LSALUNGA, LVIERNES) based on who owns the pipeline (Paul, Leslie, Lea) and the report type.

🏷️ Labels (CTE: labels)

Assigns pretty column names to col1 – col4 depending on which report type is selected (so the output looks readable).

⚖️ Row-Based Comparison (mismatch checks)

mismatch_col1 … mismatch_col4: Compares Production value vs. Staging value column by column.

Flags if values are:

❌ Different

⚠️ Missing in one environment

🚨 Missing Records

missing_in_staging: Row exists in Production but not in Staging.

missing_in_production: Row exists in Staging but not in Production.

📋 Final Output
A single table with the following columns:

🏷️ Discrepancy Type (which field mismatched or if missing)

🔑 Oms Order Number

🏭 Prod Value (Production side)

🧪 Staging Value (Staging side)

📝 Notes (explanation of mismatch/missing record)



