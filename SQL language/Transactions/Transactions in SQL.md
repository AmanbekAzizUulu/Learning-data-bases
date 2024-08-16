Transactions in SQL are a crucial concept for managing data integrity and consistency within a database. They allow a series of SQL operations to be executed as a single unit of work, ensuring that either all operations succeed or none do, thus maintaining the database's integrity.

### **Key Concepts of Transactions**

1. **Atomicity**
2. **Consistency**
3. **Isolation**
4. **Durability**

These four principles are collectively known as the **ACID** properties.

### **1. Atomicity**

**Definition:**
Atomicity ensures that a transaction is an indivisible unit of work. Either all operations within the transaction are completed successfully, or none are applied.

**Example:**
Suppose you are transferring money from one bank account to another. This involves two operations:
- Deducting money from the source account
- Adding money to the target account

Atomicity ensures that either both operations are completed, or neither is. If the deduction succeeds but the addition fails, the transaction is rolled back, and the deduction is undone.

**SQL Example:**
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;
```

![[errors_introduced_without_transaction.png]]

![[atomic_transaction_prevents_error.png]]
### **2. Consistency**

**Definition:**
Consistency ensures that a transaction brings the database from one valid state to another. The database must always remain in a valid state before and after a transaction.

**Example:**
Continuing with the money transfer example, consistency ensures that no money is created or lost during the transaction. If the initial state was valid, the final state must also be valid (e.g., the total balance in the system must be consistent).

**SQL Example:**
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
-- Check consistency condition
IF (SELECT balance FROM accounts WHERE account_id = 1) < 0 THEN
    ROLLBACK;
ELSE
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
    COMMIT;
END IF;
```

### **3. Isolation**

**Definition:**
Isolation ensures that the operations of a transaction are isolated from other transactions. The intermediate state of a transaction is not visible to other transactions until it is committed.

**Isolation Levels:**
- **Read Uncommitted:** Allows dirty reads; one transaction may see uncommitted changes made by another transaction.
- **Read Committed:** Prevents dirty reads; a transaction only sees committed changes.
- **Repeatable Read:** Ensures that if a transaction reads a record, it will see the same data if it reads again within the same transaction.
- **Serializable:** Ensures complete isolation; transactions are executed in such a way that the result is the same as if transactions were executed serially.

**SQL Example (Setting Isolation Level):**
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
-- Operations here
COMMIT;
```

### **4. Durability**

**Definition:**
Durability ensures that once a transaction is committed, its changes are permanent and will survive any subsequent system failures.

**Example:**
Once the money transfer transaction is committed, the changes to the account balances are saved to disk and will not be lost, even if the database crashes immediately after.

**SQL Example:**
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;
-- Even if the system crashes after COMMIT, the changes are durable.
```

### **Using Transactions in SQL**

#### **Basic Transaction Commands**

- **`START TRANSACTION`**: Begins a new transaction.
- **`COMMIT`**: Saves all changes made during the transaction.
- **`ROLLBACK`**: Undoes all changes made during the transaction.

**Example:**

```sql
START TRANSACTION;
UPDATE orders SET status = 'Processed' WHERE order_id = 123;
INSERT INTO order_log (order_id, action) VALUES (123, 'Processed');
-- If an error occurs, rollback the transaction
ROLLBACK;
-- If no error occurs, commit the transaction
COMMIT;
```

### **Error Handling**

Transactions should handle errors gracefully to maintain consistency and reliability. If an error occurs, a `ROLLBACK` should be issued to undo the changes.

**Example with Error Handling:**

```sql
START TRANSACTION;
BEGIN
    -- Operation 1
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

    -- Operation 2
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

    -- If no errors, commit
    COMMIT;
EXCEPTION
    -- On error, rollback
    ROLLBACK;
END;
```

### **Transactional Control in SQL**

In SQL, the control of transactions involves:
- **Defining transactions explicitly**: Use `START TRANSACTION`, `COMMIT`, and `ROLLBACK`.
- **Setting isolation levels**: Use `SET TRANSACTION ISOLATION LEVEL`.
- **Ensuring durability and consistency**: Properly commit or rollback transactions based on success or failure.

