Давайте рассмотрим пример с использованием рекурсивного соединения (recursive join) в SQL, что часто применяется для работы с иерархическими данными, такими как структура сотрудников и их менеджеров.

### Пример:
Предположим, у нас есть таблица `employees`, в которой хранятся данные о сотрудниках, включая их менеджеров. Вот структура таблицы и данные:

### Таблица `employees`:
| employee_id | name     | manager_id |
|-------------|----------|------------|
| 1           | Alice    | NULL       |
| 2           | Bob      | 1          |
| 3           | Charlie  | 2          |
| 4           | David    | 2          |
| 5           | Eva      | 1          |
| 6           | Frank    | 3          |

### Задача:
Найти иерархию сотрудников и их менеджеров до верхнего уровня, то есть показать цепочку подчиненности для каждого сотрудника.

### Использование CTE для рекурсивного соединения:
SQL Server, PostgreSQL и другие СУБД поддерживают Common Table Expressions (CTE), которые можно использовать для рекурсивного соединения.

### SQL-запрос:
```sql
WITH RECURSIVE EmployeeHierarchy AS (
    -- Anchor member: начальный запрос для получения сотрудников без менеджера
    SELECT 
        employee_id, 
        name, 
        manager_id, 
        name AS hierarchy
    FROM 
        employees
    WHERE 
        manager_id IS NULL
    
    UNION ALL
    
    -- Recursive member: рекурсивный запрос для получения сотрудников, подчиненных менеджерам
    SELECT 
        e.employee_id, 
        e.name, 
        e.manager_id, 
        eh.hierarchy || ' -> ' || e.name AS hierarchy
    FROM 
        employees e
    INNER JOIN 
        EmployeeHierarchy eh 
    ON 
        e.manager_id = eh.employee_id
)
SELECT 
    employee_id, 
    name, 
    manager_id, 
    hierarchy
FROM 
    EmployeeHierarchy;
```

### Объяснение:
1. **CTE (Common Table Expression)**: Мы используем `WITH RECURSIVE` для создания временной таблицы `EmployeeHierarchy`.
   
2. **Anchor member**: 
    - Этот запрос выбирает сотрудников без менеджера (менеджеры верхнего уровня).
    - В нашем случае это Alice, так как у нее `manager_id` = NULL.
    - Мы также создаем начальное значение для столбца `hierarchy`, которое будет содержать цепочку подчиненности.

3. **Recursive member**: 
    - Этот запрос объединяет сотрудников с их менеджерами, основываясь на значении `manager_id`.
    - Рекурсивный запрос объединяет каждого сотрудника с его менеджером и обновляет значение `hierarchy`, добавляя имя текущего сотрудника к иерархии его менеджера.

4. **Рекурсия**: Процесс продолжается, пока все сотрудники не будут включены в иерархию.

### Результат:
| employee_id | name     | manager_id | hierarchy                |
|-------------|----------|------------|--------------------------|
| 1           | Alice    | NULL       | Alice                    |
| 2           | Bob      | 1          | Alice -> Bob             |
| 3           | Charlie  | 2          | Alice -> Bob -> Charlie  |
| 4           | David    | 2          | Alice -> Bob -> David    |
| 5           | Eva      | 1          | Alice -> Eva             |
| 6           | Frank    | 3          | Alice -> Bob -> Charlie -> Frank |

### Подробное объяснение:

1. **Alice**:
   - `employee_id` = 1, `manager_id` = NULL. Она находится на верхнем уровне иерархии.
   - Иерархия: Alice.

2. **Bob**:
   - `employee_id` = 2, `manager_id` = 1. Его менеджер - Alice.
   - Иерархия: Alice -> Bob.

3. **Charlie**:
   - `employee_id` = 3, `manager_id` = 2. Его менеджер - Bob.
   - Иерархия: Alice -> Bob -> Charlie.

4. **David**:
   - `employee_id` = 4, `manager_id` = 2. Его менеджер - Bob.
   - Иерархия: Alice -> Bob -> David.

5. **Eva**:
   - `employee_id` = 5, `manager_id` = 1. Ее менеджер - Alice.
   - Иерархия: Alice -> Eva.

6. **Frank**:
   - `employee_id` = 6, `manager_id` = 3. Его менеджер - Charlie.
   - Иерархия: Alice -> Bob -> Charlie -> Frank.

Таким образом, с помощью рекурсивного соединения мы можем создать иерархическую структуру, показывающую цепочку подчиненности сотрудников в организации.