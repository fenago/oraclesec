
# Lab16: Oracle 19c Security - Creating and Using Invoker's Rights Procedures

## Objective
Learn how to create and use invoker's rights procedures in Oracle 19c.

### Step 1: Connect as a User with DBA Role
Connect to the database as a user with the DBA role (e.g., `ernesto`).

```sql
SQL> connect ernesto
```

### Step 2: Create Users and Grant Privileges
Create users `procuser1` and `procuser2`, and grant them necessary privileges.

```sql
SQL> create user procuser1 identified by oracle1;
SQL> create user procuser2 identified by oracle2;
SQL> grant create session to procuser1;
SQL> grant create session to procuser2;
```

### Step 3: Create a Table and Grant Privileges
Create a table `table1` and grant select and update privileges to `procuser1`, and select privilege to `procuser2`.

```sql
SQL> create table table1(a number, b varchar2(30));
SQL> insert into table1 values(1, 'old_value');
SQL> commit;
SQL> grant select, update on table1 to procuser1;
SQL> grant select on table1 to procuser2;
```

### Step 4: Create an Invoker's Rights Procedure
Create a procedure with invoker's rights to update `table1`.

```sql
CREATE OR REPLACE PROCEDURE UpdateTable1 (x IN number,
       y IN varchar2)
  AUTHID CURRENT_USER
    AS
      BEGIN
       UPDATE TABLE1
       SET b = y
        WHERE a = x;
    END;
  /
```

### Step 5: Grant Execute on Procedure
Grant execute permission on `UpdateTable1` to `procuser1` and `procuser2`.

```sql
SQL> grant execute on UpdateTable1 to procuser1;
SQL> grant execute on UpdateTable1 to procuser2;
```

### Step 6: Execute Procedure as procuser1
Connect as `procuser1` and execute the `UpdateTable1` procedure.

```sql
SQL> connect procuser1
SQL> EXEC UpdateTable1(1, 'new_value');
-- Expect "PL/SQL procedure successfully completed."
SQL> commit;
```

### Step 7: Verify Table Update
Check if the table is updated.

```sql
SQL> select * from table1;
-- Expect to see the updated value: new_value
```

### Step 8: Attempt Execution as procuser2
Connect as `procuser2` and try to execute `UpdateTable1`.

```sql
SQL> connect procuser2
SQL> EXEC UpdateTable1(1, 'newer_value');
-- Expect an ORA-01031 error due to insufficient privileges
```
