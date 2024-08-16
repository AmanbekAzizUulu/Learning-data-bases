### 1. **IFNULL()**

**Описание**:
- Функция `IFNULL()` используется для замены значения `NULL` на другое значение, заданное пользователем. Она принимает два аргумента: если первый аргумент не равен `NULL`, то возвращается его значение, в противном случае возвращается второй аргумент.

**Синтаксис**:
```sql
IFNULL(expression, replacement)
```

- `expression`: Значение, которое проверяется на `NULL`.
- `replacement`: Значение, которое возвращается, если `expression` равно `NULL`.

**Примеры использования**:

1. **Пример с числовым значением**:
   ```sql
   SELECT IFNULL(NULL, 10) AS result;
   ```
   **Результат**: `10`  
   *Здесь `NULL` заменяется на `10`.*

2. **Пример с текстовым значением**:
   ```sql
   SELECT IFNULL(name, 'No Name') AS customer_name FROM customers;
   ```
   *Если в поле `name` есть значение `NULL`, то вместо него вернётся строка `'No Name'`.*

3. **Пример с запросом**:
   ```sql
   SELECT IFNULL(discount, 0) AS final_discount FROM orders;
   ```
   *Если в поле `discount` для какого-либо заказа значение `NULL`, оно заменяется на `0`.*

### 2. **COALESCE()**

**Описание**:
- Функция `COALESCE()` возвращает первый ненулевой (не `NULL`) аргумент из списка. Она полезна, если нужно проверить несколько столбцов или выражений и вернуть первое ненулевое значение.

**Синтаксис**:
```sql
COALESCE(expression1, expression2, ..., expressionN)
```

- `expression1, expression2, ..., expressionN`: Значения, которые проверяются на `NULL`.

**Примеры использования**:

1. **Пример с несколькими значениями**:
   ```sql
   SELECT COALESCE(NULL, NULL, 'Default Value', 'Another Value') AS result;
   ```
   **Результат**: `'Default Value'`  
   *Здесь функция возвращает первое ненулевое значение `'Default Value'`.*

2. **Пример с текстовыми полями**:
   ```sql
   SELECT COALESCE(first_name, middle_name, last_name, 'No Name') AS full_name FROM customers;
   ```
   *Функция возвращает первое ненулевое значение из списка полей `first_name`, `middle_name`, `last_name`.*

3. **Пример с вычисляемым выражением**:
   ```sql
   SELECT COALESCE(total_amount, subtotal + tax, 0) AS final_amount FROM invoices;
   ```
   *Если значение `total_amount` равно `NULL`, то используется сумма `subtotal + tax`. Если и это равно `NULL`, то возвращается `0`.*

**Сравнение IFNULL() и COALESCE()**:

- `IFNULL()` принимает только два аргумента и заменяет `NULL` на заданное значение.
- `COALESCE()` может принимать любое количество аргументов и возвращает первое ненулевое значение из списка.

**Пример сравнения**:
```sql
SELECT 
    IFNULL(NULL, 'Fallback') AS ifnull_result,
    COALESCE(NULL, NULL, 'Fallback') AS coalesce_result;
```

**Результат**: Оба выражения вернут `'Fallback'`.

`COALESCE()` более универсальна и гибка, чем `IFNULL()`, но в простых случаях, когда нужно заменить `NULL` на конкретное значение, `IFNULL()` может быть предпочтительнее из-за своей простоты.