
**Functions** in SQL are reusable, pre-defined blocks of SQL code that perform a specific task and return a value. They are used to encapsulate logic that can be executed with a single call within SQL queries or statements.

#### **Types of Functions in SQL:**

1. **Scalar Functions**: 
   - Return a single value (e.g., `GETDATE()`, `LOWER()`, `ROUND()`).
   - Can be used anywhere an expression is allowed.

2. **Aggregate Functions**:
   - Perform a calculation on a set of values and return a single value (e.g., `SUM()`, `AVG()`, `COUNT()`).
   - Often used with `GROUP BY` clauses.

3. **Table-Valued Functions**:
   - Return a table as a result. This can be used as a subquery or a table source in `FROM` clauses.

4. **System Functions**:
   - Built-in functions provided by SQL for various operations (e.g., `@@IDENTITY`, `@@ROWCOUNT`).

5. **User-Defined Functions (UDFs)**:
   - Functions created by users to perform custom operations.
   - Can be scalar or table-valued.

#### **Syntax for a Scalar User-Defined Function:**

```sql
CREATE FUNCTION function_name (@parameter datatype)
RETURNS return_datatype
AS
BEGIN
    -- Function logic here
    RETURN some_value;  -- Return a single value
END;
```

**Example:**

```sql
CREATE FUNCTION get_discounted_price (@price DECIMAL(10,2), @discount DECIMAL(5,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @price - (@price * @discount / 100);
END;
```

### **Differences Between Functions and Stored Procedures**

| **Aspect**           | **Functions**                                              | **Stored Procedures**                                           |
|----------------------|------------------------------------------------------------|-----------------------------------------------------------------|
| **Purpose**          | Perform a specific calculation and return a value.         | Perform a series of actions and may return multiple values or none. |
| **Return Type**      | Must return a single value (scalar) or a table.            | Can return zero or more values, including multiple result sets. |
| **Call Syntax**      | Called within SQL queries (e.g., `SELECT`, `WHERE`, `JOIN`).| Called using the `EXEC` or `CALL` statement.                   |
| **Parameter Types**  | Only input parameters are allowed.                         | Supports input, output, and input-output parameters.            |
| **Transaction Control** | Cannot contain `INSERT`, `UPDATE`, `DELETE`, or `MERGE` statements. | Can contain `INSERT`, `UPDATE`, `DELETE`, and `MERGE` statements. |
| **Side Effects**     | Cannot modify database state (no changes to tables).       | Can modify database state (e.g., updating or inserting records).|
| **Error Handling**   | Limited error handling with `TRY...CATCH`.                 | Full error handling with `TRY...CATCH`, `THROW`, `RAISEERROR`.  |
| **Usage in Queries** | Can be used in `SELECT`, `WHERE`, `JOIN`, etc.             | Cannot be used directly in queries (except by invoking inside a function). |
| **Performance**      | Generally faster for simple calculations.                  | May be slower if used inappropriately in contexts where a function would suffice. |

### **Examples of Usage**

1. **Using a Function in a Query**:
   ```sql
   SELECT product_name, get_discounted_price(price, 10) AS discounted_price
   FROM products;
   ```
   - Here, `get_discounted_price` is a user-defined function applied directly in a `SELECT` statement.

2. **Using a Stored Procedure**:
   ```sql
   EXEC update_product_price @product_id = 101, @new_price = 19.99;
   ```
   - This calls a stored procedure that might update a product's price in the database.

### **Key Takeaways**

- **Functions** are ideal for computations or retrieving single values within SQL queries. They are not designed to perform data manipulation (e.g., inserts or updates).
- **Stored Procedures** are more powerful, allowing for complex operations that can include modifying the database, handling transactions, and controlling the flow of execution. They are best used for operations that require more procedural logic, such as data manipulation or multiple-step processes.

Both functions and stored procedures have their specific use cases and are chosen based on the requirements of the task at hand.

---

In SQL, attributes of functions refer to various properties and characteristics that define how a function behaves and interacts with SQL queries and the database. Here’s a detailed breakdown of these attributes:

### **1. Return Type**

