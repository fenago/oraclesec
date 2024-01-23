
# Lab06: Oracle 19c Security - Creating and Using Proxy Users

## Objective
Learn how to create and manage proxy users in Oracle 19c.

### Step 1: Connect to the Database as a DBA
Open PowerShell and connect to the database as a user with the DBA role.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Create a Proxy User
Create a proxy user named `appserver`.

```sql
SQL> create user appserver identified by oracle_1;
```

### Step 3: Grant Create Session to the Proxy User
Grant the `CREATE SESSION` privilege to `appserver`.

```sql
SQL> grant create session to appserver;
```

### Step 4: Allow a User to Connect Through the Proxy User
Alter the user `steve` to enable connection through the proxy user `appserver`.

```sql
SQL> alter user steve grant connect through appserver;
```

### Step 5: Connect to the Database Through Proxy User
Connect to the database using the proxy user credentials.

```sql
SQL> connect appserver[steve]@localhost:1521/orclpdb
```

### Step 6: Enter Password for the Proxy User
Enter the password for the proxy user `appserver` (e.g., `oracle_1`).

```sql
Enter password:
```

### Step 7: Connect as a DBA to Revoke Proxy
Connect to the database as a DBA to revoke the proxy settings.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 8: Revoke Connection Through the Proxy User
Revoke the connection through the proxy user `appserver` from user `steve`.

```sql
SQL> alter user steve revoke connect through appserver;
```
