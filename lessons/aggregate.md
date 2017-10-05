# Aggregate Functions

Aggregate functions operate on the values in columns. So, they are referred to as column functions.

A query that contains one or more aggregate functions is typically called *summary query*.

We can use DISTINCT and ALL keywords to omit duplicate values.

```sql
SELECT SUM(DISTINCT vendor_id) FROM invoices;

SELECT SUM(ALL vendor_id) FROM invoices;
```

- AVG: average
- SUM: sum of all values.
- MIN: minimum of all values.
- MAX: maximum of all
- COUNT: numberof non-null values.

**Note:** All aggregate functions except COUNT(*) ignore null values.

```sql
SELECT 'After January' AS selection_date,
    COUNT (*) AS number_of_invoices,
    ROUND(AVG(invoice_total), 2) AS average_amount,
    SUM(invoice_total) AS total_amount
FROM invoices
WHERE invoice_date > '2015-31-31';
```

```sql
SELECT MIN(vendor_name) AS first_vendor,
    MAX(vendor_name) AS last_vendor,
    COUNT(vendor_name) AS number_of_vendors
FROM vendors;
```

```sql
SELECT COUNT (DISTINCT vendor_id) AS number_of_vendors,
    COUNT(vendor_id) AS number_of_invoices,
    ROUND(AVG(invoice_total), 2) AS avg_invoice_amount,
    SUM(invoice_total) AS total_invoice_amount
FROM invoices;
```

### GROUP BY

Data can be grouped using GROUP BY and HAVING keywords.
The HAVING clause specifies a search condition that applies on group and it is applied after GROUP BY clause.

```sql
SELECT vendor_id, ROUND(AVG(invoice_total),2) AS average
FROM invoices
GROUP BY vendor_id
HAVING AVG(invoice_total) > 2000
ORDER BY average DESC;
```

```sql
SELECT vendor_id, COUNT(*) AS invoice_qty
FROM invoices
GROUP BY vendor_id;
```

```sql
SELECT vendor_state, vendor_city, COUNT(*) AS invoice_qty,
    ROUND(AVG(invoice_total),2) AS invoice_qty
FROM invoices JOIN vendors
    ON invoices.vendor_id = vendors.vendor_id
GROUP BY vendor_state, vendor_city
HAVING COUNT(*) >= 2;
```

```sql
SELECT vendor_name,COUNT(*) AS invoice_qty, ROUND(AVG(invoice_total), 2) AS invoice_avg
FROM vendors JOIN invoices 
    ON vendors.vendor_id = invoices.vendor_id
WHERE invoice_total > 500
GROUP BY vendor_name
ORDERBY invoice_qty DESC;
```

```sql
SELECT invoice_date, COUNT(*) AS invoice_qty, SUM(invoice_total) AS invoice_sum
FROM invoices
WHERE invoice_date BETWEEN '2016-01-01' AND '2016-06-31'
GROUP BY invoice_date
HAVING COUNT(*) > 1 AND SUM(invoice_total) > 100
ORDER BY invoice_date DESC;
```

