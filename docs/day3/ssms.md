# Activity: Query Your Data in SSMS

!!! abstract "S9: Query and manipulate data using tools and programming such as SQL and Python. Manage database access, and implement automated validation checks."

## Load to SQL Server

```powershell
python load_to_sql.py
```

Open SSMS and connect to `localhost`. Open the `library_warehouse` database. Try these queries.

**Transactions per branch**

```sql
SELECT branch_id, COUNT(*) AS transactions
FROM circulation_clean
GROUP BY branch_id
ORDER BY transactions DESC;
```

**Genres with the most titles**

```sql
SELECT Genre, COUNT(*) AS titles
FROM catalogue_clean
GROUP BY Genre
ORDER BY titles DESC;
```

**Invalid ISBNs**

```sql
SELECT ISBN, Title, Author
FROM catalogue_clean
WHERE ISBN_Clean IS NULL;
```

**Average event feedback score by branch**

```sql
SELECT branch, ROUND(AVG(CAST(feedback_score AS FLOAT)), 2) AS avg_score
FROM events_clean
GROUP BY branch
ORDER BY avg_score DESC;
```

**Member feedback ratings by branch**

```sql
SELECT branch, rating, count
FROM feedback_summary
ORDER BY branch, rating;
```

**Join circulation to catalogue**

```sql
SELECT c.transaction_id, c.checkout_date, cat.Title, cat.Author
FROM circulation_clean c
JOIN catalogue_clean cat
  ON c.ISBN_Clean = cat.ISBN_Clean;
```

This joins on `ISBN_Clean`, not the raw `isbn`/`ISBN` columns - the raw values have inconsistent formatting (some catalogue ISBNs lose their hyphens and become plain numbers), so the join only works cleanly once `validate_isbn()` normalises both sides.

Write your own query - find something interesting in the data.
