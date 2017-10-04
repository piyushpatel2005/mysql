# SQL Language

The statements that work with the data in a database are called **data manipulation language (DML)**. The statements that create databases and work with the objects within a database are called **data definition language(DDL)**. 

In SQL, comments can be inserted using /*... */
Single line comments can be inserted using two hyphens (--).

**SELECT** To view data from the table, use *SELECT* statement.

```sql
SELECT * FROM <table_name>
-- Here star is selector. This means all columns.
```

To see all the databases:

```sql
SHOW DATABASES;
```

To use one of the databases,

```sql
USE <dbname>;
USE invoice_db;
```

```sql
SELECT <column_name> FROM <table_name>
```

**Basic syntax for SELECT**

```sql
SELECT column_names
    [FROM table_name]
    [WHERE search_condition]
    [ORDER BY order_by_list]
    [LIMIT row_limit]
;
```

- `SELECT`  - describes the columns in the result set.
- `FROM` - names of the base table from which the query is retrieves data.
- `WHERE` - condition that must be met in the result.
- `ORDER BY` - specifies how to sort the result set.
- `LIMIT` - specifies the number of rows to return.

Before selecting rows, you might need to see how  many tables are there.

```sql
SHOW TABLES;  -- shows tables in the database
```

To see the structure of the table (which columns are there in the table), use:

```sql
DESCRIBE <table_name>
```

```sql
DESCRIBE invoices
```

`invoices` tables consists of invoice_id, vendor_id, invoice_number, invoice_date, invoice_total, payment_total, credit_total, terms_id, invoice_due_date and payment_date.

- You can specify `ORDER BY` in ascending order or descending order using `ASC` (default) and `DESC`.

```sql
SELECT invoice_id, invoice_total, credit_total
FROM invoices
ORDER BY invoice_total DESC;
```

```sql
SELECT invoice_id, invoice_total, credit_total
FROM invoices
WHERE invoice_id = 18;
```

```sql
SELECT invoice_number, invoice_date, invoice_total
FROM invoices
WHERE invoice_date BETWEEN '2015-02-01' AND '2015-06-30'
ORDER BY invoice_date;
```

We can also rename the column on the fly for a view, using `AS` keyword.

```sql
SELECT [ALL | DISTINCT]
    column_names AS result_column;
```

We can also concatenate first name and last name fields from the table.

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM persons;
```

```sql
SELECT 
    invoice_number AS "Invoice Number", 
    invoice_date AS Date, 
    invoice_total AS Total
FROM invoices;
```

```sql
SELECT invoice_number, invoice_date, invoice_total, invoice_total - payment_total - credit_total AS due
FROM invoices;
```

MySQL evaluates an arithmetic expressions from left to right and also on order or precedence.

|Operator | Name |
-----------:|----:|
|*          |Multiplication |
| /         |Division |
| DIV       | Integer Division |
|(MOD)      | Modulo |
|+          | Addition |
|-          | Subtraction |

```sql
SELECT invoice_id,
        invoice_id / 3 AS decimal_quotient,
        invoice_id DIV 3 AS integer_quotient,
        invoice_id % 3 AS remainder
FROM invoices;
```

- Use `CONCAT` to combine two string fields.

```sql
SELECT vendor_id, vendor_name, CONCAT(vendor_city, ', ', vendor_state) 
AS address 
FROM vendors;
```

To escape single quote, we use two single quotes (')

```sql
SELECT CONCAT(vendor_name, '''s Address: ') AS Vendor,
CONCAT(vendor_city, ', ', vendor_state)
AS Address
FROM vendors;
-- vendor_name's Address: city, state
```

