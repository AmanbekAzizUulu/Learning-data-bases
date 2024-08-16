SQL Events are a feature available in some database systems (notably MySQL) that allow you to schedule and automate the execution of SQL code at a specified time or on a recurring basis. Events are similar to cron jobs in Unix/Linux systems and can be used for tasks such as data cleanup, report generation, or periodic updates.

### **Key Concepts of SQL Events**

1. **Event Scheduler:**
   - The event scheduler must be enabled in the database system for events to work. In MySQL, the event scheduler is controlled by the `event_scheduler` system variable.
   - To check if the event scheduler is enabled:
     ```sql
     SHOW VARIABLES LIKE 'event_scheduler';
     ```
   - To enable the event scheduler:
     ```sql
     SET GLOBAL event_scheduler = ON;
     ```

2. **Creating an Event:**
   - Events are created using the `CREATE EVENT` statement. You can specify a one-time event or a recurring event.

### **Examples of SQL Events**

#### **1. One-Time Event**

This example creates a one-time event that deletes all records from a `logs` table that are older than 30 days. The event will execute once, at a specific date and time.

```sql
CREATE EVENT delete_old_logs
ON SCHEDULE AT '2024-08-31 00:00:00'
DO
DELETE FROM logs WHERE log_date < NOW() - INTERVAL 30 DAY;
```

- **Explanation:**
  - `CREATE EVENT delete_old_logs`: Creates a new event named `delete_old_logs`.
  - `ON SCHEDULE AT '2024-08-31 00:00:00'`: Schedules the event to run at midnight on August 31, 2024.
  - `DO DELETE FROM logs WHERE log_date < NOW() - INTERVAL 30 DAY;`: The event will execute this SQL statement to delete logs older than 30 days.

#### **2. Recurring Event**

This example creates a recurring event that updates a `sales_summary` table every day at midnight.

```sql
CREATE EVENT daily_sales_summary
ON SCHEDULE EVERY 1 DAY
STARTS '2024-08-15 00:00:00'
DO
BEGIN
    DELETE FROM sales_summary;
    INSERT INTO sales_summary (date, total_sales)
    SELECT CURDATE(), SUM(amount) FROM sales WHERE sale_date = CURDATE();
END;
```

- **Explanation:**
  - `CREATE EVENT daily_sales_summary`: Creates a new event named `daily_sales_summary`.
  - `ON SCHEDULE EVERY 1 DAY STARTS '2024-08-15 00:00:00'`: Schedules the event to run every day at midnight, starting on August 15, 2024.
  - The `BEGIN ... END` block allows multiple SQL statements to be executed as part of the event.
  - The event deletes the current records in the `sales_summary` table and then inserts a summary of sales for the current day.

#### **3. Event That Executes Every Hour**

This example creates an event that archives records from an `orders` table every hour.

```sql
CREATE EVENT archive_orders
ON SCHEDULE EVERY 1 HOUR
DO
BEGIN
    INSERT INTO archived_orders (order_id, order_date, customer_id, amount)
    SELECT order_id, order_date, customer_id, amount
    FROM orders
    WHERE order_date < NOW() - INTERVAL 1 MONTH;

    DELETE FROM orders WHERE order_date < NOW() - INTERVAL 1 MONTH;
END;
```

- **Explanation:**
  - `CREATE EVENT archive_orders`: Creates a new event named `archive_orders`.
  - `ON SCHEDULE EVERY 1 HOUR`: Schedules the event to run every hour.
  - The event archives orders that are older than one month by copying them to the `archived_orders` table and then deletes those records from the `orders` table.

### **Modifying and Dropping Events**

1. **Modifying an Event:**
   - You can modify an existing event using the `ALTER EVENT` statement.

   ```sql
   ALTER EVENT daily_sales_summary
   ON SCHEDULE EVERY 1 DAY
   STARTS '2024-08-16 00:00:00';
   ```

   - This alters the `daily_sales_summary` event to start on August 16, 2024, instead of August 15.

2. **Dropping an Event:**
   - If you no longer need an event, you can drop it using the `DROP EVENT` statement.

   ```sql
   DROP EVENT IF EXISTS delete_old_logs;
   ```

   - This drops the `delete_old_logs` event if it exists.

### **Managing Events:**

- **Listing Events:**
  - To see all events in the current database, use:
    ```sql
    SHOW EVENTS;
    ```

- **Disabling an Event:**
  - If you want to temporarily disable an event without dropping it:
    ```sql
    ALTER EVENT event_name DISABLE;
    ```

- **Enabling an Event:**
  - To re-enable a disabled event:
    ```sql
    ALTER EVENT event_name ENABLE;
    ```

