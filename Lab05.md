
# Lab05: Oracle 19c Security - Locking and Expiring User Accounts

## Part 1: Locking a User Account

### Objective
Learn how to lock and unlock a user account in Oracle 19c.

#### Step 1: Connect to the Database
Open PowerShell and connect to the database as a user with `ALTER USER` privilege.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

#### Step 2: Lock a User Account
Lock the account of user `steve`.

```sql
SQL> alter user steve account lock;
```

#### Step 3: Unlock a User Account
If needed, unlock the account of user `steve`.

```sql
SQL> alter user steve account unlock;
```

## Part 2: Expiring a User's Password

### Objective
Learn how to expire a user's password in Oracle 19c.

#### Step 1: Connect to the Database
Connect to the database as a user with the `ALTER USER` privilege.

```sql
$ sqlplus /
```

#### Step 2: Expire a User's Password
Expire the password of user `steve`.

```sql
SQL> alter user steve password expire;
```
