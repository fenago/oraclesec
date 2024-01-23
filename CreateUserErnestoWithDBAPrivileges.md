
# Lab: Oracle 19c - Creating a User with DBA Privileges

## Objective
Learn how to create a new user named `ernesto` and grant them DBA privileges in Oracle 19c.

### Step 1: Connect as SYSDBA
Open PowerShell and connect to the Oracle database as the SYSDBA.

```sql
sqlplus / as sysdba
```

### Step 2: Create New User Named 'ernesto'
Create a new user named `ernesto` with a designated password (e.g., `oracle123`).

```sql
CREATE USER ernesto IDENTIFIED BY oracle123;
```

### Step 3: Grant DBA Privileges to 'ernesto'
Grant the `DBA` role to the newly created user `ernesto`.

```sql
GRANT DBA TO ernesto;
```

### Step 4: Verify the Grant (Optional)
Optionally, you can verify that `ernesto` has been granted the `DBA` role.

```sql
SELECT grantee, granted_role FROM dba_role_privs WHERE grantee = 'ERNESTO';
```

This lab demonstrates the basic steps to create a new user with DBA privileges in an Oracle 19c database.
