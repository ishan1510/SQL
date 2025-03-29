# SQL UNION and UNION ALL

`UNION` and `UNION ALL` are **set operations** used in SQL to combine the results of two queries.

- `UNION` removes duplicates and returns unique records.
- `UNION ALL` includes all records, including duplicates.

Both operations require the queries to have the **same number of columns with compatible data types**.

---

### UNION: Combine and Remove Duplicates
The `UNION` operator merges two datasets and removes duplicate rows.

Example : List all employees from **Sales** and **Marketing**, removing duplicates.

```sql
SELECT EmployeeID, Name FROM SalesEmployees
UNION
SELECT EmployeeID, Name FROM MarketingEmployees;
```

👉 This returns unique employees from both tables.

---

### UNION ALL: Combine and Keep Duplicates
The `UNION ALL` operator merges two datasets **without** removing duplicates.

Example : List all employees from **Sales** and **Marketing**, keeping duplicates.

```sql
SELECT EmployeeID, Name FROM SalesEmployees
UNION ALL
SELECT EmployeeID, Name FROM MarketingEmployees;
```

👉 This returns all employees, even if they exist in both tables.

---

### Key Differences Between UNION and UNION ALL
| Feature          | UNION  | UNION ALL |
|----------------|--------|------------|
| Removes Duplicates | ✅ Yes | ❌ No |
| Performance | 🚀 Slower (due to deduplication) | ⚡ Faster (no deduplication) |
| Use Case | When you need unique records | When you need all records |

---

### When to Use UNION and UNION ALL?
- Use `UNION` when you want **unique** records.
- Use `UNION ALL` when you need **all records, including duplicates**.
- `UNION` is useful when merging datasets **without duplicates**.
- `UNION ALL` is useful for **performance optimization** when duplicate removal is unnecessary.

---

### Conclusion
`UNION` and `UNION ALL` help merge datasets efficiently. Choosing the right one depends on whether you need **deduplication** (`UNION`) or **performance** (`UNION ALL`). 🚀
