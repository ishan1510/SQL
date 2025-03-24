# 📘 Creating Tables in SQL  

## 1️. Basic Syntax 
To create a table, use the `CREATE TABLE` statement:  
```sql
CREATE TABLE employees (
    employee_id INT IDENTITY(1,1) PRIMARY KEY,  -- Auto-incrementing primary key
    first_name NVARCHAR(50) ,           -- Unicode string
    last_name NVARCHAR(50) ,
    birth_date DATE,                            -- Date only
    hire_date DATETIME,                         -- Date & time
    salary DECIMAL(10,2) ,                      -- Decimal with precision
    is_active BIT DEFAULT 1,                    -- Boolean representation (0 or 1)
    email VARCHAR(100) ,                        -- Store character
    profile_image VARBINARY(MAX)                -- Stores images or binary data
);
```
<span style="font-size:8px;">

Explanation of Data Types:

- INT IDENTITY(1,1) → Auto-incrementing integer (Primary Key).
- NVARCHAR(50) → Stores Unicode text up to 50 characters.
- DATE → Stores only date (YYYY-MM-DD).
- DATETIME → Stores date and time.
- DECIMAL(10,2) → Decimal value with 10 digits, 2 after the decimal point.
- BIT → Boolean-like column (0 = false, 1 = true).
- VARCHAR(100) → Variable-length text (non-Unicode).
- VARBINARY(MAX) → Stores binary data (like images).
</span>

## 2️. Adding Constraints 
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,                      -- Unique identifier
    customer_id INT NOT NULL,                      -- Must have a value
    order_date DATETIME DEFAULT GETDATE(),         -- Default current date/time
    total_amount DECIMAL(10,2) CHECK (total_amount >= 0),  -- Must be non-negative
    status VARCHAR(20) DEFAULT 'Pending',          -- Default value
    CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(customer_id),  -- Foreign key
    CONSTRAINT unique_order UNIQUE (customer_id, order_date)  -- Ensures unique orders per customer per day
);
```
Explanation of Constraints:

- PRIMARY KEY → Ensures uniqueness and acts as an identifier.
- NOT NULL → Column cannot have NULL values.
- DEFAULT → Sets a default value if no value is provided.
- CHECK → Ensures values meet a condition.
- FOREIGN KEY → Links one table to another.
- UNIQUE → Ensures values in a column are unique.


## 3. Creating a Table from Another Table

```sql
CREATE TABLE backup_employees AS  
SELECT * FROM employees WHERE 1=0;  -- Creates an empty copy
```
