### Представления (Views) в SQL

**Представление (View)** — это виртуальная таблица в SQL, которая создается на основе результата SQL-запроса. Представление не содержит данных само по себе, а лишь показывает данные, полученные из одной или нескольких таблиц. Оно функционирует как сохраненный запрос, который можно использовать для упрощения сложных запросов, повышения безопасности данных и улучшения читаемости кода.

Views in SQL provide several benefits, making them a valuable tool for database management. Here are some key advantages:

#### 1. **Data Abstraction and Simplification**
   - **Simplified Queries**: Views allow you to encapsulate complex queries, making it easier to work with complicated data sets. Instead of writing complex joins or aggregations repeatedly, you can create a view and reference it like a table.
   - **Abstraction**: Views can hide the complexity of the underlying data structures from users. For example, a view can combine columns from multiple tables into a single, user-friendly interface.

#### 2. **Security and Access Control**
   - **Restricted Access**: Views can be used to restrict access to specific rows or columns of data, allowing users to see only the information they are authorized to access. This is especially useful in situations where sensitive data is involved.
   - **Row and Column-Level Security**: By defining a view with specific filters or selecting only certain columns, you can control what data a user can see without altering the underlying tables.

#### 3. **Consistency and Reusability**
   - **Consistent Results**: Views ensure that certain queries produce consistent results, which can be reused across multiple applications or queries. This helps maintain data consistency across different parts of an application.
   - **Code Reusability**: Instead of duplicating complex SQL queries, you can create a view and reuse it wherever needed, leading to cleaner and more maintainable code.

#### 4. **Maintenance and Evolution**
   - **Easier Maintenance**: If the logic for a particular set of data changes, you can simply update the view instead of updating multiple queries scattered throughout the application.
   - **Database Evolution**: Views can help in evolving the database schema by providing a stable interface for legacy applications while the underlying schema changes.

#### 5. **Performance Optimization**
   - **Materialized Views**: Some databases support materialized views, which store the results of the view physically on disk. This can significantly improve performance for complex queries that are frequently executed.
   - **Query Optimization**: In some cases, views can help optimize queries by breaking them down into simpler parts that the database engine can handle more efficiently.

#### 6. **Derived Data**
   - **Computed Columns**: Views can include computed columns, where the data is derived from other columns. This allows for on-the-fly calculations without storing redundant data in the database.

#### 7. **Encapsulation of Business Logic**
   - **Business Rules**: Views can encapsulate business rules and logic within the database layer, ensuring that all applications accessing the data adhere to these rules.

By using views effectively, you can improve the security, maintainability, and performance of your database systems, while also making life easier for users and developers alike.
### Зачем использовать представления?

1. **Упрощение сложных запросов**: Вместо написания сложных запросов каждый раз, вы можете создать представление и использовать его как обычную таблицу.

2. **Безопасность данных**: Представления позволяют скрывать определенные столбцы или строки от пользователей. Вы можете предоставить доступ к представлению, не раскрывая всю информацию, содержащуюся в базовой таблице.

3. **Поддержка абстракции данных**: Представления могут скрывать сложные связи и детали хранения данных, предоставляя простой интерфейс для пользователя.

4. **Поддержка переноса данных**: Если структура таблицы меняется, представление можно обновить, не изменяя SQL-запросы, которые его используют.

### Синтаксис создания представления

Создание представления выполняется с помощью команды `CREATE VIEW`. Вот базовый синтаксис:

```sql
CREATE 
	VIEW view_name AS
SELECT 
	column1, 
	column2, 
	...
	column_n
FROM 
	table_name
WHERE 
	condition;
```

- **view_name**: Имя, которое вы присваиваете представлению.
- **SELECT ...**: Запрос, определяющий, какие данные будут отображаться в представлении.

### Пример 1: Простое представление

Предположим, у вас есть таблица `employees`, и вы хотите создать представление, которое отображает только имена и должности сотрудников.

