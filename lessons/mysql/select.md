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

## Functions

We can use various SQL functions to manipulate the output view.

- `LEFT` - this functions gives the left half with specified number of characters.

```sql
SELECT first_name, last_name, CONCAT(LEFT(first_name, 1), LEFT(last_name, 1)) AS initials
FROM persons;
```

- `DATE_FORMAT` function is used to display date in different formats.

```sql
SELECT invoice_date,
    DATE_FORMAT(invoice_date, '%m/%d/%y') AS 'MM/DD/YY'
FROM invoices;
```

- `ROUND` function can be used to round the number.

```sql
SELECT invoice_date, invoice_total, ROUND(invoice_total, 1) AS nearest_dime
FROM invoices;
-- display invoice_total to one decimal point
```

- We can test different statements using like follows.

```sql
SELECT "Ed" AS first_name, "Williams" AS last_name,
CONCAT(LEFT("Ed", 1), LEFT("Williams" ,1)) AS initials;
```

- `DISTINCT` is used to eliminate duplicates.

```sql
SELECT DISTINCT vendor_city, vendor_state
FROM vendors
ORDER BY vendor_city;
-- select distinct city and state combinations
```

### Logical and Relational Operators

- `WHERE` statement allows us to specify condition for which to check the output.

The character comparions on MySQL databases are not case-sensitive. So, 'GJ' and 'gj' are considered equivalent.

`WHERE vendor_state = 'GJ'`

`WHERE invoice_date <= '2015-01-31'`

- **NOT EQUAL TO** has two variations.

`WHERE credit_total <> 0

WHERE credit_total != 0`


```sql
WHERE vendor_state = 'NJ' AND vendor_city = 'New Jersey
``'

```sql
WHERE NOT vendor_state = 'CA';
```

- Compound condition using parentheses.

```sql
WHERE (invoice_date > '2015-01-31' OR invoice_total > 500) AND invoice_total - payment_total - credit_total > 0
```

- `IN` operator can be used to form subquery.

`WHERE vendor_id IN (1, 3, 4)`

```sql
WHERE vendor_state NOT IN ('CA', 'NY', 'PH')
```

```sql
-- subquery
WHERE vendor_id IN 
    (SELECT vendor_id
    FROM invoices
    WHERE invoice_date = '2015-03-31')
```

- `BETWEEN...AND`: If the value falls within this range, the row is included in the query result.

```sql
WHERE invoice_date BETWEEN '2015-06-01' AND '2015-07-01'
```

```sql
WHERE zip NOT BETWEEN 387370 AND 400000
```

```sql
WHERE invoice_total - payment_total - credit_total BETWEEN 200 AND 500
```

- `LIKE` is used to find result set which is like this given expression.

- LIKE expression is used with special characters.

| Symbol | Description |
---------|:------------:|
| %     | matches any string of zero or more characters. |
| _     | matches any string character. |


**Examples:**

```sql
WHERE city LIKE 'DeL%'      -- matches 'Delhi' and 'Delware'

WHERE name LIKE 'COMPU_ER%'     -- matches computer and then any words. Computerworld, Computerzone, etc.
```

- `REGEXP` can be used to search for regular expression.


| Character | Description |
-----------|:-------------|
| ^         | matches the pattern to the beginning of the value being tested. |
| $         | matches the pattern to the end of the value being tested. |
| [charlist] | matches any single character listed within the brackets. |
| .         | matches any single character. |
| [char1-char2] | matches any single character within the given range. |
| |             | separates two string patterns and matches either one. |

```sql
WHERE city REGEXP '^SA'     -- starting with 'SA' case-insensitive

WHERE city REGEXP 'NA$'     -- string ending with 'NA' case-insensitive

WHERE city REGEXP 'SA'  -- city contains 'SA' case-insensitive

WHERE state REGEXP 'N[CV]'  -- city contains NC or NV but not NY

WHERE name REGEXP 'DAMI[EO]N'   -- matches 'Damien' or 'Damion'
```

- `IS NULL`: In MySQL, we can check for null value using `IS NULL`.

```sql
WHERE total IS NULL

WHERE address IS NOT NULL
```

- `ORDER BY` is used to reorder the result.

`ORDER BY expression [ASC|DESC] [, expression [ASC |DESC]...]`

```sql
SELECT vendor_name,CONCAT(vendor_city, ', ', vendor_state, ' ', vendor_zip) AS address 
FROM vendors
ORDER BY vendor_name;
```

- We can also sort by multiple fields.

```sql
SELECT vendor_name,CONCAT(vendor_city, ', ', vendor_state, ' ', vendor_zip) AS address 
FROM vendors
ORDER BY vendor_state, vendor_city, vendor_name;
-- sort by state, then city followed by name
```

- ORDER BY can also use alias that are created in the statement.

```sql
SELECT name, CONCAT(city, ', ', state) AS address
FROM vendors
ORDER BY address, name;
```

- We can also use result set's column number to sort by. The first column is specified as column 1.

```sql
SELECT vendor_name, CONCAT(vendor_city, ', ', vendor_state, ' ', vendor_zip_code) AS address
FROM vendors
ORDER BY 2, 1;
```

- `LIMIT` is used to specify the maximum number of rows that are returned in the result set. It can also be used to return range of rows.


```sql
SELECT id, total FROM invoices
ORDER BY total DESC
LIMIT 5
-- limit to only 5 results
```

```sql
SELECT id, vendor_id, invoice_total
FROM invoices
ORDER BY id
LIMIT 2, 5;
-- show 5 results from 3rd row.
```
