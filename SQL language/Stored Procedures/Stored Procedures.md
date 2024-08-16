Хранимые процедуры (Stored Procedures) — это заранее написанные и сохраненные на сервере базы данных SQL скрипты, которые могут выполняться по запросу. Они позволяют организовать и повторно использовать сложные последовательности SQL операторов, объединяя их в один блок. Это не только упрощает администрирование базы данных, но и помогает оптимизировать производительность и безопасность.

### Преимущества хранимых процедур

1. **Повышение производительности**: 
   - Хранимые процедуры компилируются и оптимизируются при первом запуске, что снижает затраты на выполнение SQL-кода при последующих вызовах.
   - Они выполняются на сервере базы данных, что сокращает объем данных, передаваемых между сервером и клиентом.

2. **Повторное использование кода**:
   - Процедуры можно вызывать повторно, что уменьшает дублирование кода и упрощает поддержку.

3. **Безопасность**:
   - Хранимые процедуры могут ограничивать доступ к данным, позволяя пользователям выполнять только определенные действия без прямого доступа к таблицам.
   - Они также защищают от SQL-инъекций, так как код в процедуре уже проверен и защищен.

4. **Инкапсуляция логики**:
   - Бизнес-логику можно сосредоточить в хранимых процедурах, обеспечивая единый подход к обработке данных.

### Синтаксис создания хранимой процедуры

```sql
CREATE PROCEDURE имя_процедуры
    @параметр1 тип_данных,
    @параметр2 тип_данных OUTPUT
AS
BEGIN
    -- SQL код процедуры
END;
```

### Пример 1: Простая процедура для вставки данных

Предположим, у нас есть таблица `Employees`, и мы хотим создать процедуру для добавления нового сотрудника.

```sql
CREATE PROCEDURE AddEmployee
    @FirstName NVARCHAR(50),
    @LastName NVARCHAR(50),
    @BirthDate DATE,
    @HireDate DATE
AS
BEGIN
    INSERT INTO Employees (FirstName, LastName, BirthDate, HireDate)
    VALUES (@FirstName, @LastName, @BirthDate, @HireDate);
END;
```

Вызов процедуры:

```sql
EXEC AddEmployee @FirstName = 'Иван', @LastName = 'Иванов', @BirthDate = '1990-01-01', @HireDate = '2024-08-12';
```

### Пример 2: Процедура с возвращаемым значением

Создадим процедуру, которая возвращает количество сотрудников в таблице `Employees`.

```sql
CREATE PROCEDURE GetEmployeeCount
    @Count INT OUTPUT
AS
BEGIN
    SELECT @Count = COUNT(*)
    FROM Employees;
END;
```

Вызов процедуры с выводом результата:

```sql
DECLARE @TotalEmployees INT;
EXEC GetEmployeeCount @Count = @TotalEmployees OUTPUT;
PRINT 'Total Employees: ' + CAST(@TotalEmployees AS NVARCHAR(10));
```

### Пример 3: Процедура с обработкой ошибок

Добавим обработку ошибок в процедуру:

```sql
CREATE PROCEDURE SafeAddEmployee
    @FirstName NVARCHAR(50),
    @LastName NVARCHAR(50),
    @BirthDate DATE,
    @HireDate DATE
AS
BEGIN
    BEGIN TRY
        INSERT INTO Employees (FirstName, LastName, BirthDate, HireDate)
        VALUES (@FirstName, @LastName, @BirthDate, @HireDate);
    END TRY
    BEGIN CATCH
        PRINT 'Ошибка при добавлении сотрудника: ' + ERROR_MESSAGE();
    END CATCH;
END;
```

### Заключение

Хранимые процедуры — это мощный инструмент для разработки и администрирования баз данных, позволяющий улучшить производительность, безопасность и управляемость SQL-запросов. Их использование может существенно повысить эффективность работы с базой данных.

---

Stored Procedures are precompiled SQL statements stored in a database that can be executed as needed. They allow you to encapsulate complex logic in the database, making code more reusable, secure, and easier to maintain.

### Key Features and Benefits

1. **Improved Performance**:
   - **Precompilation**: Stored procedures are compiled and optimized by the database engine when they are first executed. This reduces the overhead of query parsing and optimization in subsequent calls.
   - **Reduced Network Traffic**: By bundling multiple SQL statements into a single procedure, you minimize the amount of data sent between the client and the server, improving overall performance.

