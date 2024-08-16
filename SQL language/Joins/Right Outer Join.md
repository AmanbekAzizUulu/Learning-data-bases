Давайте рассмотрим пример с использованием двух таблиц: `employees` (сотрудники) и `departments` (отделы). Предположим, у нас есть следующие данные:

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

Теперь выполним `RIGHT OUTER JOIN` (или просто `RIGHT JOIN`) для этих таблиц.

### SQL-запрос:
```sql
SELECT 
	employees.employee_id, 
	employees.name, 
	departments.department_id, 
	departments.department_name
FROM 
	employees
RIGHT JOIN 
	departments
	ON 
		employees.department_id = departments.department_id;
```

### Результат:
| employee_id | name     | department_id | department_name |
|-------------|----------|---------------|-----------------|
| 1           | Alice    | 10            | HR              |
| 2           | Bob      | 20            | IT              |
| 4           | David    | 20            | IT              |
| 5           | Eva      | 30            | Marketing       |
| NULL        | NULL     | 40            | Finance         |

### Подробное объяснение:
1. **Соединение по совпадающим значениям**:
   - Таблица `employees` имеет столбец `department_id`, который связывает сотрудников с отделами.
   - Таблица `departments` также имеет столбец `department_id`.
   - Мы соединяем эти таблицы по `department_id`.

2. **Что делает RIGHT JOIN**:
   - `RIGHT JOIN` возвращает все строки из правой таблицы (`departments`), а также совпадающие строки из левой таблицы (`employees`).
   - Если нет совпадения в левой таблице, для соответствующих столбцов левой таблицы возвращается `NULL`.

3. **Результат**:
   - Строки, где `department_id` совпадает (например, 10, 20, 30), включают данные как из `employees`, так и из `departments`.
   - Строка с `department_id` 40 в таблице `departments` не имеет соответствующих строк в таблице `employees`, поэтому для столбцов из таблицы `employees` возвращаются значения `NULL`.

### Итоги:
- В таблице `employees` нет сотрудников, которые принадлежат отделу с `department_id` 40, поэтому в результате для этих сотрудников возвращаются `NULL`.
- Таким образом, `RIGHT JOIN` гарантирует, что все строки из правой таблицы (`departments`) будут включены в результат, даже если нет соответствующих строк в левой таблице (`employees`).