# Inserting, modifying and deleting Data

## Insert

First, we can create table as a copy of result set from another query using `CREATE TABLE AS` statement.

```sql
CREATE TABLE invoice_copy AS 
SELECT * FROM invoices;
```

```sql
INSERT INTO <table_name> VALUES (val1, val2, ...);
```

```sql
INSERT INTO employees
VALUES('Piyush', 'Patel', 'Electrical Department', 2000.00);
```

- We can also specify the column names and the corresponding values must be in the same order.

```sql
INSERT INTO employees (first_name, last_name, department, salary) VALUES ('Piyush', 'Patel', 'Electrical Department', 2000.00);
```

- We can also insert multiple values using:

```sql
INSERT INTO employees (first_name, last_name, department, salary) VALUES ('Piyush', 'Patel', 'Electrical Department', 2000.00),
('Pritesh', 'Patel', 'Quality Department', 32000.00);
```

- If we have specified default value during creation of the table and allowed null, then those values can be omitted and will be automatically inserted. In that case, we need to specify the column name in which we want to insert value.

```sql
INSERT INTO employess (first_name, last_name) VALUES ('Ishit', 'Patel');
-- department and salary will be inserted with default values.
```

- We can also use subquery to insert data values from another query.

```sql
INSERT INTO invoice_archive
    (invoice_id, invoice_number, invoice_total) 
    SELECT invoice_id, invoice_number, invoice_total FROM invoices
    WHERE invoice_total - payment_total > 500;
```

## Update data

```sql
UPDATE table_name 
SET column1=expression1, ....
WHERE search_condition;
```

```sql
UPDATE invoices
SET payment_date = '2016-09-22', payment_total = 2015.0
WHERE invoice_number = '987A';
```

If the search condition matches more than one row, all rows are updated.

- Use subquery to update

```sql
UPDATE invoices
SET vendor_id = 1
WHERE vendor_id IN
    (SELECT vendor_id
     FROM vendors
     WHERE vendor_state IN ('CA', 'AR', 'OH'));
```

## Delete

To delete one or more rows, use DELETE statement.

```sql
DELETE FROM table_name
[WHERE search_condition];
```

```sql
DELETE FROM invoices
WHERE invoice_id = 303;
```

It deletes all the rows that match the search condition.

- Use subquery to delete

```sql
DELETE FROM invoices
WHERE invoice_id IN
    (SELECT invoice_id
    FROM invoices_accounts
    WHERE vendor_id = 114);
```

