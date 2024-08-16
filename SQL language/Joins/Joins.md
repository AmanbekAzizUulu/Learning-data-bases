In SQL, joins are used to combine rows from two or more tables based on a related column between them. There are several types of joins, each serving a different purpose:

1. **INNER JOIN**: Returns only the rows where there is a match in both tables.
   ```sql
   SELECT 
	   columns
   FROM 
	   table1
   INNER JOIN 
	   table2
	   ON 
		   table1.column = table2.column;
   ```

2. **LEFT (OUTER) JOIN**: Returns all rows from the left table, and the matched rows from the right table. If there is no match, the result is NULL on the side of the right table.
   ```sql
   SELECT 
	   columns
   FROM 
	   table1
   LEFT JOIN 
	   table2
	   ON 
		   table1.column = table2.column;
   ```

3. **RIGHT (OUTER) JOIN**: Returns all rows from the right table, and the matched rows from the left table. If there is no match, the result is NULL on the side of the left table.
   ```sql
   SELECT 
	   columns
   FROM 
	   table1
   RIGHT JOIN 
	   table2
	   ON 
		   table1.column = table2.column;
   ```

4. **FULL (OUTER) JOIN**: Returns all rows when there is a match in one of the tables. If there is no match, the result is NULL from the table that lacks the match.
   ```sql
   SELECT 
	   columns
   FROM 
	   table1
   FULL OUTER JOIN 
	   table2
	   ON 
		   table1.column = table2.column;
   ```

5. **CROSS JOIN**: Returns the Cartesian product of the two tables, meaning every row in the first table is paired with every row in the second table.
   ```sql
   SELECT 
	   columns
   FROM 
	   table1
   CROSS JOIN 
	   table2;
   ```

6. **SELF JOIN**: A regular join, but the table is joined with itself. Useful for finding hierarchical data or comparing rows within the same table.
   ```sql
   SELECT a.columns, b.columns
   FROM table1 a, table1 b
   WHERE a.column = b.column;
   ```

7. **NATURAL JOIN**: Joins two tables based on columns with the same name and data types in both tables. This type of join eliminates the need to specify a condition.
   ```sql
   SELECT columns
   FROM table1
   NATURAL JOIN table2;
   ```

8. **UNION JOIN**: This is not an actual join but a way to combine the results of two or more SELECT statements into a single result set. It removes duplicate rows by default.
   ```sql
   SELECT columns FROM table1
   UNION
   SELECT columns FROM table2;
   ```

Each type of join serves different use cases depending on how you want to retrieve and combine data from multiple tables.


---

In SQL joins, the "left" table and the "right" table are determined by the order in which you specify them in the SQL query. Here’s how you can determine which is which:

1. **In a LEFT JOIN:**
   - The table that appears before the `LEFT JOIN` keyword is the **left table**.
   - The table that appears after the `LEFT JOIN` keyword is the **right table**.

   Example:
   ```sql
   SELECT columns
   FROM table1  -- This is the left table
   LEFT JOIN table2  -- This is the right table
   ON table1.column = table2.column;
   ```

2. **In a RIGHT JOIN:**
   - The table that appears before the `RIGHT JOIN` keyword is the **left table**.
   - The table that appears after the `RIGHT JOIN` keyword is the **right table**.

   Example:
   ```sql
   SELECT columns
   FROM table1  -- This is the left table
   RIGHT JOIN table2  -- This is the right table
   ON table1.column = table2.column;
   ```

### Visual Representation

Consider the following tables:

- `employees` (left table)
- `departments` (right table)

### LEFT JOIN Example
```sql
SELECT employees.name, departments.department_name
FROM employees
LEFT JOIN departments
ON employees.department_id = departments.id;
```

In this example, `employees` is the left table, and `departments` is the right table. The result will include all rows from `employees`, and the matching rows from `departments`. If there is no match, the result will have `NULL` for columns from the `departments` table.

### RIGHT JOIN Example
```sql
SELECT employees.name, departments.department_name
FROM employees
RIGHT JOIN departments
ON employees.department_id = departments.id;
```

In this example, `employees` is still the left table, and `departments` is still the right table. The result will include all rows from `departments`, and the matching rows from `employees`. If there is no match, the result will have `NULL` for columns from the `employees` table.

Understanding the position of tables relative to the `JOIN` keyword helps you determine which table is considered "left" or "right."


---

The main difference between **INNER JOIN** and **OUTER JOIN** (which includes LEFT JOIN, RIGHT JOIN, and FULL JOIN) lies in how they handle unmatched rows between the tables being joined.

### INNER JOIN

- **Definition**: Returns only the rows where there is a match in both tables.
- **Behavior**: Excludes rows that do not have matching values in both tables.

**Example**:
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```
**Result**: Only the rows where `table1.column` matches `table2.column` are included in the result set.

### OUTER JOIN

**Definition**: Returns rows that have matching values in one table and includes unmatched rows from one or both tables, depending on the type of OUTER JOIN.

1. **LEFT OUTER JOIN (LEFT JOIN)**:
   - **Behavior**: Returns all rows from the left table, and the matched rows from the right table. If there is no match, the result is NULL on the side of the right table.
   
   **Example**:
   ```sql
   SELECT columns
   FROM table1
   LEFT JOIN table2
   ON table1.column = table2.column;
   ```
   **Result**: All rows from `table1` are included. If there is no match in `table2`, columns from `table2` will be NULL.

2. **RIGHT OUTER JOIN (RIGHT JOIN)**:
   - **Behavior**: Returns all rows from the right table, and the matched rows from the left table. If there is no match, the result is NULL on the side of the left table.
   
   **Example**:
   ```sql
   SELECT columns
   FROM table1
   RIGHT JOIN table2
   ON table1.column = table2.column;
   ```
   **Result**: All rows from `table2` are included. If there is no match in `table1`, columns from `table1` will be NULL.

3. **FULL OUTER JOIN (FULL JOIN)**:
   - **Behavior**: Returns all rows when there is a match in one of the tables. If there is no match, the result is NULL from the table that lacks the match.
   
   **Example**:
   ```sql
   SELECT columns
   FROM table1
   FULL OUTER JOIN table2
   ON table1.column = table2.column;
   ```
   **Result**: All rows from both `table1` and `table2` are included. Rows without a match in the other table will have NULLs for the columns of the unmatched table.

### Summary

- **INNER JOIN**: Only matched rows are included.
- **OUTER JOIN**: Includes matched rows and unmatched rows from one or both tables:
  - **LEFT JOIN**: All rows from the left table and matched rows from the right table.
  - **RIGHT JOIN**: All rows from the right table and matched rows from the left table.
  - **FULL JOIN**: All rows from both tables, with NULLs for unmatched rows.
---
[[Right Outer Join]]
[[Cross Join]]
[[Left Outer Join]]
[[Full Outer Join]]
[[Recursive Join]]
[[Self Join]]