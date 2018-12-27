# Oracle 12c Database

After installing Oracle 12c, start oracle to create tables and schemas.

```shell
sqlplus
Enter user-name: sys as sysdba
Enter password: password
```

```sql
-- In Oracle DB, the default database is the ROOT container database with name CDB$ROOT. You can view using following command.
SHOW CON_NAME;

-- Switch to pluggable database, use the ALTER SESSION statement to set the current database to the pluggable database. PDBORCL.
ALTER SESSION SET CONTAINER=orclpdb;
SHOW CON_NAME;
-- see open_mode for database
select name, open_mode from v$pdbs;
-- If orclpdb is not in open mode, we can open it using following
alter pluggable database open;
-- create user and unlock it
create user scott identified by tiger
alter user scott identified by tiger account unlock;
conn scott/tiger@orclpdb;
select * from tab;
-- shows name and id of containers
SELECT name, con_id FROM v$pdbs;
-- clear screen
cle scr;
-- find services for container id 3 which is pluggable database
select name from v$active_services where con_id=3;
-- Edit tnsnames.ora with addition of pluggable db
-- Log out and login again
sqlplus / as sysdba
-- Create username OT with password
CREATE USER OT IDENTIFIED BY Orcl1234;
-- If you get error, THEN open datbase as follows.
ALTER DATABASE OPEN;
CREATE USER OT IDENTIFIED BY Orcl1234;
GRANT CONNECT, RESOURCE, DBA TO OT;

CONNECT OT@orclpdb;

EXIT;
