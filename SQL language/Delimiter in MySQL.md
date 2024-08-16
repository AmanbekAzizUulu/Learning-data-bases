In MySQL, `DELIMITER` is a command used to change the default statement delimiter (which is `;`) to another string. This is particularly useful when you are defining stored procedures, functions, or triggers, where multiple statements are included within a single block of code.

### Why Use `DELIMITER`?

By default, MySQL treats the semicolon (`;`) as the end of a statement. However, when you are writing a stored procedure, function, or trigger, you might need to use semicolons within the block of code, such as between individual SQL statements. If MySQL interprets a semicolon as the end of the procedure definition, it will not be able to parse the entire block correctly.

To avoid this, you can temporarily change the delimiter to something else, such as `$$` or `//`, define your procedure, and then change the delimiter back to `;`.

### Syntax

The basic syntax for using `DELIMITER` is:

```sql
DELIMITER new_delimiter
```

### Example

Let’s create a simple stored procedure that uses the `DELIMITER` command.

```sql
DELIMITER $$

CREATE PROCEDURE AddEmployee(
    IN FirstName VARCHAR(50),
    IN LastName VARCHAR(50),
    IN BirthDate DATE,
    IN HireDate DATE
)
BEGIN
    INSERT INTO Employees (FirstName, LastName, BirthDate, HireDate)
    VALUES (FirstName, LastName, BirthDate, HireDate);
END $$

DELIMITER ;
```

### Explanation

1. **Change the Delimiter**:
   - `DELIMITER $$` changes the delimiter from `;` to `$$`. Now, MySQL knows that the end of the procedure is marked by `$$`, not by `;`.

2. **Create the Procedure**:
   - The procedure is defined with multiple SQL statements separated by semicolons. Because the delimiter has been changed, these semicolons do not signal the end of the procedure.

3. **Reset the Delimiter**:
   - `DELIMITER ;` resets the delimiter back to the default `;` after the procedure has been defined.

### Usage Notes

- The `DELIMITER` command is primarily used in MySQL command-line clients or scripts. It is not part of the SQL standard, so you won’t typically see it in application code or within database definitions in a GUI-based database management tool.
- It is essential to reset the delimiter back to `;` after defining the procedure or function to ensure that subsequent SQL commands are executed correctly.