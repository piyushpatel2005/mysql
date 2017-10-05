# MySQL

- open-source database.

In relational database model, data is stored in one or more columns and each table can be viewed as two dimensional matrix.

In practice, rows and columns of relational database are called records and fields.

If table contains one or more columns that uniquely identifies each row in the table, these tables can be defined as *primary key*. Sometimes, more than one column can be used to create *composite primary key*.

**Indexes** provide an efficient way of accessing the rows in a table based on the values in one or more columns.

Tables in one each row is related to one or more rows in another table using one-to-many relationship. Typically, relationships exist between the primary key in one table and the *foreign key* in another table.

One-to-one relationship is quite common. Many-to-many relationship is implemented using intermediate table. 

When you define foreign key for a table, you can have the foreign key enforce **referential integrity**. It makes sure that any changes to the data in database don't create invalid relationships between tables.


MySQL has various datatypes that makes sure the type of the data in a column.

CHAR, VARCHAR - string
INT, DECIMAL - Integer and decimal numbers that contain exact value
FLOAT - Floating point numbers that contain approximate value
DATE - Dates and times

**Entity-relationship diagram** shows how the tables in a database are defined and related. You can use enhanced version called *EER diagram* for enhanced entity-relationships.


To install MySQL Server, visit the official website.

For Linux, you can use apt-get to install:

`apt-get install mysql-server-5.7`

You can also install MySQL workbench using:

`apt-get install mysql-workbench`

To start MySQL using console, use:

`mysql -u root -p`

This will prompt you for password that you setup when installation MySQL server.

You can also specify host using `-h` flag.

`mysql -h hostname -u username -p`



**Table of Contents**

1. [SELECT statement](lessons/select.md)
2. [JOIN two tables](lessons/join.md)
3. [INSERT, UPDATE, DELETE](lessons/modify.md)
4. [Aggregate functions](lessons/aggregate.md)
5. [Subqueries](lessons/subquery.md)