В MySQL можно устанавливать уровень изоляции транзакций тремя разными способами:

```sql
SET [GLOBAL | SESSION] 
	TRANSACTION transaction_characteristic_1 , 
				[transaction_characteristic_2]
				... 
				[transaction_characteristic_n]
```


```text

transaction_characteristic:{ 
		ISOLATION LEVEL level | access_mode 
	
	} 
	
level_:{ 
		REPEATABLE READ 
		READ COMMITTED
		READ UNCOMMITTED
		SERIALIZABLE 
	} _

access_mode:{ 
		READ WRITE 
		READ ONLY 
	}
```

Конструкция `SET [GLOBAL | SESSION] TRANSACTION` позволяет настраивать характеристики транзакций, такие как уровень изоляции и режим доступа. В следующем формате я разделю синтаксис на понятные блоки и приведу примеры для каждого случая.
### Пояснение
1. **GLOBAL | SESSION**:
   - `GLOBAL`: Изменяет настройки для всех новых сеансов.
   - `SESSION`: Изменяет настройки для текущего сеанса.

2. **transaction_characteristic**:
   - **ISOLATION LEVEL**: Задаёт уровень изоляции транзакции.
   - **access_mode**: Определяет, будет ли транзакция с возможностью записи (`READ WRITE`) или только для чтения (`READ ONLY`).

### Примеры

#### 1. Установка уровня изоляции для текущей транзакции

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

Этот запрос задаёт уровень изоляции `READ COMMITTED` только для следующей транзакции.

#### 2. Установка уровня изоляции для текущего сеанса

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

Все транзакции в текущем сеансе будут использовать уровень изоляции `REPEATABLE READ`.

#### 3. Установка глобального уровня изоляции для всех новых сеансов

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

Новые сеансы будут использовать уровень изоляции `SERIALIZABLE` по умолчанию.

#### 4. Установка режима доступа для текущей транзакции

```sql
SET TRANSACTION READ ONLY;
```

Следующая транзакция будет только для чтения.

#### 5. Установка уровня изоляции и режима доступа для текущего сеанса

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED, READ WRITE;
```

Этот запрос устанавливает уровень изоляции `READ COMMITTED` и разрешает запись для всех транзакций в текущем сеансе.

#### 6. Установка глобального уровня изоляции и режима доступа

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED, READ ONLY;
```

Новые сеансы будут использовать уровень изоляции `READ UNCOMMITTED` и режим только для чтения.

---
### 1. **Установка уровня изоляции для текущей транзакции**
Этот метод применяется для одной конкретной транзакции. Уровень изоляции задаётся только для неё, и он не влияет на последующие транзакции.

```sql
SET TRANSACTION ISOLATION LEVEL уровень_изоляции;
```

Пример:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### 2. **Установка уровня изоляции для текущего сеанса**
Этот метод задаёт уровень изоляции для всех транзакций в текущем сеансе (сессии). Уровень изоляции будет применяться к каждому запросу в рамках данного подключения к базе данных, пока оно активно.

```sql
SET SESSION TRANSACTION ISOLATION LEVEL уровень_изоляции;
```

Пример:

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 3. **Установка глобального уровня изоляции для всех новых подключений**
Этот метод изменяет уровень изоляции по умолчанию для всех новых подключений к базе данных. Он не повлияет на текущие сеансы, а только на те, что будут установлены после изменения этой настройки.

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL уровень_изоляции;
```

Пример:

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Важное замечание
После установки уровня изоляции на глобальном уровне существующие соединения не изменят свой текущий уровень, он останется таким, каким был установлен в момент соединения.

Каждый из этих методов предоставляет гибкость в управлении уровнем изоляции для разных сценариев работы с транзакциями в MySQL.