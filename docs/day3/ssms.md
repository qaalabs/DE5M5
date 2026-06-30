# Activity: Query Your Data in SSMS

Connect to `localhost` in SSMS and open the `library_warehouse` database. Try these queries.

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
WHERE ISBN_valid = 0;
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

Write your own query - find something interesting in the data.
