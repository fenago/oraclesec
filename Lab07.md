
# Lab07: Oracle 19c Security - Creating and Using Database Roles

## Objective
Learn how to create and manage database roles in Oracle 19c.

### Step 1: Connect to the Database as a DBA
Open PowerShell and connect to the database as a user with the DBA role.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Create a Role
Create a role named `usr_role`.

```sql
SQL> create role usr_role;
```

### Step 3: Grant System Privilege to the Role
Grant the `CREATE SESSION` system privilege to `usr_role`.

```sql
SQL> grant create session to usr_role;
```

### Step 4: Grant Object Privileges to the Role
Grant `SELECT` and `INSERT` privileges on the `hr.employees` table to `usr_role`.

```sql
SQL> grant select, insert on hr.employees to usr_role;
```

### Step 5: Create Another Role
Create another role named `mgr_role`.

```sql
SQL> create role mgr_role;
```

### Step 6: Grant One Role to Another
Grant `usr_role` to `mgr_role`.

```sql
SQL> grant usr_role to mgr_role;
```

### Step 7: Grant System Privileges to the Second Role
Grant the `CREATE TABLE` system privilege to `mgr_role`.

```sql
SQL> grant create table to mgr_role;
```

### Step 8: Grant Object Privileges to the Second Role
Grant `UPDATE` and `DELETE` privileges on the `hr.employees` table to `mgr_role`.

```sql
SQL> grant update, delete on hr.employees to mgr_role;
```

### Step 9: Grant the First Role to a User
Grant `usr_role` to user `steve`.

```sql
SQL> grant usr_role to steve;
```

### Step 10: Grant the Second Role to Another User
Grant `mgr_role` to user `tom`.

```sql
SQL> grant mgr_role to tom;
```
