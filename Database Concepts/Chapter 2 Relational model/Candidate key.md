**Candidate key** (кандидат на роль ключа) в контексте реляционной базы данных — это минимальный набор полей (атрибутов) таблицы, который уникально идентифицирует каждую запись (строку) в этой таблице. Другими словами, кандидатный ключ — это любой набор полей, который может выступать в роли уникального идентификатора записи в таблице. В таблице может быть несколько кандидатных ключей, и один из них выбирается в качестве **Primary key** (первичного ключа).

### Характеристики кандидатного ключа

1. **Уникальность**:
   - Каждый набор значений кандидатного ключа уникален для каждой записи в таблице. Никакие две записи не могут иметь одинаковое значение кандидатного ключа.

2. **Минимальность**:
   - Кандидатный ключ содержит минимальное количество атрибутов, необходимых для уникальной идентификации записи. Никакой подмножество атрибутов кандидатного ключа не может быть использовано для той же цели.

### Пример

Рассмотрим таблицу `Employees` с колонками `EmployeeID`, `SSN` (социальный номер), `Email`, `PhoneNumber`:

| EmployeeID | SSN         | Email                | PhoneNumber |
|------------|-------------|----------------------|-------------|
| 1          | 123-45-6789 | alice@example.com    | 555-1234    |
| 2          | 987-65-4321 | bob@example.com      | 555-5678    |
| 3          | 111-22-3333 | charlie@example.com  | 555-8765    |

В данной таблице могут быть следующие кандидатные ключи:
- `EmployeeID`: уникально идентифицирует каждую запись.
- `SSN`: также уникально идентифицирует каждую запись.

Оба этих набора атрибутов уникально идентифицируют каждую запись и минимальны, поэтому они являются кандидатными ключами. Однако только один из них будет выбран в качестве первичного ключа (например, `EmployeeID`).

### Основные ключевые моменты

- В каждой таблице может быть несколько кандидатных ключей.
- Один из кандидатных ключей выбирается в качестве первичного ключа.
- Кандидатные ключи обеспечивают целостность данных, позволяя уникально идентифицировать записи.

### Различия между Candidate Key и другими ключами

- **Primary Key (Первичный ключ)**:
  - Это один из кандидатных ключей, который был выбран для уникальной идентификации записей в таблице.
  - В таблице может быть только один первичный ключ.
  
- **Alternate Key (Альтернативный ключ)**:
  - Это кандидатный ключ, который не был выбран в качестве первичного ключа.

- **Foreign Key (Внешний ключ)**:
  - Это ключ, который используется для установления связи между двумя таблицами.
  - Внешний ключ в одной таблице ссылается на первичный ключ в другой таблице.