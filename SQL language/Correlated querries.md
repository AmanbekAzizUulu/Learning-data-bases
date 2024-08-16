Correlated subqueries are a type of subquery where the inner query references a column from the outer query. Unlike a regular subquery, which is executed only once, a correlated subquery is executed once for each row processed by the outer query. This makes correlated subqueries more powerful and flexible but can also make them more complex and potentially less efficient.

### Basic Syntax

The basic syntax of a correlated subquery is:

```sql
SELECT 
	column1,
	column2,
	...
FROM
	table1 AS t1
WHERE 
	column1 [operator] 
		(
			SELECT 
				column1
            FROM 
	            table2 AS t2
            WHERE 
	            t1.columnX = t2.columnY
	    );
```

Here, the inner query (`SELECT column1 FROM table2 AS t2 WHERE t1.columnX = t2.columnY`) is executed for each row of the outer query (`SELECT column1, column2, ... FROM table1 AS t1`).

### Detailed Example

Consider two tables, `employees` and `departments`:

- `employees`: Contains information about employees, including their department ID.
- `departments`: Contains information about departments.

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10, 2),
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

#### Find Employees Who Earn More Than the Average Salary in Their Department

Let's write a query to find all employees who earn more than the average salary of their department. This is a typical scenario where a correlated subquery is useful.

```sql
SELECT 
	e.employee_id, 
	e.employee_name, 
	e.salary, 
	e.department_id
FROM 
	employees AS e
WHERE 
	e.salary > 
		(
			SELECT 
				AVG(e2.salary)
            FROM 
	            employees AS e2
            WHERE 
	            e2.department_id = e.department_id
	    );
```

### Explanation

1. **Outer Query**:

```sql
SELECT 
	e.employee_id, 
	e.employee_name, 
	e.salary, 
	e.department_id
FROM 
	employees AS e
```

- This part selects the `employee_id`, `employee_name`, `salary`, and `department_id` from the `employees` table and aliases it as `e`.

2. **Correlated Subquery**:

```sql
WHERE 
	e.salary > 
		(
			SELECT 
				AVG(e2.salary)
            FROM 
	            employees AS e2
            WHERE 
	            e2.department_id = e.department_id
	    );
```

- `SELECT AVG(e2.salary) FROM employees AS e2 WHERE e2.department_id = e.department_id`: This subquery calculates the average salary for each department. It uses the alias `e2` for the `employees` table.
- `e2.department_id = e.department_id`: This condition correlates the subquery with the outer query, ensuring that the average salary is calculated for the department of the current employee in the outer query.
- `e.salary > ...`: This condition in the outer query checks if the current employee's salary is greater than the average salary of their department.

### How It Works

1. For each row in the outer query (each employee), the subquery calculates the average salary for that employee's department.
2. The outer query then checks if the employee's salary is greater than the calculated average salary.
3. If the condition is true, the employee's details are included in the result set.

### Performance Considerations

Correlated subqueries can be less efficient than regular subqueries because the inner query is executed multiple times, once for each row processed by the outer query. Here are a few tips to improve performance:

1. **Indexes**: Ensure that appropriate indexes are created on the columns used in the join conditions and WHERE clauses.
2. **Rewrite as JOIN**: Sometimes, a correlated subquery can be rewritten as a JOIN, which can be more efficient.

### Example of Rewriting as JOIN

The previous example can be rewritten using a JOIN:

```sql
SELECT 
	e.employee_id, 
	e.employee_name, 
	e.salary, 
	e.department_id
FROM 
	employees AS e
JOIN 
	(
		SELECT 
			department_id, 
			AVG(salary) AS avg_salary
	    FROM 
		    employees
      GROUP BY 
	      department_id
	) AS avg_salaries
	ON 
		e.department_id = avg_salaries.department_id
WHERE 
	e.salary > avg_salaries.avg_salary;
```

### Explanation of Rewritten Query

1. **Subquery**:
```sql
SELECT 
	department_id, 
	AVG(salary) AS avg_salary
FROM 
	employees
GROUP BY 
	department_id
```
- This subquery calculates the average salary for each department and is executed only once.

2. **JOIN**:
```sql
JOIN 
	(subquery) AS avg_salaries
ON 
	e.department_id = avg_salaries.department_id
```
- The outer query joins the `employees` table with the result of the subquery based on the `department_id`.

3. **WHERE Clause**:
```sql
WHERE 
	e.salary > avg_salaries.avg_salary
```
- This condition checks if the employee's salary is greater than the average salary of their department.

### When to Use Correlated Subqueries

- When the logic requires comparison or aggregation based on each row of the outer query.
- When rewriting as a JOIN is not straightforward or does not improve performance.

### Conclusion

Correlated subqueries are a powerful tool in SQL for performing complex comparisons and aggregations that depend on each row of the outer query. While they can be less efficient due to multiple executions of the inner query, they provide a clear and often straightforward way to express complex conditions. By understanding how correlated subqueries work and when to use them, you can write more effective and efficient SQL queries.\

---
# Коррелирующие запросы

Коррелирующие запросы (correlated subqueries) в SQL — это подзапросы, которые зависят от внешнего запроса. Это значит, что подзапрос использует значения из внешнего запроса, и подзапрос выполняется один раз для каждой строки внешнего запроса.

### Пример

Рассмотрим таблицы `employees` и `departments`:

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10, 2)
);

CREATE TABLE departments (
    department_id INT,
    department_name VARCHAR(50)
);
```

Допустим, мы хотим найти сотрудников, которые зарабатывают больше среднего заработка в их отделе. Это можно сделать с помощью коррелирующего подзапроса:

```sql
SELECT 
	employee_name, 
	salary
