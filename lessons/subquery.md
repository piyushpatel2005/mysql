# Subquery

Subqueries can be nested with one subquery using another subquery.

```sql
SELECT invoice_number, invoice_date, invoice_total
FROM invoices
WHERE invoice_total > (SELECT AVG(invoice_total) FROM invoices)
ORDER BY invoice_total;
```

Sometimes you have an option that subquery kind of statement can be written using JOIN. Both have their own advantages.

For example:

```sql
SELECT invoice_number, invoice_date, invoice_total
FROM invoices JOIN vendors
    ON invoices.vendor_id = vendors.vendor_id
WHERE vendor_state = 'CA';


SELECT invoice_number, invoice_date, invoice_total
FROM invoices
WHERE vendor_id IN
    (SELECT vendor_id
    FROM vendors
    WHERE vendor_state = 'CA');
```

We can use SOME, ANY or ALL keywords to get results as needed.

```sql
-- get invoices smaller than the largest invoice for vendor 115
SELECT vendor_name, invoice_number, invoice_total
FROM vendors JOIN invoices ON vendors.vendor_id = invoices.invoice_id
WHERE invoice_total < ANY
    (SELECT invoice_total
    FROM invoices
    WHERE vendor_id = 115);
```

The SOME keyword works the same as the ANY keyword.

```sql
SELECT vendor_id, invoice_number, invoice_total
FROM invoices i
WHERE invoice_total > 
    (SELECT AVG(invoice_total)
    FROM invoices
    WHERE vendor_id = i.vendor_id)
ORDER BY vendor_id, invoice_total;
```

- `EXIST`: We can use EXISTS operator to test that one or more rows are returned by the subquery. Similarly, NOT EXISTS operator is used to test that no rows are returned by the subquery.

```sql
-- get vendors that don't have invoices
SELECT vendor_id, vendor_name, vendor_state
FROM vendors
WHERE NOT EXISTS
    (SELECT *
    FROM invoices
    WHERE vendor_id = vendors.vendor_id);
```

- subquery in SELECT clause

```sql
SELECT vendor_name, (SELECT MAX(invoice_date) FROM invoices
WHERE vendor_id = vendors.vendor_id) AS latest_inv
FROM vendors
ORDER BY lates_inv DESC;
```

- subquery in FROM clause

```sql
SELECT vendor_state, MAX(sum_of_invoices) AS max_sum_of_invoices
FROM 
    (
        SELECT vendor_state, vendor_name, SUM(invoice_total) AS sum_of_invoices
        FROM vendors v JOIN invoices i
        ON v.vendor_id = i.vendor_id
        GROUP BY vendor_state, vendor_name
    ) t
    GROUP BY vendor_state;
```

### How to write complex query

To write complex query
1. Identify the problem
2. Write pseudo code using simple English
3. Code the subqueries and test them to make sure that they return the correct data.
4. Include subqueries inside the main query and test.

```sql
SELECT vendor_state, MAX(sum_of_invoices) AS sum_of_invoices
FROM
(
    SELECT vendor_state, vendor_name, SUM(invoice_total) AS sum_of_invoices
    FROM vendors v JOIN invoices i
    ON v.vendor_id = i.vendor_id
    GROUP BY vendor_state, vendor_name
)t
GROUP BY vendor_state;
```



