Агрегирование в SQL означает использование функций для обработки и обобщения данных из нескольких строк, возвращая одно значение. Это полезно для вычисления сумм, средних значений, количества, максимальных и минимальных значений и других статистических показателей. 

Вот список основных агрегирующих функций в SQL:

1. **AVG(column_name)**: Вычисляет среднее значение указанного столбца.
    ```sql
    SELECT AVG(price) FROM book;
    ```

2. **COUNT(column_name)**: Подсчитывает количество значений в указанном столбце (не учитывает `NULL` значения).
    ```sql
    SELECT COUNT(title) FROM book;
    ```
    **COUNT(\*)**: Подсчитывает количество строк в таблице, включая строки с `NULL` значениями.
    ```sql
    SELECT COUNT(*) FROM book;
    ```

3. **SUM(column_name)**: Вычисляет сумму значений в указанном столбце.
    ```sql
    SELECT SUM(price) FROM book;
    ```

4. **MAX(column_name)**: Находит максимальное значение в указанном столбце.
    ```sql
    SELECT MAX(price) FROM book;
    ```

5. **MIN(column_name)**: Находит минимальное значение в указанном столбце.
    ```sql
    SELECT MIN(price) FROM book;
    ```

Примеры использования агрегирующих функций в запросах:

1. **AVG**
    ```sql
    SELECT AVG(price) AS average_price FROM book;
    ```

2. **COUNT**
    ```sql
    SELECT COUNT(title) AS book_count FROM book;
    ```

3. **SUM**
    ```sql
    SELECT SUM(price) AS total_price FROM book;
    ```

4. **MAX**
    ```sql
    SELECT MAX(price) AS highest_price FROM book;
    ```

5. **MIN**
    ```sql
    SELECT MIN(price) AS lowest_price FROM book;
    ```

Агрегирующие функции часто используются с предложением `GROUP BY` для группировки строк по определённым значениям перед применением агрегирующих функций. Например:

```sql
SELECT author, COUNT(*) AS book_count
FROM book
GROUP BY author;
```

Этот запрос подсчитывает количество книг для каждого автора.

---
Агрегирующие функции в SQL часто используются вместе с командами, которые позволяют группировать данные и накладывать условия на агрегированные результаты. Основные команды, которые часто используются с агрегирующими функциями, включают:

1. **SELECT**: Используется для выбора данных из базы данных.
    ```sql
    SELECT AVG(price) FROM book;
    ```

2. **FROM**: Указывает таблицу, из которой нужно выбрать данные.
    ```sql
    SELECT SUM(price) FROM book;
    ```

3. **WHERE**: Используется для фильтрации строк перед применением агрегирующих функций.
    ```sql
    SELECT AVG(price) FROM book WHERE author = 'John Doe';
    ```

4. **GROUP BY**: Используется для группировки строк, чтобы агрегирующие функции могли быть применены к каждой группе отдельно.
    ```sql
    SELECT author, COUNT(*) AS book_count
    FROM book
    GROUP BY author;
    ```

5. **HAVING**: Используется для фильтрации групп после применения агрегирующих функций. Это похоже на `WHERE`, но применяется к агрегированным данным.
    ```sql
    SELECT author, COUNT(*) AS book_count
    FROM book
    GROUP BY author
    HAVING COUNT(*) > 1;
    ```

6. **ORDER BY**: Используется для сортировки результатов, включая результаты агрегирующих функций.
    ```sql
    SELECT author, AVG(price) AS average_price
    FROM book
    GROUP BY author
    ORDER BY average_price DESC;
    ```

7. **JOIN**: Используется для объединения данных из нескольких таблиц перед их агрегированием.
    ```sql
    SELECT b.author, AVG(b.price) AS average_price
    FROM book b
    JOIN publisher p ON b.publisher_id = p.id
    WHERE p.name = 'Penguin'
    GROUP BY b.author;
    ```

Примеры использования этих команд вместе с агрегирующими функциями:

1. **SELECT и WHERE**
    ```sql
    SELECT MAX(price) FROM book WHERE genre = 'Science Fiction';
    ```

2. **GROUP BY и HAVING**
    ```sql
    SELECT author, SUM(price) AS total_sales
    FROM book
    GROUP BY author
    HAVING SUM(price) > 1000;
    ```

3. **JOIN и GROUP BY**
    ```sql
    SELECT p.name, COUNT(b.id) AS book_count
    FROM book b
    JOIN publisher p ON b.publisher_id = p.id
    GROUP BY p.name;
    ```

4. **ORDER BY**
    ```sql
    SELECT genre, COUNT(*) AS book_count
    FROM book
    GROUP BY genre
    ORDER BY book_count DESC;
    ```

Эти команды позволяют эффективно анализировать и обрабатывать данные, получая полезные инсайты и отчёты.