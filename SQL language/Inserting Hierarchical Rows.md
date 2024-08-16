Вставка иерархических строк в базу данных - это процесс добавления данных, которые имеют взаимосвязанные отношения, например, родительские и дочерние записи. Часто это делается в таблицах, где существует связь "один ко многим". Рассмотрим этот процесс на примере таблиц `departments` и `employees`, где каждая запись в `employees` ссылается на запись в `departments`.

### Пример иерархической структуры

Допустим, у нас есть две таблицы:

1. **departments** (отделы):
   - `department_id` (уникальный идентификатор отдела)
   - `department_name` (название отдела)

2. **employees** (сотрудники):
   - `employee_id` (уникальный идентификатор сотрудника)
   - `employee_name` (имя сотрудника)
   - `department_id` (идентификатор отдела, внешний ключ)

### Создание таблиц

Сначала создадим таблицы:

```sql
CREATE TABLE departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id) ON DELETE CASCADE
);
```

### Вставка данных в иерархические таблицы

1. **Вставка данных в родительскую таблицу**:

   Мы начнем с вставки данных в таблицу `departments`:

   ```sql
   INSERT INTO departments (department_name) VALUES ('HR');
   INSERT INTO departments (department_name) VALUES ('Finance');
   INSERT INTO departments (department_name) VALUES ('IT');
   ```

2. **Получение идентификаторов вставленных записей**:

   После вставки данных в родительскую таблицу нам нужно получить идентификаторы этих записей, чтобы использовать их в дочерней таблице. Это можно сделать с помощью функции `LAST_INSERT_ID()`, если вставка идет одна за другой, или с помощью запроса, если вставка была выполнена массово.

   ```sql
   SELECT department_id FROM departments WHERE department_name = 'HR';
   SELECT department_id FROM departments WHERE department_name = 'Finance';
   SELECT department_id FROM departments WHERE department_name = 'IT';
   ```

3. **Вставка данных в дочернюю таблицу**:

   Теперь мы можем вставить данные в таблицу `employees`, используя полученные идентификаторы отделов:

   ```sql
   -- Получаем идентификаторы отделов
   SET @HR_id = (SELECT department_id FROM departments WHERE department_name = 'HR');
   SET @Finance_id = (SELECT department_id FROM departments WHERE department_name = 'Finance');
   SET @IT_id = (SELECT department_id FROM departments WHERE department_name = 'IT');

   -- Вставляем сотрудников
   INSERT INTO employees (employee_name, department_id) VALUES ('Alice', @HR_id);
   INSERT INTO employees (employee_name, department_id) VALUES ('Bob', @Finance_id);
   INSERT INTO employees (employee_name, department_id) VALUES ('Charlie', @IT_id);
   ```

### Полный пример скрипта

```sql
-- Создание таблиц
CREATE TABLE departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id) ON DELETE CASCADE
);

-- Вставка данных в таблицу departments
INSERT INTO departments (department_name) VALUES ('HR');
INSERT INTO departments (department_name) VALUES ('Finance');
INSERT INTO departments (department_name) VALUES ('IT');

-- Получение идентификаторов отделов
SET @HR_id = (SELECT department_id FROM departments WHERE department_name = 'HR');
SET @Finance_id = (SELECT department_id FROM departments WHERE department_name = 'Finance');
SET @IT_id = (SELECT department_id FROM departments WHERE department_name = 'IT');

-- Вставка данных в таблицу employees
INSERT INTO employees (employee_name, department_id) VALUES ('Alice', @HR_id);
INSERT INTO employees (employee_name, department_id) VALUES ('Bob', @Finance_id);
INSERT INTO employees (employee_name, department_id) VALUES ('Charlie', @IT_id);
```

### Важные моменты:

1. **Внешние ключи**: Они помогают обеспечить целостность данных. В данном примере, внешний ключ в таблице `employees` гарантирует, что каждый сотрудник ссылается на существующий отдел.

2. **ON DELETE CASCADE**: Это условие в определении внешнего ключа позволяет автоматически удалять все записи из таблицы `employees`, связанные с удаляемым отделом.

3. **Последовательная вставка данных**: Сначала данные вставляются в родительскую таблицу (`departments`), затем в дочернюю таблицу (`employees`).

Следуя этим шагам, вы можете вставлять иерархические данные в ваши таблицы, соблюдая все необходимые ограничения и зависимости.

[[Пользовательские и сеансовые переменные]]