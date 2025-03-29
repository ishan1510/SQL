# Transactions & Error Handling in SQL Server

## 1️⃣ Transactions in SQL Server
A **transaction** is a sequence of operations that are executed as a single unit of work. Transactions follow the **ACID properties**:
- **Atomicity**: All operations within a transaction succeed or none do.
- **Consistency**: Ensures data integrity before and after the transaction.
- **Isolation**: Transactions do not interfere with each other.
- **Durability**: Once committed, the changes are permanent.

### **BEGIN TRANSACTION, COMMIT, and ROLLBACK**
#### ✅ Example of Using Transactions
```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 500 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 500 WHERE AccountID = 2;

COMMIT TRANSACTION;  -- Saves the changes permanently
```
If something goes wrong, we can **ROLLBACK** instead of committing.

#### ❌ Handling Errors with ROLLBACK
```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 500 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 500 WHERE AccountID = 2;

IF @@ERROR <> 0  -- If an error occurs, rollback changes
    ROLLBACK TRANSACTION;
ELSE
    COMMIT TRANSACTION;
```

---

## 2️⃣ TRY...CATCH for Error Handling
SQL Server provides **TRY...CATCH** for handling errors gracefully.

### **Syntax:**
```sql
BEGIN TRY
    -- SQL Statements
END TRY
BEGIN CATCH
    -- Error Handling Logic
END CATCH
```

### ✅ Example of TRY...CATCH in a Stored Procedure
```sql
CREATE PROCEDURE TransferMoney
    @FromAccount INT,
    @ToAccount INT,
    @Amount DECIMAL(10,2)
AS
BEGIN
    BEGIN TRANSACTION;
    BEGIN TRY
        UPDATE Accounts SET Balance = Balance - @Amount WHERE AccountID = @FromAccount;
        UPDATE Accounts SET Balance = Balance + @Amount WHERE AccountID = @ToAccount;
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        PRINT 'Error Occurred: ' + ERROR_MESSAGE();
    END CATCH;
END;
```
👉 **Key Error Functions in CATCH Block:**
- `ERROR_MESSAGE()` → Returns the error message.
- `ERROR_NUMBER()` → Returns the error number.
- `ERROR_SEVERITY()` → Returns severity of the error.
- `ERROR_STATE()` → Returns the error state.

---

## 3️⃣ Nested Transactions in SQL Server
SQL Server **does not fully support nested transactions**, but you can use `@@TRANCOUNT` to manage them.

### ✅ Example:
```sql
BEGIN TRANSACTION;
    BEGIN TRANSACTION;
        -- Some SQL operations
    COMMIT TRANSACTION;
COMMIT TRANSACTION;
```
> If an inner transaction rolls back, the outer one also **must rollback**.

---

## 4️⃣ Savepoints for Partial Rollbacks
Savepoints allow partial rollback within a transaction.
```sql
BEGIN TRANSACTION;
UPDATE Employees SET Salary = Salary * 1.1 WHERE Department = 'IT';
SAVE TRANSACTION BeforeHRUpdate;
UPDATE Employees SET Salary = Salary * 1.1 WHERE Department = 'HR';
ROLLBACK TRANSACTION BeforeHRUpdate;  -- Only rolls back HR update
COMMIT TRANSACTION;
```

---

## Conclusion
- Use **TRANSACTION** to ensure atomic operations.
- Use **COMMIT** to finalize and **ROLLBACK** to undo changes.
- **TRY...CATCH** helps handle errors gracefully.
- **Savepoints** allow partial rollbacks.

Applying these techniques ensures **data consistency and reliability** in SQL operations. 🚀
