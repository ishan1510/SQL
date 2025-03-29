# 📘 Updating Data in SQL  

## 1️. Basic Syntax  
To update existing records in a table, use the `UPDATE` statement with a `WHERE` condition to specify which rows should be modified.  

```sql
UPDATE employees  
SET salary = 70000  
WHERE employee_id = 1;
```
👉 This updates the salary of the employee with employee_id = 1 to 70,000.


## 2. Updating Multiple Columns
You can update multiple columns in a single query:
```sql
UPDATE employees  
SET salary = 80000, is_active = 0  
WHERE employee_id = 2;
```
👉 This changes the salary and sets is_active to 0 for employee_id = 2.


## 3. Updating All Rows (Use with Caution 🚨)
If you omit the WHERE clause, all rows in the table will be updated!
```sql
UPDATE employees  
SET is_active = 1;
```
⚠️ This will set is_active = 1 for every employee in the table.


## 4. Using CASE in UPDATE Statements
You can use CASE to conditionally update data:
```sql
UPDATE employees  
SET salary =  
    CASE  
        WHEN salary < 50000 THEN salary * 1.10  -- Increase by 10% if salary < 50K  
        WHEN salary BETWEEN 50000 AND 70000 THEN salary * 1.05  -- Increase by 5%  
        ELSE salary  -- No change  
    END;
```
👉 This updates salaries based on different conditions.


## 5. Updating Data from Another Table
You can update data using values from another table:
```sql
UPDATE employees  
SET salary = (  
    SELECT AVG(salary)  
    FROM employees  
    WHERE is_active = 1  
)  
WHERE is_active = 0;
```
👉 This sets the salary of inactive employees to the average salary of active employees.