2. **Reusability**:
   - **Code Reusability**: Once a procedure is created, it can be called multiple times with different parameters, reducing code duplication and simplifying maintenance.
   - **Centralized Logic**: Business logic can be centralized within stored procedures, ensuring consistency across different applications accessing the database.

3. **Security**:
   - **Access Control**: Users can be granted permission to execute a stored procedure without having direct access to the underlying tables. This enhances security by restricting the types of operations users can perform.
   - **SQL Injection Prevention**: Since stored procedures are precompiled and their parameters are passed separately, they provide a layer of protection against SQL injection attacks.

4. **Ease of Maintenance**:
   - **Simplified Maintenance**: When the logic in a stored procedure needs to be updated, you only need to modify the procedure itself, rather than updating multiple application codebases.
   - **Error Handling**: Stored procedures can include error handling logic, allowing for more robust and predictable application behavior.

5. **Transaction Control**:
   - **Transaction Management**: Stored procedures can include transaction control statements (e.g., `BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK`), allowing you to manage transactions within the procedure.

### Syntax and Structure

The basic structure of a stored procedure includes defining its name, parameters, and the SQL statements to be executed. Here’s the general syntax:

```sql
CREATE PROCEDURE procedure_name
    @parameter1 datatype,
    @parameter2 datatype OUTPUT
AS
BEGIN
    -- SQL statements
END;
```

### Example 1: Basic Insert Procedure

This procedure adds a new employee to the `Employees` table.

```sql
CREATE PROCEDURE AddEmployee
    @FirstName NVARCHAR(50),
    @LastName NVARCHAR(50),
    @BirthDate DATE,
    @HireDate DATE
AS
BEGIN
    INSERT INTO Employees (FirstName, LastName, BirthDate, HireDate)
    VALUES (@FirstName, @LastName, @BirthDate, @HireDate);
END;
```

To execute the procedure:

```sql
EXEC AddEmployee @FirstName = 'John', @LastName = 'Doe', @BirthDate = '1990-01-01', @HireDate = '2024-08-12';
```

### Example 2: Procedure with Output Parameter

This procedure calculates and returns the total number of employees.

```sql
CREATE PROCEDURE GetEmployeeCount
    @EmployeeCount INT OUTPUT
AS
BEGIN
    SELECT @EmployeeCount = COUNT(*)
    FROM Employees;
END;
```

To call the procedure and retrieve the output:

```sql
DECLARE @TotalEmployees INT;
EXEC GetEmployeeCount @EmployeeCount = @TotalEmployees OUTPUT;
PRINT 'Total Employees: ' + CAST(@TotalEmployees AS NVARCHAR(10));
```

### Example 3: Error Handling in a Procedure

This procedure includes error handling using `TRY...CATCH`.

```sql
CREATE PROCEDURE SafeAddEmployee
    @FirstName NVARCHAR(50),
    @LastName NVARCHAR(50),
    @BirthDate DATE,
    @HireDate DATE
AS
BEGIN
    BEGIN TRY
        INSERT INTO Employees (FirstName, LastName, BirthDate, HireDate)
        VALUES (@FirstName, @LastName, @BirthDate, @HireDate);
    END TRY
    BEGIN CATCH
        PRINT 'Error: ' + ERROR_MESSAGE();
    END CATCH;
END;
```

### Example 4: Procedure with Transaction Control

This procedure demonstrates how to use transaction control within a stored procedure.

```sql
CREATE PROCEDURE TransferFunds
    @FromAccountID INT,
    @ToAccountID INT,
    @Amount DECIMAL(10, 2)
AS
BEGIN
    BEGIN TRANSACTION;

    BEGIN TRY
        UPDATE Accounts
        SET Balance = Balance - @Amount
        WHERE AccountID = @FromAccountID;

        UPDATE Accounts
        SET Balance = Balance + @Amount
        WHERE AccountID = @ToAccountID;

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        PRINT 'Transaction Failed: ' + ERROR_MESSAGE();
    END CATCH;
END;
```

### Summary

Stored Procedures offer numerous benefits, including improved performance, security, and code reusability. By encapsulating logic within the database, they also simplify application development and maintenance.
