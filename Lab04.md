
# Lab04: Oracle 19c Security - Changing a User's Password

## Objective
Learn how to change a user's password in Oracle 19c.

### Step 1: Connect to the Database as a Privileged User
Open PowerShell and connect to the database as a user with `ALTER USER` privilege.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Change the Password for a Specific User
Change the password for user `jessica`.

```sql
SQL> password jessica;
```

### Step 3: Enter New Password
Type the new password (e.g., `oracle_2`). Note: Typing will not be visible in the command line.

```sql
New password:
```

### Step 4: Confirm New Password
Retype the new password for confirmation.

```sql
Retype new password:
```

### Step 5: Connect as a Regular User
Connect to the database as a regular user (e.g., `tom`) to change their own password.

```sql
$ sqlplus tom/"Qax7UnP!123*"
```

### Step 6: Initiate Password Change
Initiate the password change process.

```sql
SQL> password
```

### Step 7: Enter Old Password
Enter the old password. Typing will not be visible.

```sql
Old password:
```

### Step 8: Enter New Password
Type the new password (e.g., `oracle_123`). Note: Typing will not be visible.

```sql
New password:
```

### Step 9: Confirm New Password
Retype the new password for confirmation.

```sql
Retype new password:
```
