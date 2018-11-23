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

1. [SELECT statement](lessons/mysql/select.md)
2. [JOIN two tables](lessons/mysql/join.md)
3. [INSERT, UPDATE, DELETE](lessons/mysql/modify.md)
4. [Aggregate functions](lessons/mysql/aggregate.md)
5. [Subqueries](lessons/mysql/subquery.md)


# Oracle SQL

Oracle Database has 10g, 11g which stands for grid computing where multiple disks are available. 12c means cloud which can be accessed from cloud.

**Data Definition Language**

- will be interacting with database directly. Because of this, there will be implicit commit before and after the command. So, this is three step process. So, rollback is not possible with such statements.
- They are usually much faster compared to other types of commands. So, TRUNCATE is faster than DELETE statement. DML statements interact with buffer. That's why they are comparatively slower.
- Some of the DDL commands, CREATE, ALTER, TRUNCATE, RENAME, DROP, FLASHBACK, PURGE, COMMENT

**Data Manipulation Language**

- These are used for data structure. It first interacts with buffer. When we commit, buffer content will be loaded into database. If we rollback, the buffer content will be removed. Here, rollback is possible. These can be read (SELECT) or write (INSERT, UPDATE, DELETE, MERGE). Read operations are Data retrieval commands.
- There will be implicit row level locks. So, other users cannot manipulate that row or record until the lock is released.

**Transaction Control Language**

These are used for transactions. A set of DML operations with commit or rollback is called transaction. Starting from DML to commit/rollback is called a transaction. These incldues COMMIT, ROLLBACK, SVAEPOINT (temporary saving point), SET TRANSACTION (change mode of transaction)

**Data Control Language**

DCL commands also interacts with Database directly. They deal with privileges only. There will be implicit commit in DCL as well. These incldues GRANT, REVOKE, SET ROLE.

1. [DDL Comamnds](lessons/oracle/ddl.md)