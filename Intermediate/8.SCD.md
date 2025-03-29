# Slow Changing Dimensions (SCD) in SQL

**Slow Changing Dimensions (SCD)** are used in data warehousing to manage **historical changes** in dimension tables. These changes occur **slowly** over time and need to be tracked efficiently.

There are different types of SCD, but the most common ones are:
- **SCD Type 1:** Overwrites old data (no history).
- **SCD Type 2:** Maintains history by adding new rows.
- **SCD Type 3:** Keeps limited history by adding new columns.

---

### SCD Type 1: Overwrite Data (No History)

✅ **Used when historical data is NOT needed.**

```sql
UPDATE Customers
SET Address = 'New Address'
WHERE CustomerID = 101;
```
👉 The old address is lost since the update overwrites the previous value.

---

### SCD Type 2: Maintain History (New Rows)

✅ **Used when historical tracking is required.**

### Steps:
1. Insert a new record with a new **Surrogate Key**.
2. Mark the old record as **inactive**.
3. Maintain **Start Date & End Date** for version tracking.

```sql
-- Step 1: Mark old record as inactive
UPDATE Employees
SET EndDate = GETDATE(), IsActive = 0
WHERE EmployeeID = 101 AND IsActive = 1;

-- Step 2: Insert new record with updated values
INSERT INTO Employees (EmployeeID, Name, JobTitle, StartDate, EndDate, IsActive)
VALUES (101, 'John Doe', 'Senior Analyst', GETDATE(), NULL, 1);
```
👉 The **old record is kept** with an end date, and a **new record is inserted** for tracking history.

---

### SCD Type 3: Limited History (New Columns)

✅ **Used when only recent history needs to be tracked.**

Storing Previous Job Title
```sql
ALTER TABLE Employees ADD PreviousJobTitle VARCHAR(100);

UPDATE Employees
SET PreviousJobTitle = JobTitle, JobTitle = 'Senior Analyst'
WHERE EmployeeID = 101;
```
👉 Only one level of history is stored in the `PreviousJobTitle` column.

---

### When to Use Each SCD Type?
| SCD Type | Use Case |
|----------|---------|
| **Type 1** | When history is NOT required (e.g., customer address updates). |
| **Type 2** | When full historical tracking is needed (e.g., employee promotions). |
| **Type 3** | When only recent changes are needed (e.g., tracking previous department). |

---

## Conclusion
Understanding **SCD techniques** helps maintain data integrity in a data warehouse. Choosing the right approach depends on how much historical data needs to be preserved. 🚀
