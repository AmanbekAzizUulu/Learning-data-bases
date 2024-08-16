Adding parameters to stored procedures in SQL allows you to pass values into the procedure when it is executed. These parameters can be used to filter data, control flow, or perform calculations within the procedure.
### Syntax for Adding Parameters

When you define a stored procedure, you can specify parameters by providing their name, data type, and optionally their direction (input, output, or both). The general syntax for defining parameters is:

```sql
CREATE PROCEDURE procedure_name (
    [IN | OUT | INOUT] parameter_name datatype,
    [IN | OUT | INOUT] parameter_name datatype,
    ...
)
BEGIN
    -- SQL statements using the parameters
END;
```

### Parameter Types

1. **IN**: The default type. `IN` parameters are used to pass values into the procedure. The procedure can read these values but cannot modify them.
   - Example: `IN employee_id INT`

2. **OUT**: Used to return values from the procedure to the caller. The procedure can modify these parameters, and the final value is accessible to the caller.
   - Example: `OUT total_sales DECIMAL(10,2)`

3. **INOUT**: Combines the behaviors of `IN` and `OUT`. The parameter can be used to pass values into the procedure, and the procedure can modify it to return a value.
   - Example: `INOUT counter INT`

### Examples

#### 1. **Procedure with IN Parameters**

This procedure accepts an `employee_id` and returns information about the employee:

```sql
CREATE PROCEDURE GetEmployeeInfo(
    IN employee_id INT
)
BEGIN
    SELECT
        employee_name,
        department,
        hire_date
    FROM
        Employees
    WHERE
        id = employee_id;
END;
```

**Usage**:

```sql
CALL GetEmployeeInfo(101);
```

#### 2. **Procedure with OUT Parameters**

This procedure calculates the total sales and returns the result via an `OUT` parameter:

```sql
CREATE PROCEDURE CalculateTotalSales(
    IN start_date DATE,
    IN end_date DATE,
    OUT total_sales DECIMAL(10, 2)
)
BEGIN
    SELECT
        SUM(amount)
    INTO
        total_sales
    FROM
        Sales
    WHERE
        sale_date BETWEEN start_date AND end_date;
END;
```

**Usage**:

```sql
DECLARE @sales DECIMAL(10, 2);
CALL CalculateTotalSales('2024-01-01', '2024-07-31', @sales);
SELECT @sales;
```

#### 3. **Procedure with INOUT Parameters**

This procedure takes an integer counter, increments it, and returns the updated value:

```sql
CREATE PROCEDURE IncrementCounter(
    INOUT counter INT
)
BEGIN
    SET counter = counter + 1;
END;
```

**Usage**:

```sql
DECLARE @counter INT;
SET @counter = 5;
CALL IncrementCounter(@counter);
SELECT @counter;  -- Result will be 6
```

### Default Values for Parameters (MySQL 8.0+)

In MySQL 8.0 and later, you can also define default values for parameters. This allows the procedure to be called without providing all the parameters, using defaults where applicable:

```sql
CREATE PROCEDURE ExampleProcedure(
    IN param1 INT DEFAULT 10,
    IN param2 VARCHAR(50) DEFAULT 'Default String'
)
BEGIN
    -- Procedure logic
END;
```

**Usage**:

```sql
CALL ExampleProcedure();          -- Uses default values for both parameters
CALL ExampleProcedure(5);         -- Overrides param1, uses default for param2
CALL ExampleProcedure(5, 'Text'); -- Overrides both parameters
```

### Summary

- **IN Parameters**: Pass data into the procedure.
- **OUT Parameters**: Return data from the procedure.
- **INOUT Parameters**: Pass data into the procedure and return modified data.

By using these types of parameters, you can create versatile and reusable stored procedures that can perform complex operations and return results to the caller.

---

In MySQL stored procedures, `@` is used for **session variables** or **user-defined variables**. These variables are accessible throughout the session and are not limited to the stored procedure or function where they are defined. 

When defining parameters within a stored procedure, you do not use the `@` symbol because these parameters are **local to the procedure** and are not session variables. The syntax for referring to these local parameters in the procedure is simply their name without the `@` prefix.

Here’s why:

### Differences Between Local Parameters and Session Variables

1. **Local Parameters**:
   - **Scope**: Local to the procedure or function.
   - **No `@` Prefix**: When defining or using a parameter inside a stored procedure, you refer to it by its name without the `@` prefix.
   - **Example**: In your procedure, `state` is a local parameter.

2. **Session Variables (User-Defined Variables)**:
   - **Scope**: Accessible across the entire session, not just within a stored procedure or function.
   - **`@` Prefix**: These variables are always prefixed with `@`.
   - **Example**: `@my_variable` is a session variable.

### Example to Clarify:

```sql
CREATE PROCEDURE my_procedure(
    IN my_param INT
)
BEGIN
    DECLARE local_var INT;
    
    -- Using the local parameter
    SET local_var = my_param + 1;
    
    -- Using a session variable
    SET @session_var = my_param + 2;
    
    SELECT local_var, @session_var;
END;
```

In the example:
- `my_param` is a local parameter.
- `local_var` is a local variable declared within the procedure.
- `@session_var` is a session variable accessible throughout the session.

### Summary

- Use local parameters and variables within stored procedures **without** the `@` prefix.
- Use the `@` prefix only for session variables or user-defined variables that persist beyond the procedure scope.

In your original query, `state` is a local parameter within the procedure, so it should be used without the `@` prefix.


