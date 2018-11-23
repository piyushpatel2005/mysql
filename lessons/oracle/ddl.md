# DDL statements

## CREATE statements

- Used to create tables

```sql
CREATE TABLE Emp
(
  EmpNo NUMBER(5),
  EName VARCHAR(5),
  DOB DATE,
  Sal NUMBER(7,2)
);

-- CREATE table from existing table CTAS
CREATE TABLE Emp1 AS SELECT * FROM Emp;

-- Select only few columns from existing table
CREATE TABLE Emp1 AS SELECT
  ENo, EName 
  FROM Emp;

-- Create only structure of a table without data
CREATE TABLE Emp1 AS SELECT * FROM Emp WHERE 1 = 2;
```

## TRUNCATE

- Used to remove table data

```sql
TRUNCATE TABLE Emp;
```

## RENAME

- USed to change name of table, column or name of constraint

```sql
-- Rename table name
RENAME <old_table_name> TO <new_table_name>;
RENAME Emp to Employee;

-- Rename column name
ALTER TABLE <table_name> RENAME COLUMN <old_col> TO <new_col>;
ALTER TABLE Emp RENAME COLUMN EmpNo to ENo;

-- Rename constraint
ALTER TABLE <table_name> RENAME CONSTRAINT <old_constraint> TO <new_constraint>;
ALTER TABLE Emp RENAME CONSTRAINT PK_Emp TO P_Emp;
```

## DROP

- Used to drop the table. It will go to trash.

```sql
DROP TABLE Emp;

-- Drop column
ALTER TABLE Emp DROP(Sal, Comm);
```

## FLASHBACK

- Used to see the recycle bin and to restore tables from recycle bin

```sql
SHOW RECYCLEBIN;
SELECT * FROM RECYCLEBIN;
FLASHBACK TABLE <table_name> TO BEFORE DROP;

-- Retrieve table with different name
FLASHBACK TABLE <table_name> TO BEFORE DROP RENAME TO <another_name>;
```

## PURGE

- Used to remove objects from recycle bin permanently.

```sql
PURGE TABLE <table_name>;

-- Drop table and purge the table in single command
DROP TABLE <table_name> PURGE;
```