```sql
CREATE 
	VIEW employee_view AS
SELECT 
	first_name, 
	last_name, 
	job_title
FROM 
	employees;
```

Теперь вы можете использовать `employee_view` как таблицу:

```sql
SELECT 
	* 
FROM 
	employee_view;
```

Этот запрос вернет имена и должности всех сотрудников.

### Пример 2: Представление с условием

Предположим, вы хотите создать представление для сотрудников, которые работают в отделе "Sales".

```sql
CREATE 
	VIEW sales_employees AS
SELECT 
	first_name, 
	last_name, 
	department
FROM 
	employees
WHERE 
	department = 'Sales';
```

Теперь, когда вы выполните запрос к представлению `sales_employees`, вы получите только тех сотрудников, которые работают в отделе продаж:

```sql
SELECT 
	* 
FROM 
	sales_employees;
```

### Обновляемые представления

Некоторые представления можно обновлять, то есть можно добавлять, обновлять или удалять данные в базовых таблицах через представление. Однако не все представления обновляемы. Представление должно удовлетворять определенным условиям для того, чтобы оно было обновляемым, например, оно не должно содержать группировку (`GROUP BY`), агрегацию (`SUM`, `COUNT` и т.д.), а также вложенные запросы.

Пример обновляемого представления:

```sql
CREATE 
	VIEW basic_employee_data AS
SELECT 
	first_name, 
	last_name, 
	job_title
FROM 
	employees;
```

Вы можете обновлять данные в базовой таблице через это представление:

```sql
UPDATE 
	basic_employee_data
SET 
	job_title = 'Manager'
WHERE 
	last_name = 'Smith';
```

### Удаление представления

Если вам больше не нужно представление, вы можете удалить его с помощью команды `DROP VIEW`:

```sql
DROP VIEW view_name;
```

---
### Create or replace view

In SQL, the `CREATE OR REPLACE VIEW` statement allows you to create a new view or replace an existing one with a new definition. Here’s a basic example of how to use it:

```sql
CREATE OR REPLACE VIEW example_view AS
SELECT column1, column2
FROM your_table
WHERE condition;
```

### Explanation:

- `CREATE OR REPLACE VIEW example_view AS`: This creates a new view named `example_view`, or replaces it if it already exists.
- `SELECT column1, column2`: Specifies the columns you want to include in the view.
- `FROM your_table`: Specifies the table from which to retrieve the data.
- `WHERE condition`: Applies any filters to the data.

### Example

Suppose you have a table called `employees` and you want to create a view that shows only employees in a specific department:

```sql
CREATE OR REPLACE VIEW sales_employees AS
SELECT employee_id, employee_name, department
FROM employees
WHERE department = 'Sales';
```

In this example, the view `sales_employees` will include only those employees whose department is 'Sales'.

### Drop and Altering views

To manage views in SQL, you use the `DROP VIEW` and `ALTER VIEW` statements. Here’s how they work:

### Dropping a View

The `DROP VIEW` statement removes a view from the database. The syntax is:

```sql
DROP VIEW view_name;
```

**Example:**

```sql
DROP VIEW sales_employees;
```

This command will delete the `sales_employees` view from the database. Be cautious with this operation as it cannot be undone.

---
### Altering a View

SQL doesn’t have a direct `ALTER VIEW` command like it does for tables. Instead, you use `CREATE OR REPLACE VIEW` to modify an existing view. This approach effectively updates the view with a new definition. 

**Example:**

If you want to update the `sales_employees` view to include an additional column, such as `hire_date`, you would do:

```sql
CREATE OR REPLACE VIEW sales_employees AS
SELECT employee_id, employee_name, department, hire_date
FROM employees
WHERE department = 'Sales';
```

This command replaces the existing `sales_employees` view with a new definition that includes the `hire_date` column.

### Summary

- **Drop a view**: Use `DROP VIEW view_name;`
- **Alter a view**: Use `CREATE OR REPLACE VIEW view_name AS ...` to redefine it.

---

