# Performance Optimization & Indexing in SQL

## 1. Indexing for Performance
Indexes improve **query speed** by reducing the number of rows scanned.

### Types of Indexes:
- **Clustered Index**: Sorts and stores data physically (one per table).
- **Non-Clustered Index**: Stores a reference to actual data (multiple per table).
- **Unique Index**: Ensures values in a column are unique.
- **Filtered Index**: Indexes only a subset of data based on a filter condition.
- **Full-Text Index**: Optimized for searching text-based columns.

### Creating an Index:
```sql
CREATE INDEX IX_Employees_LastName
ON Employees (LastName);
```

👉 **Best Practices:**
- Use **indexes on columns in WHERE, JOIN, ORDER BY, and GROUP BY**.
- Avoid **over-indexing**, as it increases storage and slows down INSERT/UPDATE/DELETE.
- Use **filtered indexes** when querying a subset of data frequently.

---

## 2. Query Optimization Techniques
### 1. Use `SELECT` Efficiently
✅ **Avoid `SELECT *`**, instead specify required columns.
```sql
SELECT FirstName, LastName FROM Employees;
```

### 2. Use `EXISTS` Instead of `IN`
✅ `EXISTS` stops checking after finding the first match, whereas `IN` scans all rows.
```sql
SELECT * FROM Employees e
WHERE EXISTS (
    SELECT 1 FROM Departments d WHERE e.DepartmentID = d.DepartmentID
);
```

### 3. Use Proper Data Types
✅ Choose the **smallest appropriate data type** to reduce storage and improve speed.

### 4. Avoid Functions on Indexed Columns
❌ **Bad Example:**
```sql
SELECT * FROM Employees WHERE LOWER(LastName) = 'smith';
```
✅ **Good Example:**
```sql
SELECT * FROM Employees WHERE LastName = 'Smith';
```
👉 Functions prevent index usage!

### 5. Avoid Wildcards at the Start of `LIKE`
❌ **Bad Example:**
```sql
SELECT * FROM Employees WHERE LastName LIKE '%son';
```
✅ **Good Example:**
```sql
SELECT * FROM Employees WHERE LastName LIKE 'John%';
```

### 6. Use `JOIN` Instead of Subqueries
✅ Joins are often **faster** than correlated subqueries.
```sql
SELECT e.EmployeeID, e.EmployeeName, d.DepartmentName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```
### 7. Optimize Temp Tables & CTEs
✅ Avoid excessive use of **temporary tables** if **CTEs** can be used instead.
```sql
WITH EmployeeCTE AS (
    SELECT EmployeeID, EmployeeName FROM Employees
)
SELECT * FROM EmployeeCTE;
```

---

## Conclusion
Optimizing SQL performance involves **proper indexing, efficient queries, and regular maintenance**. Applying these techniques ensures fast, scalable, and efficient database queries. 🚀
