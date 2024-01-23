
# Lab21: Creating a Redaction Policy Using Random Redaction in Oracle 19c

This lab demonstrates how to create a redaction policy using random redaction in Oracle 19c. You will use PowerShell on Windows to perform these tasks.

## Steps

1. **Connect to the Database as a User with SELECT Privilege**
   Open PowerShell and enter:
   ```sql
   $ sqlplus hr
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```

2. **Verify Data Visibility**
   Execute the following query to view data in clear text format:
   ```sql
   select employee_id, salary, commission_pct from hr.employees
   where commission_pct IS NOT NULL
   order by employee_id
   fetch first 10 rows only;
   ```

3. **Connect as the secmgr User**
   ```sql
   SQL> connect secmgr/oracle
   ```

4. **Create the Redaction Policy EMP_POL**
   This policy applies random redaction to the salary column when accessed by the user from step 1 (e.g., hr):
   ```sql
   SQL> begin
       dbms_redact.add_policy(
       object_schema => 'HR',
       object_name => 'EMPLOYEES',
       policy_name => 'EMP_POL',
       column_name => 'SALARY',
       function_type => DBMS_REDACT.RANDOM,
       expression => 'SYS_CONTEXT("USERENV", "SESSION_USER") = "HR"');
       end;
   /
   ```

5. **Test the Redaction Policy**
   Connect to the database as the same user as in step 1 (e.g., hr) and execute the same query as in step 2, twice, to observe the redaction policy in action.

   ```sql
   SQL> connect hr
   SQL> select employee_id, salary, commission_pct from hr.employees
        where commission_pct IS NOT NULL
        order by employee_id
        fetch first 10 rows only;
   ```

   After applying the redaction policy, the salary column data should appear randomly redacted.