### **Considerations When Using Events**

- **Permissions:** Ensure that the user creating the event has the necessary privileges (`EVENT` privilege).
- **Resource Usage:** Be cautious about scheduling events that run too frequently or that perform resource-intensive tasks, as they can impact database performance.
- **Time Zones:** If your application operates across multiple time zones, consider how this will affect event scheduling.

SQL Events provide a powerful tool for automating database management tasks, reducing the need for manual intervention, and ensuring that routine operations are carried out consistently and on time.

---
Вот общая форма синтаксиса для создания событий в MySQL, а также несколько примеров:

### **Общая Форма Синтаксиса**

```sql
CREATE 
	EVENT event_name
ON SCHEDULE 
	schedule
[DO 
	 sql_statement]
```

```sql
CREATE [DEFINER = user] EVENT [IF NOT EXISTS] event_name
ON SCHEDULE 
	schedule 
	[ON COMPLETION [NOT] PRESERVE] 
	[ENABLE | DISABLE | DISABLE ON {REPLICA | SLAVE}] 
	[COMMENT 'string'] 
DO 
	event_body; 
```

```plain text
schedule:
	{ 
		- AT timestamp [+ INTERVAL interval ...] 
		
		- EVERY interval [STARTS timestamp [+ INTERVAL interval] ...] 
		                 [ENDS timestamp [+ INTERVAL interval] ...] 
	}
	
interval: 
	quantity
		{
			- YEAR 
			- QUARTER 
			- MONTH 
			- DAY 
			- HOUR 
			- MINUTE 
			- WEEK 
			- SECOND 
			- YEAR_MONTH 
			- DAY_HOUR 
			- DAY_MINUTE 
			- DAY_SECOND 
			- HOUR_MINUTE 
			- HOUR_SECOND 
			- MINUTE_SECOND
		}
```
#### **Компоненты:**

- **`event_name`:** Имя события. Должно быть уникальным в рамках базы данных.
- **`ON SCHEDULE schedule`:** Определяет расписание для события. Может быть `AT` для одноразового выполнения или `EVERY` для повторяющихся событий.
- **`DO sql_statement`:** SQL-оператор или блок операторов, который будет выполнен при наступлении события.

Вот отформатированное и упрощенное объяснение синтаксиса для создания событий в MySQL:

### **Синтаксис Создания События**

```sql
CREATE
    [DEFINER = user]
    EVENT
    [IF NOT EXISTS]
    event_name
    ON SCHEDULE schedule
    [ON COMPLETION [NOT] PRESERVE]
    [ENABLE | DISABLE | DISABLE ON {REPLICA | SLAVE}]
    [COMMENT 'string']
    DO event_body;
```

### **Пояснение Компонентов**

#### **1. DEFINER**

- **`DEFINER = user`**: Определяет пользователя, от имени которого будет выполняться событие.

#### **2. IF NOT EXISTS**

- **`IF NOT EXISTS`**: Если событие с таким именем уже существует, создание нового события не будет выполнено.

#### **3. event_name**

- **`event_name`**: Имя создаваемого события. Должно быть уникальным в базе данных.

#### **4. ON SCHEDULE**

- **`ON SCHEDULE schedule`**: Определяет расписание выполнения события.

**Синтаксис Расписания (`schedule`)**:

- **`AT timestamp [+ INTERVAL interval] ...`**: Указывает конкретное время или дату для одноразового выполнения события. Можно добавить интервал для выполнения через определенное время от указанного `timestamp`.

- **`EVERY interval`**: Определяет периодическое выполнение события.

  **Дополнительно**:
  - **`STARTS timestamp [+ INTERVAL interval] ...`**: Определяет время начала для периодического выполнения события. Можно добавить интервал для начала выполнения через определенное время от указанного `timestamp`.
  - **`ENDS timestamp [+ INTERVAL interval] ...`**: Определяет время окончания для периодического выполнения события. Можно добавить интервал для окончания выполнения через определенное время от указанного `timestamp`.

**Синтаксис Интервала (`interval`)**:

- **`quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE | WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE | DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}`**

  - **`YEAR`**: Год
  - **`QUARTER`**: Квартал
  - **`MONTH`**: Месяц
  - **`DAY`**: День
  - **`HOUR`**: Час
  - **`MINUTE`**: Минуты
  - **`WEEK`**: Неделя
  - **`SECOND`**: Секунда
  - **`YEAR_MONTH`**: Год и месяц
  - **`DAY_HOUR`**: День и час
  - **`DAY_MINUTE`**: День и минута
  - **`DAY_SECOND`**: День и секунда
  - **`HOUR_MINUTE`**: Час и минута
  - **`HOUR_SECOND`**: Час и секунда
  - **`MINUTE_SECOND`**: Минута и секунда

