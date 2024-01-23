
# Lab15: Oracle 19c Security - Creating and Using Definer's Rights Procedures

## Objective
Learn how to create and use definer's rights procedures in Oracle 19c.

### Step 1: Connect as a User with DBA Role
Connect to the database as a user with the DBA role (e.g., `ernesto` with `SYSDBA` role).

```sql
SQL> connect ernesto
```

### Step 2: Create Users and Grant Privileges
Create two users `procowner` and `procuser`, and grant them necessary privileges.

```sql
SQL> create user procowner identified by oracle1;
SQL> create user procuser identified by oracle2;
SQL> grant create session, create procedure to procowner;
SQL> grant create session to procuser;
```

### Step 3: Create a Table and Grant Privileges
Create a table `ernesto.tbl` and grant privileges to the users.

```sql
SQL> create table ernesto.tbl(a number, b varchar2(40));
SQL> insert into ernesto.tbl values(1, 'old_value');
SQL> commit;
SQL> grant select on ernesto.tbl to procuser;
SQL> grant update on ernesto.tbl to procowner;
```

### Step 4: Create and Grant Execute on Procedure
Connect as `procowner`, create a procedure to update `ernesto.tbl`, and grant execute permission to `procuser`.

```sql
SQL> connect procowner/oracle1
CREATE OR REPLACE PROCEDURE UpdateTbl (x IN number,
       y IN varchar2)
     AUTHID DEFINER
       AS
         BEGIN
          UPDATE ERNESTO.TBL
          SET b = y
           WHERE a = x;
       END;
     /
SQL> grant execute on UpdateTbl to procuser;
```

### Step 5: Attempt Direct Update as procuser
Connect as `procuser` and try to directly update `ernesto.tbl`.

```sql
SQL> connect procuser/oracle2
SQL> UPDATE ERNESTO.TBL SET B = 'value1' WHERE A = 1;
-- Expect an ORA-01031 error due to insufficient privileges
```

### Step 6: Update Table Using Procedure
Use the `UpdateTbl` procedure to update the table.

```sql
SQL> EXEC procowner.UpdateTbl(1, 'new_value');
-- Expect "PL/SQL procedure successfully completed."
```

### Step 7: Verify Table Update
Check if the table is updated.

```sql
SQL> select * from ernesto.tbl;
-- Expect to see the updated value: new_value
```