FROM 
	employees as outer_empl
WHERE 
	salary > 
		(
		    SELECT 
			    AVG(salary)
		    FROM 
			    employees
		    WHERE 
			    department_id = outer_empl.department_id
		);
```

### Как это работает

1. **Внешний запрос**: `SELECT employee_name, salary FROM employees as outer_empl
2. **Подзапрос**: `SELECT AVG(salary) FROM employees WHERE department_id = e.department_id`

Подзапрос выполняется для каждой строки из внешнего запроса. Для каждого сотрудника он вычисляет среднюю зарплату в его отделе и проверяет, больше ли его зарплата этого значения.

### Плюсы и Минусы

**Плюсы**:
- Мощные возможности для создания сложных условий.
- Удобны для написания запросов, которые требуют сравнения данных в пределах одной и той же таблицы.

**Минусы**:
- Могут быть менее эффективны по сравнению с другими подходами, такими как соединения (JOIN), особенно при работе с большими объемами данных.
- Сложнее для понимания и отладки.

### Оптимизация

Для улучшения производительности можно рассмотреть использование индексов или переписывание запроса с использованием соединений (JOIN).

```sql
SELECT 
	outer_empl.employee_name, 
	outer_empl.salary
FROM 
	employees as outer_empl
JOIN 
	(
	    SELECT 
		    department_id, 
		    AVG(salary) AS avg_salary
	    FROM 
		    employees
	    GROUP BY 
		    department_id
	) as inner_empl 
	ON 
		outer_empl.department_id = inner_empl.department_id
WHERE 
	outer_empl.salary > inner_empl.avg_salary;
```

Этот подход может быть более эффективным, так как избегает выполнения подзапроса для каждой строки внешнего запроса.

 Давай рассмотрим, как работает этот запрос пошагово:

```sql
SELECT 
	outer_empl.employee_name, 
	outer_empl.salary
FROM 
	employees as outer_empl
JOIN 
	(
	    SELECT 
		    department_id, 
		    AVG(salary) AS avg_salary
	    FROM 
		    employees
	    GROUP BY 
		    department_id
	) as inner_empl 
	ON 
		outer_empl.department_id = inner_empl.department_id
WHERE 
	outer_empl.salary > inner_empl.avg_salary;
```

### Этапы выполнения запроса

1. **Подзапрос (Вложенный запрос)**:

```sql
SELECT 
	department_id, 
	AVG(salary) AS avg_salary
FROM 
	employees
GROUP BY 
	department_id
```

 **Этап 1**: 
 - Из таблицы `employees` выбираются столбцы `department_id` и средняя зарплата `AVG(salary)`.
 
**Этап 2**: 
- Выполняется группировка по `department_id`, чтобы вычислить среднюю зарплату для каждого отдела.

**Результат**: 
- Подзапрос возвращает набор данных, содержащий идентификаторы отделов и соответствующие средние зарплаты.

Пример результата подзапроса:
```
    department_id | avg_salary
    --------------|-----------
    1             | 5000
    2             | 6000
    3             | 7000
```

2. **Соединение (JOIN)**:
```sql
    FROM 
	    employees as outer_empl
    JOIN (
        ...
    ) as inner_empl 
    ON 
	    outer_empl.department_id = inner_empl .department_id
```

- **Этап 3**: Результат подзапроса (`e2`) соединяется с основной таблицей `employees` (`e1`) по `department_id`.
    - **Результат**: Создается новый набор данных, где каждая строка из `employees` соответствует строке из подзапроса по идентификатору отдела.

3. **Фильтрация (WHERE)**:
    ```sql
    WHERE e1.salary > e2.avg_salary;
    ```

**Этап 4**: 
- Применяется условие фильтрации, чтобы выбрать только те строки, где зарплата сотрудника (`e1.salary`) больше средней зарплаты в его отделе (`e2.avg_salary`).


4. **Выборка (SELECT)**:
```sql
    SELECT 
	    outer_empl.employee_name, 
	    outer_empl.salary
```

 **Этап 5**: 
 - Из оставшихся строк выбираются только `employee_name` и `salary`.

### Полный процесс
1. **Выполняется подзапрос**:
    - Вычисляется средняя зарплата для каждого отдела.
2. **Соединение**:
    - Основная таблица `employees` соединяется с результатом подзапроса по `department_id`.
3. **Фильтрация**:
    - Выбираются только те сотрудники, чья зарплата выше средней зарплаты в их отделе.
4. **Финальная выборка**:
    - Из выбранных строк извлекаются имена сотрудников и их зарплаты.

### Пример работы

Если у нас есть данные:

`employees` таблица:
```
employee_id | employee_name | department_id | salary
------------|---------------|---------------|-------
1           | Alice         | 1             | 5500
2           | Bob           | 1             | 4500
3           | Carol         | 2             | 6500
4           | Dave          | 2             | 6000
5           | Eve           | 3             | 7500
6           | Frank         | 3             | 6800
```

После выполнения подзапроса и соединения, получаем:

```
employee_id | employee_name | department_id | salary | avg_salary
------------|---------------|---------------|--------|-----------
1           | Alice         | 1             | 5500   | 5000
2           | Bob           | 1             | 4500   | 5000
3           | Carol         | 2             | 6500   | 6250
4           | Dave          | 2             | 6000   | 6250
5           | Eve           | 3             | 7500   | 7150
6           | Frank         | 3             | 6800   | 7150
```

После фильтрации остаются только строки:

```
employee_name | salary
--------------|-------
Alice         | 5500
Carol         | 6500
Eve           | 7500
```

Надеюсь, это помогло понять, как работает запрос! Если есть дополнительные вопросы или что-то нужно уточнить, дай знать.