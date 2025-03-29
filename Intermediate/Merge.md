# MERGE Statement in SQL

The **MERGE** statement (also known as **UPSERT**) is used to **insert, update, or delete** records in a target table based on a source table. It is useful for **handling slowly changing dimensions (SCD), data synchronization, and ETL processes**.

Syntax -
```sql
MERGE INTO target_table AS tgt
USING source_table AS src
ON tgt.id = src.id
WHEN MATCHED THEN
    UPDATE SET tgt.column1 = src.column1
WHEN NOT MATCHED THEN
    INSERT (id, column1) VALUES (src.id, src.column1);
```

---

### When to Use MERGE?
✅ **Data Synchronization** – Keep tables in sync with changes.<br>
✅ **ETL Processes** – Manage updates in data warehouses.<br>
✅ **SCD Type 2 Implementation** – Track history with new records.<br>

---

### Example: Updating Employee Records

Consider two tables:
- `Employees` (Target Table)
- `NewEmployees` (Source Table)

```sql
MERGE INTO Employees AS emp
USING NewEmployees AS new_emp
ON emp.EmployeeID = new_emp.EmployeeID
WHEN MATCHED THEN
    UPDATE SET emp.Name = new_emp.Name, emp.Salary = new_emp.Salary
WHEN NOT MATCHED THEN
    INSERT (EmployeeID, Name, Salary)
    VALUES (new_emp.EmployeeID, new_emp.Name, new_emp.Salary);
```
👉 If an employee exists, their details are updated. If they don’t exist, a new record is inserted.

---

### Handling DELETE in MERGE
If the record exists in the target but **not** in the source, it can be deleted.

```sql
MERGE INTO Employees AS emp
USING NewEmployees AS new_emp
ON emp.EmployeeID = new_emp.EmployeeID
WHEN MATCHED THEN
    UPDATE SET emp.Name = new_emp.Name, emp.Salary = new_emp.Salary
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;
```
👉 This removes employees who are **no longer present** in the source table.

---

### MERGE for SCD Type 2
MERGE can be used to **track history** by marking old records as inactive.

```sql
MERGE INTO Employees AS emp
USING NewEmployees AS new_emp
ON emp.EmployeeID = new_emp.EmployeeID AND emp.IsActive = 1
WHEN MATCHED THEN
    UPDATE SET emp.IsActive = 0, emp.EndDate = GETDATE()
WHEN NOT MATCHED THEN
    INSERT (EmployeeID, Name, Salary, StartDate, IsActive)
    VALUES (new_emp.EmployeeID, new_emp.Name, new_emp.Salary, GETDATE(), 1);
```
👉 This deactivates old records and inserts a new version of the employee.

---

### Key Advantages of MERGE
- 🚀 **Reduces multiple queries** (INSERT, UPDATE, DELETE in one statement).
- ⚡ **Improves performance** in data warehouse updates.
- ✅ **Ensures data consistency** in ETL workflows.

---

### Conclusion
The **MERGE** statement is a powerful SQL feature that simplifies data synchronization, supports SCD implementation, and optimizes ETL processes. 🚀
