# SQL INTERSECT and EXCEPT

### Introduction
`INTERSECT` and `EXCEPT` are **set operations** used in SQL to compare the results of two queries.

- `INTERSECT` returns **only the common records** from both queries.
- `EXCEPT` returns records **present in the first query but not in the second**.

Both operations require the queries to have the **same number of columns with compatible data types**.

---

### INTERSECT: Find Common Records
The `INTERSECT` operator returns **only the rows that exist in both result sets**.

Example : Find employees who are **both** in the Sales and Marketing departments.

```sql
SELECT EmployeeID, Name FROM SalesEmployees
INTERSECT
SELECT EmployeeID, Name FROM MarketingEmployees;
```

👉 This returns employees who appear in **both** tables.

---

### EXCEPT: Find Unique Records
The `EXCEPT` operator returns **only the rows from the first query that do not appear in the second**.

Example : Find employees who are in Sales but **not** in Marketing.

```sql
SELECT EmployeeID, Name FROM SalesEmployees
EXCEPT
SELECT EmployeeID, Name FROM MarketingEmployees;
```

👉 This returns employees **only in Sales**, excluding those in both departments.

---

### LEFT ANTI JOIN (Alternative to EXCEPT)
Some databases don’t support `EXCEPT`. You can achieve the same result using a **LEFT JOIN with NULL filtering**.

```sql
SELECT s.EmployeeID, s.Name 
FROM SalesEmployees s
LEFT JOIN MarketingEmployees m
ON s.EmployeeID = m.EmployeeID
WHERE m.EmployeeID IS NULL;
```

👉 This finds employees in Sales who **don’t** exist in Marketing.


---

## Conclusion
`INTERSECT` and `EXCEPT` are powerful SQL operators for comparing datasets. Understanding these operations helps in data filtering, deduplication, and advanced querying. 🚀