#### **5. ON COMPLETION**

- **`ON COMPLETION [NOT] PRESERVE`**: Определяет, должен ли MySQL сохранить событие после его завершения:
  - **`PRESERVE`**: Событие будет сохранено и продолжит существовать, даже после его выполнения.
  - **`NOT PRESERVE`**: Событие будет удалено после выполнения.

#### **6. ENABLE | DISABLE | DISABLE ON {REPLICA | SLAVE}**

- **`ENABLE`**: Событие активно и будет выполняться согласно расписанию.
- **`DISABLE`**: Событие деактивировано и не будет выполняться.
- **`DISABLE ON {REPLICA | SLAVE}`**: Событие деактивировано только на репликах или слейвах (если используется репликация).

#### **7. COMMENT**

- **`COMMENT 'string'`**: Добавляет комментарий к событию для пояснения его назначения или назначения.

#### **8. DO**

- **`DO event_body`**: Определяет блок SQL-кода, который будет выполнен при наступлении события.

### **Примеры**

**Пример 1: Одноразовое Событие**

```sql
CREATE EVENT one_time_event
ON SCHEDULE AT '2024-09-01 00:00:00'
DO
BEGIN
    DELETE FROM logs WHERE log_date < NOW() - INTERVAL 30 DAY;
END;
```

**Пример 2: Повторяющееся Событие Каждые Час**

```sql
CREATE EVENT hourly_event
ON SCHEDULE EVERY 1 HOUR
STARTS '2024-08-14 00:00:00'
ENDS '2024-08-20 23:59:59'
DO
BEGIN
    UPDATE daily_summary SET summary_date = CURDATE();
END;
```

**Пример 3: Событие с Комментариями**

```sql
CREATE EVENT daily_cleanup
ON SCHEDULE EVERY 1 DAY
STARTS '2024-08-14 00:00:00'
COMMENT 'Daily cleanup of old records'
DO
BEGIN
    DELETE FROM old_records WHERE creation_date < NOW() - INTERVAL 90 DAY;
END;
```

Этот формат позволяет легко понимать и использовать события в MySQL для автоматизации задач и планирования.


### **Примеры**

#### **1. Одноразовое Событие**

Создание события, которое выполнится один раз в определенное время.

```sql
CREATE EVENT event_one_time
ON SCHEDULE AT '2024-09-01 00:00:00'
DO
BEGIN
    -- Пример SQL-оператора, который удаляет старые записи
    DELETE FROM logs WHERE log_date < NOW() - INTERVAL 30 DAY;
END;
```

- **`ON SCHEDULE AT '2024-09-01 00:00:00'`:** Событие выполнится 1 сентября 2024 года в полночь.

#### **2. Повторяющееся Событие**

Создание события, которое выполняется каждый день.

```sql
CREATE EVENT event_daily
ON SCHEDULE EVERY 1 DAY
STARTS '2024-08-15 00:00:00'
DO
BEGIN
    -- Пример SQL-оператора, который обновляет суммарные продажи
    INSERT INTO sales_summary (date, total_sales)
    SELECT CURDATE(), SUM(amount) FROM sales WHERE sale_date = CURDATE();
END;
```

- **`ON SCHEDULE EVERY 1 DAY STARTS '2024-08-15 00:00:00'`:** Событие начнет выполняться каждый день, начиная с 15 августа 2024 года в полночь.

#### **3. Событие с Ограниченным Временем Выполнения**

Создание события, которое выполняется каждый час, но только в течение определенного периода.

```sql
CREATE EVENT event_hourly_limited
ON SCHEDULE EVERY 1 HOUR
STARTS '2024-08-14 00:00:00'
ENDS '2024-08-20 23:59:59'
DO
BEGIN
    -- Пример SQL-оператора, который архивирует старые заказы
    INSERT INTO archived_orders (order_id, order_date, customer_id, amount)
    SELECT order_id, order_date, customer_id, amount
    FROM orders
    WHERE order_date < NOW() - INTERVAL 1 MONTH;

    DELETE FROM orders WHERE order_date < NOW() - INTERVAL 1 MONTH;
END;
```

- **`ON SCHEDULE EVERY 1 HOUR STARTS '2024-08-14 00:00:00' ENDS '2024-08-20 23:59:59'`:** Событие начнет выполняться каждый час, начиная с 14 августа 2024 года и заканчивая 20 августа 2024 года в 23:59.

### **Другие Варианты**

