
# Lab08: Oracle 19c Security - The SYSBACKUP Privilege

## Objective
Understand the use of the `SYSBACKUP` privilege in Oracle 19c, its applications, and how to grant it.

## Database Authentication Steps

### Step 1: Connect to the Database as SYSDBA
Open PowerShell and connect to the database as a user with the `SYSDBA` role.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Grant the SYSBACKUP Privilege
Grant the `SYSBACKUP` privilege to user `tom`.

```sql
grant sysbackup to tom;
```

### Step 3: Verify the Grant in the Password File
Check if there is an entry in the password file for user `tom` with the `SYSBACKUP` administrative privilege. 

```sql
select * from v$pwfile_users;
```

#### Expected Result
The result should include a table showing various users and their privileges. Look for an entry similar to:

| Username | sysdb | sysop | sysas | sysba | sysdg | syskm | con_id |
|----------|-------|-------|-------|-------|-------|-------|--------|
| sys      | TRUE  | TRUE  | FALSE | FALSE | FALSE | FALSE | 0      |
| sysdg    | FALSE | FALSE | FALSE | FALSE | TRUE  | FALSE | 0      |
| sysbackup| FALSE | FALSE | FALSE | TRUE  | FALSE | FALSE | 0      |
| syskm    | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE  | 0      |
| tom      | FALSE | FALSE | FALSE | TRUE  | FALSE | FALSE | 0      |

### Step 4: Test Connection Using RMAN
Test the connection to the database using RMAN (Recovery Manager) as user `tom` with the `SYSBACKUP` privilege.

```sql
rman target '"tom/oracle_123 as sysbackup"'
```