- **Scalar Functions**: Return a single value of a specific data type (e.g., `INT`, `VARCHAR`, `DATE`).
- **Table-Valued Functions**: Return a result set (table) and can be used like a table in `FROM` clauses.

  **Example:**
  ```sql
  CREATE FUNCTION get_employee_salary (emp_id INT)
  RETURNS DECIMAL(10,2)
  AS
  BEGIN
      RETURN (SELECT salary FROM employees WHERE employee_id = emp_id);
  END;
  ```

### **2. Parameters**

- **Input Parameters**: Parameters that are passed to the function when it is called.
- **Output Parameters**: Parameters that are used to return values from the function (Note: Not typically used in scalar functions but are part of stored procedures).
- **Input/Output Parameters**: Parameters that can be used both to pass values in and to return values from the function (more relevant in stored procedures).

  **Example:**
  ```sql
  CREATE FUNCTION calculate_area (radius FLOAT)
  RETURNS FLOAT
  AS
  BEGIN
      RETURN PI() * radius * radius;
  END;
  ```

### **3. Deterministic vs. Non-Deterministic**

- **Deterministic Functions**: Always return the same result given the same input values.
- **Non-Deterministic Functions**: May return different results for the same input values due to factors like current time or random values.

  **Example of Deterministic:**
  ```sql
  CREATE FUNCTION square_number (number INT)
  RETURNS INT
  AS
  BEGIN
      RETURN number * number;
  END;
  ```

  **Example of Non-Deterministic:**
  ```sql
  CREATE FUNCTION get_current_date ()
  RETURNS DATE
  AS
  BEGIN
      RETURN CURRENT_DATE;
  END;
  ```

### **4. Inline vs. Multistatement Functions**

- **Inline Functions**: Defined with a single `SELECT` statement and are often more efficient.
- **Multistatement Functions**: Defined with multiple statements and may include procedural logic like conditional statements or loops.

  **Example of Inline Function:**
  ```sql
  CREATE FUNCTION get_full_name (first_name VARCHAR(50), last_name VARCHAR(50))
  RETURNS VARCHAR(100)
  AS
  BEGIN
      RETURN CONCAT(first_name, ' ', last_name);
  END;
  ```

  **Example of Multistatement Function:**
  ```sql
  CREATE FUNCTION calculate_discounted_price (price DECIMAL(10,2), discount DECIMAL(5,2))
  RETURNS DECIMAL(10,2)
  AS
  BEGIN
      DECLARE final_price DECIMAL(10,2);
      SET final_price = price - (price * discount / 100);
      RETURN final_price;
  END;
  ```

### **5. Security and Permissions**

- Functions can have different permissions depending on the database system:
  - **EXECUTE Permission**: Required for calling the function.
  - **ALTER Permission**: Required for modifying the function definition.
  - **CONTROL Permission**: Required for full control over the function.

### **6. Volatility**

- **Stable**: Functions that do not modify database state and return consistent results for the same inputs.
- **Immutable**: Functions that are stable and always produce the same results for given inputs.
- **Volatile**: Functions that can produce different results for the same input, often due to database state changes (more common in stored procedures).

### **7. Error Handling**

- **Exception Handling**: Some databases allow handling errors within functions using mechanisms like `TRY...CATCH` (in SQL Server) or `BEGIN...EXCEPTION` (in PostgreSQL).

  **Example:**
  ```sql
  CREATE FUNCTION divide_numbers (numerator DECIMAL, denominator DECIMAL)
  RETURNS DECIMAL
  AS
  BEGIN
      BEGIN
          RETURN numerator / denominator;
      EXCEPTION
          WHEN division_by_zero THEN
              RETURN NULL;  -- Handle division by zero
      END;
  END;
  ```

### **8. Performance Considerations**

- Functions should be optimized for performance, especially when used in large queries. For example:
  - Avoid using non-deterministic functions in indexes or join conditions.
  - Use inline functions for simple computations.

### **9. Usage Context**

- **Within Queries**: Functions can be used in `SELECT`, `WHERE`, `JOIN`, `ORDER BY`, and other SQL clauses.
- **In Stored Procedures**: Functions can be called from within stored procedures to perform calculations or return values.

### **Summary**

SQL functions are powerful tools for encapsulating reusable logic, and their attributes determine how they interact with SQL queries and the database. Understanding these attributes helps in designing efficient and effective functions that meet your specific needs.