- **`ON SCHEDULE EVERY 2 WEEKS`:** Выполняется каждые 2 недели.
- **`ON SCHEDULE EVERY 5 MINUTE`:** Выполняется каждые 5 минут.

### **Дополнительные Команды**

- **Изменение события:**
  ```sql
  ALTER EVENT event_name
  ON SCHEDULE EVERY 2 DAY;
  ```

- **Удаление события:**
  ```sql
  DROP EVENT IF EXISTS event_name;
  ```

- **Просмотр всех событий:**
  ```sql
  SHOW EVENTS;
  ```

- **Просмотр определения конкретного события:**
  ```sql
  SHOW CREATE EVENT event_name;
  ```

Эти примеры и синтаксис помогут вам создавать и управлять событиями в MySQL для автоматизации задач и улучшения управления базой данных.

---

В MySQL, при создании событий (Events) для планирования задач, можно использовать различные ключевые слова и опции для определения расписания выполнения события. Вот подробное объяснение ключевых слов и опций, таких как `STARTS`, `ENDS`, и другие:

### **Ключевые Слова и Опции для Расписания Событий**

#### **1. `ON SCHEDULE`**

Используется для определения расписания выполнения события. Включает следующие опции:

- **`AT`**: Указывает точное время выполнения события один раз.
  ```sql
  ON SCHEDULE AT 'YYYY-MM-DD HH:MM:SS'
  ```

- **`EVERY`**: Указывает периодическое выполнение события.
  ```sql
  ON SCHEDULE EVERY interval
  ```

- **`STARTS`**: Определяет начало выполнения события. Используется вместе с `EVERY`.
  ```sql
  STARTS 'YYYY-MM-DD HH:MM:SS'
  ```

- **`ENDS`**: Определяет окончание выполнения события. Используется вместе с `EVERY`.
  ```sql
  ENDS 'YYYY-MM-DD HH:MM:SS'
  ```

#### **2. Примеры Использования**

**Пример 1: Одноразовое Событие**

Создает событие, которое выполнится один раз в определенное время.

```sql
CREATE EVENT event_one_time
ON SCHEDULE AT '2024-09-01 00:00:00'
DO
BEGIN
    -- Пример SQL-оператора
    DELETE FROM logs WHERE log_date < NOW() - INTERVAL 30 DAY;
END;
```

- **`ON SCHEDULE AT '2024-09-01 00:00:00'`**: Выполняет событие 1 сентября 2024 года в полночь.

**Пример 2: Повторяющееся Событие**

Создает событие, которое выполняется каждый день.

```sql
CREATE EVENT event_daily
ON SCHEDULE EVERY 1 DAY
STARTS '2024-08-15 00:00:00'
DO
BEGIN
    -- Пример SQL-оператора
    INSERT INTO sales_summary (date, total_sales)
    SELECT CURDATE(), SUM(amount) FROM sales WHERE sale_date = CURDATE();
END;
```

- **`ON SCHEDULE EVERY 1 DAY STARTS '2024-08-15 00:00:00'`**: Событие начнет выполняться каждый день, начиная с 15 августа 2024 года в полночь.

**Пример 3: Повторяющееся Событие с Ограниченным Временем**

Создает событие, которое выполняется каждый час в течение определенного периода.

```sql
CREATE EVENT event_hourly_limited
ON SCHEDULE EVERY 1 HOUR
STARTS '2024-08-14 00:00:00'
ENDS '2024-08-20 23:59:59'
DO
BEGIN
    -- Пример SQL-оператора
    INSERT INTO archived_orders (order_id, order_date, customer_id, amount)
    SELECT order_id, order_date, customer_id, amount
    FROM orders
    WHERE order_date < NOW() - INTERVAL 1 MONTH;

    DELETE FROM orders WHERE order_date < NOW() - INTERVAL 1 MONTH;
END;
```

- **`ON SCHEDULE EVERY 1 HOUR STARTS '2024-08-14 00:00:00' ENDS '2024-08-20 23:59:59'`**: Событие начнет выполняться каждый час, начиная с 14 августа 2024 года и заканчивая 20 августа 2024 года в 23:59.

### **Общие Правила**

- **`STARTS` и `ENDS`**: Опциональны. Если не указаны, событие будет выполняться бесконечно (или до тех пор, пока не будет отключено или удалено).
- **`interval`**: Могут использоваться различные интервалы, такие как `1 DAY`, `2 WEEK`, `5 MINUTE`, и т.д.

Эти опции позволяют гибко настраивать расписание выполнения событий в MySQL, обеспечивая автоматизацию и планирование задач в вашей базе данных.