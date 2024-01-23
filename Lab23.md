
# Lab23: Changing Function Parameters and Adding a Column to a Redaction Policy in Oracle 19c

This lab involves altering an existing redaction policy to change its function parameters and adding a new column to the policy in Oracle 19c. Perform these steps using PowerShell on Windows.

## Part 1: Changing the Function Parameters for a Specified Column

1. **Connect to the Database as the secmgr User**
   Open PowerShell and enter:
   ```sql
   $ sqlplus secmgr
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```
   
2. **Alter the Policy EMP_POL**
   Change the function parameters for a specified column:
   ```sql
   SQL> BEGIN
       DBMS_REDACT.ALTER_POLICY(
       object_schema   => 'ernesto',
       object_name     => 'tbl',
       policy_name     => 'a_tbl_partial',
       action          => DBMS_REDACT.MODIFY_COLUMN,
       column_name     => 'a',
       function_type   => DBMS_REDACT.PARTIAL,
       function_parameters    => '9,1,4');
       END;
   /
   ```

3. **Connect as the User usr2 and View Data**
   ```sql
   SQL> connect usr2/oracle2
   SQL> select a from ernesto.tbl;
   ```

   After altering the policy, the column A in ernesto.tbl should now show redacted data as specified.

## Part 2: Adding a Column to the Redaction Policy

1. **Connect to the Database as the secmgr User**
   ```sql
   $ sqlplus secmgr
   ```

2. **Alter the EMP_POL Policy to Add a New Column**
   ```sql
   SQL> BEGIN
       DBMS_REDACT.ALTER_POLICY(
       object_schema   => 'HR',
       object_name     => 'EMPLOYEES',
       policy_name     => 'EMP_POL',
       action          => DBMS_REDACT.ADD_COLUMN,
       column_name     => 'COMMISSION_PCT',
       function_type   => DBMS_REDACT.FULL);
       END;
   /
   ```

3. **Connect as the User hr and Execute a Query**
   ```sql
   SQL> connect hr
   SQL> select employee_id, salary, commission_pct from hr.employees
        where commission_pct IS NOT NULL
        order by employee_id
        fetch first 10 rows only;
   ```

   After adding the new column to the policy, both the salary and commission_pct columns should be redacted.
