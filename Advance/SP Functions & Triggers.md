# Stored Procedures & Functions in SQL Server

SQL allows us to write reusable logic in the form of **Stored Procedures, Functions, and Triggers**, making database operations easier, faster, and more organized. Let’s break these concepts down in a simple way. 🚀

---

## 1. Stored Procedures (Reusable SQL Scripts with Parameters)
A **Stored Procedure** is like a recipe that tells the database exactly what to do. Instead of writing the same SQL query again and again, we store it as a **procedure** and call it when needed.

### ✅ Creating a Simple Stored Procedure
Imagine we have a table called `Employees` and we want to fetch all employee details. Instead of writing the query each time, we can store it like this:
```sql
CREATE PROCEDURE GetEmployees
AS
BEGIN
    SELECT * FROM Employees;
END;
```
### ✅ Stored Procedure with Parameters
We can also **pass values** (like asking a function to return something specific). If we only want to fetch details of a single employee:
```sql
CREATE PROCEDURE GetEmployeeByID
    @EmployeeID INT
AS
BEGIN
    SELECT * FROM Employees WHERE EmployeeID = @EmployeeID;
END;
```
### ✅ Executing a Stored Procedure
To run the stored procedures, we use:
```sql
EXEC GetEmployees;  -- Fetch all employees
EXEC GetEmployeeByID @EmployeeID = 101;  -- Fetch details of Employee 101
```
### ✅ Why Use Stored Procedures?
- **Saves Time**: No need to rewrite the same SQL queries.
- **Improves Performance**: The database stores and optimizes the query.
- **More Secure**: Users don’t need direct access to tables; they just execute the procedure.

---

## 2. SQL Functions
SQL **Functions** are like mini-programs that return either a single value or an entire table. They help in breaking down complex queries into smaller, manageable pieces.

### 🔹 **Scalar Functions (Return a Single Value)**
A **scalar function** is like a calculator – it takes input and returns a single value. For example, to count the total number of employees:
```sql
CREATE FUNCTION GetTotalEmployees()
RETURNS INT
AS
BEGIN
    DECLARE @Total INT;
    SELECT @Total = COUNT(*) FROM Employees;
    RETURN @Total;
END;
```
#### ✅ Using a Scalar Function
```sql
SELECT dbo.GetTotalEmployees() AS TotalEmployees;
```
This will return **one number**, like `250`, meaning there are 250 employees.

### 🔹 **Table-Valued Functions (Return a Table)**
A **table-valued function** is like a small database query inside a function. It returns a table instead of a single value.
```sql
CREATE FUNCTION GetEmployeesByDepartment (@DepartmentID INT)
RETURNS TABLE
AS
RETURN (
    SELECT EmployeeID, EmployeeName FROM Employees WHERE DepartmentID = @DepartmentID
);
```
#### ✅ Using a Table-Valued Function
```sql
SELECT * FROM dbo.GetEmployeesByDepartment(2);
```
This returns **a list of employees** in a particular department.

### ✅ Why Use Functions?
- **Make Queries Simpler**: Instead of writing long SQL statements, just call a function.
- **Can Be Used in SELECT Queries**: Unlike stored procedures, functions work inside `SELECT`.
- **Reusable Code**: Write once, use many times.

---

## 3. Triggers (Automating Tasks When Data Changes)
A **Trigger** is like a security camera – it watches a table and **automatically** does something when data changes. For example:

### 🔹 **Example: Keeping a Log When a New Employee is Added**
```sql
CREATE TRIGGER trg_EmployeeInsert
ON Employees
AFTER INSERT
AS
BEGIN
    INSERT INTO AuditLog (EventType, EventTime)
    VALUES ('Employee Inserted', GETDATE());
END;
```
This trigger **automatically adds a record** to `AuditLog` whenever a new employee is added.

### 🔹 **Example: Prevent Deleting Managers**
What if we don’t want to allow deleting managers? We can use a trigger to stop it.
```sql
CREATE TRIGGER trg_PreventManagerDelete
ON Employees
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM deleted WHERE Role = 'Manager')
    BEGIN
        RAISERROR ('Managers cannot be deleted!', 16, 1);
        ROLLBACK;
    END
END;
```
If someone tries to delete a **Manager**, the system **throws an error and stops** the deletion.

### ✅ Why Use Triggers?
- **Automate Business Rules**: Automatically enforce policies like logging or security.
- **Ensure Data Integrity**: Stop accidental or unauthorized changes.
- **Reduce Manual Work**: Database handles tasks without human intervention.

---

## Conclusion 🎯
- **Stored Procedures** help execute reusable SQL logic efficiently.
- **Functions** allow returning **single values** (`Scalar`) or **entire tables** (`Table-Valued`).
- **Triggers** automate database actions when data changes.

These features **save time, improve performance, and make databases more secure**! 🚀
