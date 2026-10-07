# Activity: Build a Report in Power BI

You have just queried your cleaned data in SSMS. Now put the same data into a report.

Watch the demo first, then repeat these steps on your own VM. You only need one visual - this is about seeing your pipeline's output in a reporting tool, not about learning Power BI.

## Step 1: Connect to SQL Server

1. Open **Power BI Desktop** on the VM.

2. On the **Home** ribbon, select **Get data** and choose **SQL Server**.

3. Enter the connection details:

    - **Server:** `localhost`
    - **Database:** `library_warehouse`
    - **Data Connectivity mode:** Import

4. Select **OK**.

5. If you are asked for credentials, stay on the **Windows** tab, keep **Use my current credentials** selected, and select **Connect**.

6. If you see a warning that the connection is not encrypted, select **OK** to continue.

## Step 2: Load the data

1. In the **Navigator** window, tick `circulation_clean`.

2. Select **Load**.

    !!! success "`circulation_clean` should now appear in the **Data** pane on the right, with its columns listed underneath."

## Step 3: Build the visual

1. In the **Visualizations** pane, select the **Clustered column chart**.

2. From the **Data** pane, drag:

    - `branch_id` to the **X-axis**
    - `transaction_id` to the **Y-axis**

3. Check the Y-axis field says **Count of transaction_id**. If it does not, select the small arrow next to it and choose **Count**.

    !!! success "You should see one column per branch, showing the number of transactions."

## Step 4: Compare it with SSMS

Go back to the first query you ran in SSMS:

```sql
SELECT branch_id, COUNT(*) AS transactions
FROM circulation_clean
GROUP BY branch_id
ORDER BY transactions DESC;
```

Hover over a column in your chart. Does the count match the query result for that branch?

## Discussion

- The report and the SQL query give the same answer. What had to be true about the data for that to happen?
- This afternoon you will build the same kind of report in Microsoft Fabric. What do you expect to be the same, and what will be different?
