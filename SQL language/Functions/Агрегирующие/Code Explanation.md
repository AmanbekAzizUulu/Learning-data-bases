
The two SQL queries you provided give different results due to differences in their `GROUP BY` clauses and the usage of the `ROLLUP` operator. Let's break down each query and explain why they produce different outcomes.

### Query 1

```sql
select
    pms.payment_method_name,
    sum(ps.amount) as total
from
    payments as ps
join
    payment_methods pms
    on
        ps.payment_method_id = pms.payment_method_id
group by
    payment_method_name,
    ps.amount with rollup;
```

#### Explanation:

- **Selection**: Selects the payment method name and the sum of the payment amounts.
- **Join**: Joins the `payments` table with the `payment_methods` table based on `payment_method_id`.
- **Group By**: Groups the results by `payment_method_name` and `ps.amount`, with `ROLLUP`.
- **ROLLUP**: Adds subtotals and a grand total for each combination of `payment_method_name` and `ps.amount`.

In this query, the `ROLLUP` is applied to the combination of `payment_method_name` and `ps.amount`. This means that it will produce subtotals for each payment amount within each payment method and a grand total for all payment methods and amounts. 

This can result in a large number of rows, especially if there are many distinct `amount` values. The subtotals will be grouped by `payment_method_name` first and then by each `ps.amount`.

### Query 2

```sql
select
    pms.payment_method_name,
    sum(ps.amount) as total
from
    payments as ps
join
    payment_methods pms
    on
        ps.payment_method_id = pms.payment_method_id
group by
    payment_method_name with rollup;
```

#### Explanation:

- **Selection**: Selects the payment method name and the sum of the payment amounts.
- **Join**: Joins the `payments` table with the `payment_methods` table based on `payment_method_id`.
- **Group By**: Groups the results by `payment_method_name`, with `ROLLUP`.

In this query, the `ROLLUP` is applied only to `payment_method_name`. This means it will produce subtotals for each payment method and a grand total for all payment methods, without considering individual payment amounts.

### Key Differences

1. **Grouping Columns**:
    - **Query 1** groups by both `payment_method_name` and `ps.amount`.
    - **Query 2** groups only by `payment_method_name`.

2. **ROLLUP Application**:
    - **Query 1**: `ROLLUP` operates on combinations of `payment_method_name` and `ps.amount`, leading to subtotals for each amount within each payment method.
    - **Query 2**: `ROLLUP` operates on `payment_method_name` alone, leading to subtotals only for each payment method.

3. **Result Set Size**:
    - **Query 1** can produce a larger and more detailed result set due to the finer level of grouping.
    - **Query 2** produces a more summarized result set, with subtotals and grand totals only at the level of payment methods.

### Example Scenario

Assume you have the following data:

| payment_method_name | amount |
|---------------------|--------|
| Credit Card         | 100    |
| Credit Card         | 200    |
| Cash                | 150    |
| Cash                | 150    |

#### Query 1 Results:

| payment_method_name | amount | total |
|---------------------|--------|-------|
| Credit Card         | 100    | 100   |
| Credit Card         | 200    | 200   |
| Credit Card         | NULL   | 300   | (Subtotal for Credit Card)
| Cash                | 150    | 300   |
| Cash                | NULL   | 300   | (Subtotal for Cash)
| NULL                | NULL   | 600   | (Grand Total)

#### Query 2 Results:

| payment_method_name | total |
|---------------------|-------|
| Credit Card         | 300   |
| Cash                | 300   |
| NULL                | 600   | (Grand Total)

### Summary

- **Query 1** provides a detailed breakdown including the amounts, leading to more rows with subtotals for each `amount` within each `payment_method_name`.
- **Query 2** provides a summary at the `payment_method_name` level, leading to fewer rows with a single subtotal for each `payment_method_name`.

This explains why the two queries produce different results.