## INSERT | UPDATE  | DELETE statements with views

When working with views in SQL, you can use `INSERT`, `UPDATE`, and `DELETE` statements to modify the underlying data through the view. However, there are some important considerations and limitations to keep in mind, as not all views are updatable. Here’s how to use these statements with views:

### 1. `INSERT` Statement

You can insert data into a view if the view is updatable and does not include columns that are derived from other tables or aggregate functions.

**Example:**

Assume you have a view called `active_employees` that displays employees from the `employees` table:

```sql
CREATE 
	VIEW active_employees AS
	SELECT 
		employee_id, 
		employee_name, 
		department
	FROM 
		employees
	WHERE 
		status = 'Active';
```

You can insert a new row into this view, which will be reflected in the `employees` table:

```sql
INSERT INTO 
	active_employees (
						employee_id, 
						employee_name, 
						department
	)
VALUES 
	(
		1234, 
		'John Doe', 
		'Sales'
	);
```

### 2. `UPDATE` Statement

Updating data through a view is possible if the view is updatable and the columns being updated are directly mapped to the base table.

**Example:**

Assuming the same `active_employees` view, you can update an employee’s department:

```sql
UPDATE 
	active_employees
SET 
	department = 'Marketing'
WHERE 
	employee_id = 1234;
```

This update will be reflected in the `employees` table where `employee_id` is 1234.

### 3. `DELETE` Statement

You can delete rows from a view if it is updatable and rows in the view correspond directly to rows in the base table.

**Example:**

To delete an employee through the `active_employees` view:

```sql
DELETE FROM 
	active_employees
WHERE 
	employee_id = 1234;
```

This command will delete the employee with `employee_id` 1234 from the `employees` table.

### Considerations

1. **Updatable Views**: Views are updatable if they meet certain criteria, such as:
   - Not using `GROUP BY`, `DISTINCT`, or aggregate functions.
   - Not involving joins that make it unclear where the data should be inserted or updated.
   - Not including expressions or functions in the column list.

2. **Read-Only Views**: Some views are read-only due to their complexity. For example, views that use aggregate functions or involve multiple tables with joins may not be updatable.

3. **Permissions**: Ensure that you have the necessary permissions to perform `INSERT`, `UPDATE`, or `DELETE` operations on the base tables through the view.

4. **Triggers**: If your view involves complex logic, you might need to implement triggers on the base tables to handle `INSERT`, `UPDATE`, or `DELETE` operations appropriately.

### Summary

- **`INSERT`**: Can add new rows to the underlying base table via the view.
- **`UPDATE`**: Can modify existing rows in the base table via the view.
- **`DELETE`**: Can remove rows from the base table via the view.

Always verify whether the view is updatable and test these operations to ensure they behave as expected in your specific database environment.

Когда вы выполняете `INSERT`, `UPDATE` или `DELETE` через представление (view) в SQL, вы фактически изменяете данные в базовых таблицах, из которых это представление создано. Это связано с тем, что представление — это всего лишь логическое представление данных, которое создается на основе одной или нескольких таблиц.

**Однако** есть некоторые ограничения и нюансы:

- Представление должно быть обновляемым, что значит, что SQL-движок должен иметь возможность корректно сопоставить изменение в представлении с изменением в базовой таблице.
- Некоторые представления, особенно те, которые используют агрегатные функции, `JOIN` операций, подзапросы или сложные выражения, могут быть не обновляемыми. В таких случаях изменения через представление невозможны.
- Если представление содержит такие конструкции, как `DISTINCT`, `GROUP BY`, `HAVING`, или подзапросы, это также может сделать его не обновляемым.

Но в общем случае, да, изменения, сделанные через представление, будут отражаться в базовых таблицах.

---
### Заключение

Представления в SQL — мощный инструмент для организации данных, повышения безопасности и упрощения взаимодействия с базой данных. Они предоставляют виртуальный слой, позволяющий абстрагировать и упрощать доступ к сложным данным.