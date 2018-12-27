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

## COMMENT

- Used to provide useful comments, description

```sql
COMMENT ON TABLE <table_Name> IS 'comment details';
COMMENT ON TABLE emp IS 'Employee Details';
COMMENT ON COLUMN <table_name.col_name> IS 'column details';
```

To view the comments, we have to query data dictionary views `USER_TAB_COMMENTS` and `USER_COL_COMMENTS`.

## ALTER

- Used to change structure of the object.

```sql
ALTER TABLE <table_name>
[ADD | MODIFY | DROP | RENAME | SET UNUSED | ENABLE | DISABLE ] <other options>;

ALTER TABLE <table_name> ADD col_name DATA_TYPE;
ALTER TABLE <table_name> ADD (col1 DATATYPE1, col2 DATATYPE2);

ALTER TABLE <table_name> MODIFY <col_name> DATATYPE;

ALTER TABLE <table_name> DROP COLUMN <col_name>;

-- Hide the column
ALTER TABLE <table_name> SET UNUSED (col_name);
ALTER TABLE <table_name> DROP UNUSED COLUMNS;
```

```sql
INSERT INTO <table_name>(col1, col2, ...) VALUES (val1, val2, ...);
INSERT INTO Emp(ENo, DOB, EName, Sal) VALUES (1, '12-MAR-78', 'Unknown', 1000);
-- If colnames are more than values, then null values are inserted in last few columns
INSERT INTO Emp, VALUES(&ENo, &EName, &DOB, &Sal);


SELECT EName, ENo FROM Emp;
SELECT * FROM Emp;
SELECT Sal * 2 FROM Emp;

UPDATE Emp SET EName = 'all names'; -- All record names will be updated here
UPDATE Emp SET EName = 'abc', Sal = 2000 WHERE ENo=1;
UPDATE Emp SET EName = 'abc', Sal = 2000 WHERE ENo=1 AND Sal > 3000;

-- With DML command DELETE, rollback is possible
DELETE FROM Emp WHERE EName='abc';  -- delete all records where Employee Name is 'abc'
DELETE FROM Emp WHERE DOB='12-MAR-89';
```

MERGE command is available for supporting INSERT and UPDATE in single command. 

```sql
MERGE INTO Emp e
  USING (SELECT * FROM Emp2) b
  ON (e.ENo = b.ENo)
  WHEN MATCHED THEN 
    UPDATE SET a.EName = b.EName, a.Sal = b.Sal
  wHEN NOT MATCHED THEN
    INSERT (ENo, EName, DOB, Sal) VALUES (b.ENo, b.EName, b.DOB, b.Sal);
```

## DCL Commands

These are commands dealing with privileges only. These are like three step commands with commit before and after the command. These are also faster because they directly interact with database without buffer.

There can be system privileges or object privileges. System privileges are at higher level. We can get data from single table and restrict access to other tables. Object privileges are provided by database owners.

GRANT is used to provide privileges to other users for access to our objects.
REVOKE is used to revoke the privileges from other users.
SET ROLE to set role to specific user. We can create a role with different privileges and assign it to a user.

```sql
GRANT CONNECT, Resource TO USER IDENTIFIED BY Password;
SHOW USER -- user is u1
GRANT SELECT ON Emp TO u2; -- provide u2 user SELECT permission on Emp table.
-- Now u2 can run as SELECT DATA FROM u1.Emp; schema name is mandatory.
GRANT ALL ON Emp TO u2;
GRANT SELECT, INSERT ON Emp TO u2, u3;
-- If we want to pass these privileges to other user, we should get privileges through wITH GRANT option
-- Now u2 can get privileges but can also pass privileges to other user.
GRANT SELECT ON Emp TO u2 WITH GRANT OPTION
-- Now u2 can perform following operation
GRANT SELECT ON u1.Emp TO u3;
-- Now u3 can run following with schema owner name specified.
SELECT * FROM u1.Emp; -- run by u3 user

-- u1 want to revoke SELECT on Emp from u3, but he cannot remove it.
-- u1 can directly revoke SELECT from u2, which will revoke u3 also.
REVOKE SELECT ON Emp FROM u2;
REVOKE ALL ON Emp FROM u2;

CREATE ROLE r1 [NOT IDENTIFIED];
CREATE ROLE r1 IDENTIFIED BY password;

GRANT p1 [, p2, ...] TO r1;

GRANT r1 to u1, u2, ...;

ALTER ROLE r1 IDENTIFIED BY password; -- adding password
ALTER ROLE r1 NOT IDENTIFIED; -- remove password
ALTER USER u1 DEFAULT ROLE r1; -- setting default role to user

SET ROLE r1; -- set role to current user
SET ROLE ALL; -- set all roles to current user
SET ROLE ALL EXCEPT r1;
SET ROLE NONE;
SET ROLE r1 IDENTIFIED BY password;
```

## TCL commands

Within transaction, TCL commands are valid. These are group of DML comamnds with commit or rollback.