# JOIN

JOIN allows to join two tables in the database.

```sql
SELECT invoice_number, vendor_name FROM vendors INNER JOIN invoices ON vendors.vendor_id = invoices.vendor_id
ORDER BY invoice_number;
-- if two columns have the same name, they are specified using dot(.)
```

Tables are typically joined on the relationship between the primary key in one table and a foreign key in the other table. The INNER keyword is optional.

**Table alias** is alternative table name assigned in the FROM clause. 

```sql
SELECT invoice_number, vendor_name, invoice_due_date
FROM vendors v JOIN invoices i
ON v.vendor_id = i.vendor_id
-- Here, v and i are aliases for vendors and invoices respectively.
-- These aliases are optional but they make writing query easy by shortening table name.
```

- JOIN a table in another database

```sql
SELECT vendor_name, customer_name, vendor_state AS state, vendor_city AS city
FROM vendors v
JOIN clients.customers c
ON v.vendor_zip_code = c.customer_zip
ORDER BY state, city;
```

- We can also make compound JOIN using AND or OR operations.

```sql
SELECT c.first_name, c.last_name FROM customers c JOIN employees e
ON c.first_name = e.first_name
AND c.last_name = e.last_name;
```

A **self join** joins a table to itself.

```sql
SELECT DISTINCT v1.vendor_name, v1.vendor_city
FROM vendors v1 JOIN vendors v2
ON v1.vendor_city = v2.vendor_city;
```

- We can also join multiple tables.

```sql
SELECT vendor_name, invoice_number, invoice_date, account_description
FROM vendors v
JOIN invoices i
    ON v.vendor_id = i.vendor_id
JOIN accounts a
    ON a.account_number = i.account_number;
```

**Implicit joins** don't use JOIN keyword.

```sql
SELECT invoice_number, vendor_name
FROM vendors v, invoices i
WHERE v.vendor_id = i.vendor_id
ORDER BY invoice_number;
```

- Join multiple tables.

```sql
SELECT vendor_name, invoice_number, invoice_date, account_description
FROM vendors v, invoices i, accounts a
WHERE v.vendor_id = i.vendor_id
    AND a.account_number = i.account_number;
```

### Outer join

- Outer join returns all the rows from all the tables involved in the join unlike inner join.

They are LEFT and RIGHT which means full LEFT (first) table or full RIGHT (second) table. OUTER keyword is optional is typically omitted.


```sql
SELECT vendor_name, invoice_number, invoice_total 
FROM vendors LEFT JOIN invoices
    ON vendors.vendor_id = invoices.vendor_id;
```

- `USING` is used to make equijoins. In JOINs, usually two tables will have the same column name which are compared. So, the join can be simplied using `USING` keyword to specify the column name on which the join is made.

```sql
SELECT invoice_number, vendor_name
FROM vendors
    JOIN invoices USING (vendor_id)
ORDER BY invoice_number;
```

- Multiple table joins

```sql
SELECT department_name, last_name, project_number
FROM departments
    JOIN employees USING (department_number)
    LEFT JOIN projects USING (employee_id)
ORDER BY department_name;
```

- `NATURAL` JOINS are used to code a natural join. When coding natural join, you don't specify the column that's used to join the two tables. The database automatically joins the two tables based on all columns in the two tables that have the same name.

**Note:** Natural joins are not recommended to use.

```sql
SELECT invoice_number, vendor_name
FROM vendors
    NATURAL JOIN invoices
```

A **cross join** produces a result set that includes each row from the first table joined with each row from the second table. This is also known as *Cartesian product* of the table.

```sql
SELECT vendor_id, vendor_name, invoice_id 
FROM vendors
CROSS JOIN invoices;
```

## Unions

A **union** combines data from two or more tables. Union combines rows from two or more result sets. By default, union operation removes duplicate rows from the result set. If you want all results, use UNION ALL. For this UNION to work, the result set of each SELECT statement must have the *same number of columns and the data types* of each table must be compatible.

```sql
SELECT 'Active' AS source, invoice_number, invoice_date, invoice_total
FROM active_invoices
WHERE invoice_date >= '2016-06-01'
UNION [ALL]
SELECT 'Paid' AS source, invoice_number, invoice_date, invoice_total
FROM paid_invoices
WHERE invoice_date >= '2016-06-01'
ORDER BY invoice_total DESC;
```

## FULL OUTER JOIN

MySQL doesn't provide support for full outer join, but that can be simulated using LEFT, RIGHT JOIN and UNION.

```sql
SELECT department_name AS dept_name, e.department_number AS e_dept_no, last_name
FROM departments d
    LEFT JOIN employees e
    ON d.department_number = e.department_number
UNION
    SELECT department_name AS dept_name,
    e.department_number as e_dept_no, last_name
    FROM department d
        RIGHT JOIN employees e
        ON d.department_number = e.department_number;
```


