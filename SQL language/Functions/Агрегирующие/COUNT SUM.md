### Функции `COUNT` и `SUM` в SQL

Функции `COUNT` и `SUM` являются агрегатными функциями в SQL, которые используются для выполнения операций над группами данных.

### 1. `COUNT`

Функция `COUNT` используется для подсчета количества строк в наборе результатов. Она может принимать различные аргументы:

- `COUNT(*)`: Подсчитывает все строки в таблице, включая строки с NULL значениями.
- `COUNT(column_name)`: Подсчитывает только ненулевые значения в указанном столбце.
- `COUNT(DISTINCT column_name)`: Подсчитывает уникальные ненулевые значения в указанном столбце.

#### Примеры использования `COUNT`

1. **Подсчет всех строк в таблице**

```sql
SELECT COUNT(*) AS total_books 
FROM book;
```

Этот запрос возвращает общее количество строк в таблице `book`.

2. **Подсчет ненулевых значений в столбце**

```sql
SELECT COUNT(price) AS count_of_prices 
FROM book;
```

Этот запрос возвращает количество ненулевых значений в столбце `price`.

3. **Подсчет уникальных ненулевых значений**

```sql
SELECT COUNT(DISTINCT author) AS unique_authors 
FROM book;
```

Этот запрос возвращает количество уникальных авторов в таблице `book`.

### 2. `SUM`

Функция `SUM` используется для вычисления суммы значений в столбце. Она игнорирует NULL значения.

#### Примеры использования `SUM`

1. **Суммирование значений в столбце**

```sql
SELECT SUM(price) AS total_price 
FROM book;
```

Этот запрос возвращает общую сумму всех значений в столбце `price` таблицы `book`.

2. **Суммирование с условием**

```sql
SELECT SUM(price) AS total_price_above_500 
FROM book 
WHERE price > 500;
```

Этот запрос возвращает сумму всех значений в столбце `price`, которые больше 500.

### Объединение `COUNT` и `SUM` с `GROUP BY`

Функции `COUNT` и `SUM` часто используются вместе с `GROUP BY` для агрегирования данных по группам.

#### Пример объединения

```sql
SELECT author, 
       COUNT(*) AS total_books, 
       SUM(price) AS total_price 
FROM book 
GROUP BY author;
```

Этот запрос возвращает количество книг и общую сумму цен для каждого автора.

### Подробное объяснение примеров

#### Пример работы `COUNT`

```sql
SELECT COUNT(*) AS total_books 
FROM book;
```

- **Описание:** Подсчитывает все строки в таблице `book`.
- **Результат:** Возвращает одно число, представляющее общее количество строк.

#### Пример работы `SUM`

```sql
SELECT SUM(price) AS total_price 
FROM book;
```

- **Описание:** Вычисляет общую сумму всех значений в столбце `price`.
- **Результат:** Возвращает одно число, представляющее общую сумму цен всех книг.

#### Пример объединения `COUNT` и `SUM` с `GROUP BY`

```sql
SELECT author, 
       COUNT(*) AS total_books, 
       SUM(price) AS total_price 
FROM book 
GROUP BY author;
```

- **Описание:** Группирует данные по авторам, затем подсчитывает количество книг (`COUNT(*)`) и вычисляет общую сумму цен (`SUM(price)`) для каждого автора.
- **Результат:** Возвращает таблицу с колонками `author`, `total_books`, и `total_price`, где каждая строка представляет одного автора с соответствующими агрегированными данными.

### Примеры данных и результирующие таблицы

Предположим, у нас есть следующая таблица `book`:

| author        | title            | price  |
|---------------|------------------|--------|
| Булгаков М.А. | Мастер и Маргарита | 500.00 |
| Булгаков М.А. | Собачье сердце   | 300.00 |
| Есенин С.А.   | Стихотворения    | 200.00 |
| Толстой Л.Н.  | Война и мир      | 600.00 |
| Толстой Л.Н.  | Анна Каренина    | 400.00 |

#### Результат запроса `COUNT(*) AS total_books`

| total_books |
|-------------|
| 5           |

#### Результат запроса `SUM(price) AS total_price`

| total_price |
|-------------|
| 2000.00     |

#### Результат запроса 
```sql
SELECT author, 
	   COUNT(*) AS total_books, 
	   SUM(price) AS total_price 
FROM book 
GROUP BY author
```

| author        | total_books | total_price |
|---------------|-------------|-------------|
| Булгаков М.А. | 2           | 800.00      |
| Есенин С.А.   | 1           | 200.00      |
| Толстой Л.Н.  | 2           | 1000.00     |

Эти примеры демонстрируют, как функции `COUNT` и `SUM` могут быть использованы для получения агрегированной информации из базы данных.


