
# Lab03: Oracle 19c Security - Creating Password Profiles and Users

## Part 1: Creating a Password Profile

**Objective:** Learn how to create and apply password profiles in Oracle 19c.

### Step 1: Connect to the Database
Open PowerShell and connect to the database with a user who has the `CREATE PROFILE` privilege.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Create a Password Profile
Create a new password profile named `userprofile`.

```sql
create profile userprofile limit
failed_login_attempts 4
password_lock_time 2
password_life_time 180;
```

### Step 3: Apply the Profile to a User
Alter the user `scott` to use the newly created password profile.

```sql
alter user scott profile userprofile;
```

### Step 4: Alter the Default Password Profile
Modify the default profile's settings.

```sql
alter profile default limit
failed_login_attempts 4;
```

## Part 2: Creating Password-Authenticated Users

**Objective:** Learn how to create users with password authentication in Oracle 19c.

### Step 1: Connect to the Database
Connect to the database with a user who has the `CREATE USER` privilege.

```sql
sqlplus / as sysdba
ALTER SESSION SET CONTAINER = orclpdb;
```

### Step 2: Create a Basic User
Create a user `jessica` with a simple password.

```sql
create user jessica identified by oracle_1;
```

### Step 3: Create a User with a Complex Password
Create a user `tom` with a complex password.

```sql
create user tom identified by "Qax7UnP!123*";
```

### Step 4: Create a User with a Specific Password Profile
Create a user `steve` who uses the `userprofile`.

```sql
create user steve identified by test1 profile userprofile;
```

### Step 5: Create a User with a Forced Password Change on First Login
Create a user `john` who must change the password upon the first login.

```sql
create user john identified by password1 password expire;
```

### Step 6: Create a User with Specific Tablespaces and Quota
Create a user `richard` with specified tablespaces and quota.

```sql
create user richard identified by oracle_2 default
tablespace users temporary tablespace temp quota unlimited
on users;
```

### Step 7: Create a User with Specific Tablespaces and Quota
Create a user `ernesto` with specified tablespaces and quota.

```sql
create user ernesto identified by oracle_2 default
tablespace users temporary tablespace temp quota unlimited
on users;
```
