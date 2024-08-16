Давайте рассмотрим пример с использованием тех же таблиц: `employees` (сотрудники) и `departments` (отделы). Предположим, у нас есть следующие данные:

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

Теперь выполним `LEFT OUTER JOIN` (или просто `LEFT JOIN`) для этих таблиц.

### SQL-запрос:
```sql
SELECT 
    employees.employee_id, 
    employees.name, 
    employees.department_id, 
    departments.department_name 
FROM 
    employees
LEFT JOIN 
	departments
	ON 
		employees.department_id = departments.department_id;
```

### Результат:
| employee_id | name     | department_id | department_name |
|-------------|----------|---------------|-----------------|
| 1           | Alice    | 10            | HR              |
| 2           | Bob      | 20            | IT              |
| 3           | Charlie  | NULL          | NULL            |
| 4           | David    | 20            | IT              |
| 5           | Eva      | 30            | Marketing       |

### Подробное объяснение:
1. **Соединение по совпадающим значениям**:
   - Таблица `employees` имеет столбец `department_id`, который связывает сотрудников с отделами.
   - Таблица `departments` также имеет столбец `department_id`.
   - Мы соединяем эти таблицы по `department_id`.

2. **Что делает LEFT JOIN**:
   - `LEFT JOIN` возвращает все строки из левой таблицы (`employees`), а также совпадающие строки из правой таблицы (`departments`).
   - Если нет совпадения в правой таблице, для соответствующих столбцов правой таблицы возвращается `NULL`.

3. **Результат**:
   - Строки, где `department_id` совпадает (например, 10, 20, 30), включают данные как из `employees`, так и из `departments`.
   - Строка с `employee_id` 3 (Charlie) в таблице `employees` не имеет соответствующих строк в таблице `departments`, поэтому для столбца `department_name` возвращается значение `NULL`.

### Итоги:
- В таблице `employees` есть сотрудник, который не имеет назначенного отдела (`department_id` = NULL), поэтому в результате для столбца `department_name` возвращается значение `NULL`.
- Таким образом, `LEFT JOIN` гарантирует, что все строки из левой таблицы (`employees`) будут включены в результат, даже если нет соответствующих строк в правой таблице (`departments`).