### **Best Practices**

1. **Keep Transactions Short**: To minimize locking and improve performance.
2. **Use Proper Isolation Levels**: Choose the appropriate isolation level based on the requirements of your application.
3. **Handle Errors**: Ensure transactions are properly rolled back in case of errors to maintain data integrity.
4. **Test Transactions**: Thoroughly test transactions to ensure they handle all edge cases and maintain consistency.

Transactions are essential for maintaining the integrity and consistency of your database, especially in environments with concurrent access and complex data manipulations. Understanding and implementing the ACID properties effectively is crucial for reliable database management.

---

Транзакции в SQL используются для группировки одного или нескольких операций в единое логическое действие, которое может быть успешно выполнено или отменено целиком, если что-то пойдет не так. Ниже приведены примеры использования транзакций в SQL:

### Пример 1: Простая транзакция с COMMIT и ROLLBACK
Этот пример показывает, как можно использовать транзакцию для вставки данных в таблицу, и как откатить изменения в случае ошибки.

```sql
-- Начало транзакции
start transaction;

-- Вставка данных в таблицу customers
insert into customers (customer_id, customer_name, balance)
values (1, 'John Doe', 1000);

-- Вставка данных в таблицу orders
insert into orders (order_id, customer_id, order_date)
values (101, 1, '2024-08-14');

-- Если все операции прошли успешно, фиксируем транзакцию
commit;

-- Если произошла ошибка, отменяем все изменения
rollback;
```

### Пример 2: Транзакция с проверкой условий
Этот пример показывает, как транзакция может быть использована для выполнения проверок перед внесением изменений в базу данных.

```sql
-- Начало транзакции
start transaction;

-- Проверка остатка на счете перед списанием
declare @balance decimal(10, 2);
select @balance = balance from customers where customer_id = 1;

if @balance >= 500
begin
    -- Списание средств
    update customers
    set balance = balance - 500
    where customer_id = 1;

    -- Добавление записи о списании в таблицу транзакций
    insert into transactions (customer_id, amount, transaction_date)
    values (1, -500, getdate());

    -- Фиксируем транзакцию
    commit;
end
else
begin
    -- Недостаточно средств, откатываем транзакцию
    rollback;
end;
```

### Пример 3: Транзакция с вложенными операциями
В этом примере транзакция охватывает несколько операций обновления разных таблиц.

```sql
-- Начало транзакции
start transaction;

-- Обновление баланса у отправителя
update accounts
set balance = balance - 200
where account_id = 1;

-- Обновление баланса у получателя
update accounts
set balance = balance + 200
where account_id = 2;

-- Вставка записи о переводе
insert into transfers (from_account, to_account, amount, transfer_date)
values (1, 2, 200, getdate());

-- Фиксируем транзакцию
commit;
```

### Пример 4: Транзакция с обработкой ошибок
Этот пример показывает, как можно использовать транзакцию для обработки ошибок и откатывания изменений.

```sql
-- Начало транзакции
start transaction;

begin try
    -- Обновление данных
    update products
    set quantity = quantity - 1
    where product_id = 123;

    -- Вставка данных в таблицу sales
    insert into sales (product_id, sale_date, quantity)
    values (123, getdate(), 1);

    -- Фиксируем транзакцию, если всё прошло успешно
    commit;
end try
begin catch
    -- В случае ошибки откатываем транзакцию
    rollback;

    -- Обработка ошибки
    declare @error_message nvarchar(4000) = error_message();
    print 'Error occurred: ' + @error_message;
end catch;
```

### Пояснение к использованию:
- **start transaction** — начало транзакции.
- **commit** — фиксирует изменения, внесённые в рамках транзакции.
- **rollback** — отменяет все изменения, сделанные после начала транзакции.
- **try...catch** — позволяет отлавливать ошибки и корректно обрабатывать их, откатывая изменения, если это необходимо.