# Activity: Create Gold Layer Table

## Medallion Architecture

- Bronze is raw data exactly as it arrived.
- Silver is cleaned, structured and model-ready.
- Gold answers a business question.

For example:

- How many books are being borrowed per branch per month?
- Which members are high-value?

!!! info "Gold tables are usually aggregated, business focussed, and ready to report on."

---

## Step 1: Create a Gold table using SQL

In the SQL endpoint (or notebook SQL cell):

```sql
CREATE OR REPLACE TABLE gold_circulation_summary AS
SELECT
    branch_id,
    DATE_TRUNC('month', checkout_date) AS month,
    COUNT(*) AS total_loans,
    COUNT(DISTINCT member_id) AS unique_members
FROM silver_circulation
GROUP BY branch_id, DATE_TRUNC('month', checkout_date)
ORDER BY month, branch_id;
```

!!! note "This gives a business summary per branch per month."

## Step 2: Visualise the results

- Right-click the `gold_circulation_summary` table
- ClicK: `New report`

Drag:

- `month` to X axis
- `total_loans` as a line or bar chart

!!! success "You have created a simple Gold layer table with a visualisation"
