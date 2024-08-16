`CASE` в SQL — это выражение, которое позволяет вам выполнять условную логику в SQL-запросах. Оно работает по принципу `if-else` конструкций в языках программирования. `CASE` может использоваться в SELECT-запросах, а также в предложениях `WHERE`, `ORDER BY`, `HAVING` и других частях SQL.

### Синтаксис `CASE`

`CASE` имеет два основных варианта использования:

1. **Простой синтаксис:**
    ```sql
    CASE expression
        WHEN value_1 THEN result_1
        WHEN value_2 THEN result_2
        ...
        ELSE result_n
    END
    ```

2. **Поиск условий:**
    ```sql
    CASE
        WHEN condition_1 THEN result_1
        WHEN condition_2 THEN result_2
        ...
        ELSE result_n
    END
    ```

### Пример 1: Простой синтаксис
Предположим, у вас есть таблица `employees` с колонкой `job_title`, и вы хотите вывести категорию для каждой должности:

```sql
SELECT 
    employee_id,
    job_title,
    CASE job_title
        WHEN 'Manager' THEN 'Leadership'
        WHEN 'Developer' THEN 'Technical'
        WHEN 'Salesperson' THEN 'Sales'
        ELSE 'Other'
    END AS job_category
FROM employees;
```

**Объяснение:**
- `CASE job_title` проверяет значение в колонке `job_title`.
- Если оно равно `'Manager'`, то результат будет `'Leadership'`.
- Если значение равно `'Developer'`, результат будет `'Technical'`.
- Если значение равно `'Salesperson'`, результат будет `'Sales'`.
- Если ни одно из условий не выполнено, будет возвращено значение `'Other'`.

### Пример 2: Поиск условий
Теперь предположим, что вы хотите классифицировать сотрудников по их зарплате:

```sql
SELECT 
    employee_id,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'High Salary'
        WHEN salary >= 50000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END AS salary_category
FROM employees;
```

**Объяснение:**
- В этом случае `CASE` не принимает выражение после слова `CASE`, а сразу проверяет условия.
- Если `salary >= 100000`, результат будет `'High Salary'`.
- Если `salary >= 50000`, но меньше 100000, результат будет `'Medium Salary'`.
- Если ни одно из условий не выполнено (то есть зарплата меньше 50000), результатом будет `'Low Salary'`.

### Пример 3: Использование `CASE` в `ORDER BY`
Вы можете использовать `CASE` в `ORDER BY`, чтобы управлять порядком сортировки на основе сложных условий:

```sql
SELECT 
    employee_id,
    department_id,
    salary
FROM 
    employees
ORDER BY 
    CASE 
        WHEN department_id = 1 THEN salary
        ELSE 0
    END DESC,
    employee_id;
```

**Объяснение:**
- В данном запросе строки сортируются по зарплате (`salary`), если `department_id = 1`. Все остальные строки сортируются по `employee_id`.

### Пример 4: Использование `CASE` в `WHERE`
Вы можете использовать `CASE` в условиях фильтрации, хотя чаще всего это делается с использованием стандартных условий:

```sql
SELECT 
    employee_id,
    salary
FROM 
    employees
WHERE 
    CASE 
        WHEN department_id = 1 THEN salary > 50000
        WHEN department_id = 2 THEN salary > 60000
        ELSE salary > 30000
    END;
```

**Объяснение:**
- В этом примере условие в `WHERE` будет проверять разные условия для разных департаментов. Если сотрудник из департамента 1, то он должен зарабатывать больше 50000, если из департамента 2 — больше 60000, иначе — больше 30000.

### Заключение
`CASE` — это мощный инструмент для выполнения условной логики прямо внутри SQL-запросов. Он позволяет гибко управлять результатами запросов, сортировкой, фильтрацией и даже агрегированием данных.