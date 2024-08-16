Давайте рассмотрим пример с использованием таблиц: `employees` (сотрудники) и `departments` (отделы). Предположим, у нас есть следующие данные:

### Таблица `employees`:
| employee_id | name     | department_id |
|-------------|----------|---------------|
| 1           | Alice    | 10            |
| 2           | Bob      | 20            |
| 3           | Charlie  | NULL          |
| 4           | David    | 20            |
| 5           | Eva      | 30            |

### Таблица `departments`:
| department_id | department_name |
|---------------|-----------------|
| 10            | HR              |
| 20            | IT              |
| 30            | Marketing       |
| 40            | Finance         |

Теперь выполним `FULL OUTER JOIN` (или просто `FULL JOIN`) для этих таблиц.

### SQL-запрос:
```sql
SELECT 
	employees.employee_id, 
	employees.name, 
	employees.department_id, 
	departments.department_name
FROM 
	employees
FULL OUTER JOIN 
	departments
	ON 
		employees.department_id = departments.department_id;
```

### Результат:
| employee_id | name    | department_id | department_name |
| ----------- | ------- | ------------- | --------------- |
| 1           | Alice   | 10            | HR              |
| 2           | Bob     | 20            | IT              |
| 4           | David   | 20            | IT              |
| 5           | Eva     | 30            | Marketing       |
| NULL        | NULL    | 40            | Finance         |
| 3           | Charlie | NULL          | NULL            |

### Подробное объяснение:
1. **Соединение по совпадающим значениям**:
   - Таблица `employees` имеет столбец `department_id`, который связывает сотрудников с отделами.
   - Таблица `departments` также имеет столбец `department_id`.
   - Мы соединяем эти таблицы по `department_id`.

2. **Что делает FULL OUTER JOIN**:
   - `FULL OUTER JOIN` возвращает все строки из обеих таблиц. Если есть совпадение, строки объединяются.
   - Если нет совпадения, для столбцов из таблицы, где нет соответствующих строк, возвращается значение `NULL`.

3. **Результат**:
   - Строки, где `department_id` совпадает (например, 10, 20, 30), включают данные как из `employees`, так и из `departments`.
   - Строка с `department_id` 40 в таблице `departments` не имеет соответствующих строк в таблице `employees`, поэтому для столбцов из таблицы `employees` возвращаются значения `NULL`.
   - Строка с `employee_id` 3 (Charlie) в таблице `employees` не имеет назначенного отдела (`department_id` = NULL), поэтому для столбца `department_name` возвращается значение `NULL`.

### Итоги:
- `FULL OUTER JOIN` гарантирует, что все строки из обеих таблиц будут включены в результат.
- Если нет совпадения, строки одной таблицы будут дополняться `NULL` для столбцов другой таблицы.
- Таким образом, результат содержит все строки из обеих таблиц с соответствующими значениями и `NULL` там, где нет совпадения.

### Примерный результат на основе данных:

```sql
------------------------------------------------------------
| employee_id | name     | department_id | department_name |
|-------------|----------|---------------|-----------------|
| 1           | Alice    | 10            | HR              |
| 2           | Bob      | 20            | IT              |
| 4           | David    | 20            | IT              |
| 5           | Eva      | 30            | Marketing       |
| NULL        | NULL     | 40            | Finance         |
| 3           | Charlie  | NULL          | NULL            |
------------------------------------------------------------
```

Этот пример показывает, что `FULL OUTER JOIN` объединяет все данные из обеих таблиц, заполняя отсутствующие совпадения значениями `NULL`.