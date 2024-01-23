
# Lab17: Oracle 19c Security - Using Code-Based Access Control

## Objective
Learn how to implement and use code-based access control in Oracle 19c.

### Step 1: Create User and Grant Privileges
Connect as a DBA user (e.g., `ernesto`) and create `proc_user`, granting the `CREATE SESSION` privilege.

```sql
SQL> create user proc_user identified by oracle1;
SQL> grant create session to proc_user;
```

### Step 2: Create Table and Insert Data
Create a table `tbl1` and insert test data into it.

```sql
SQL> create table tbl1(a number, b varchar2(30));
SQL> insert into tbl1 values (1, 'old_value');
SQL> commit;
```

### Step 3: Create Invoker's Rights Procedure
Create the invoker's rights procedure `UpdateTbl1` and grant execute permission to `proc_user`.

```sql
CREATE OR REPLACE PROCEDURE UpdateTbl1 (x IN number,
       y IN varchar2)
  AUTHID CURRENT_USER
    AS
      BEGIN
       UPDATE TBL1
       SET b = y
        WHERE a = x;
    END;
    /
SQL> grant execute on UpdateTbl1 to proc_user;
```

### Step 4: Create Role and Grant Privileges
Create the role `proc_role` and grant update privilege on `tbl1` to `proc_role`.

```sql
SQL> create role proc_role;
SQL> grant update on tbl1 to proc_role;
```

### Step 5: Grant Role to Procedure
Grant the role `proc_role` to the procedure `UpdateTbl1`.

```sql
SQL> grant proc_role to procedure UpdateTbl1;
```

### Step 6: Connect as proc_user
Connect to the database as `proc_user`.

```sql
SQL> connect proc_user
```

### Step 7: Attempt Direct Update
Try to directly update `tbl1` as `proc_user`.

```sql
SQL> update tbl1 set b = 'value1' where a = 1;
-- Expect an ORA-00942 error due to insufficient privileges
```

### Step 8: Execute Procedure
Execute the procedure `UpdateTbl1` as `proc_user`.

```sql
SQL> execute UpdateTbl1(1, 'new_value');
-- Expect "PL/SQL procedure successfully completed."
```

### Step 9: Verify Table Update
Connect as `ernesto` and verify if `tbl1` is updated.

```sql
SQL> connect ernesto
SQL> select * from tbl1;
-- Expect to see the updated value: new_value
```
