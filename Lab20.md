
# Lab20: Creating a Redaction Policy with Partial Redaction in Oracle 19c

This lab guides you through the steps to create and apply a redaction policy using partial redaction in Oracle 19c. This lab should be performed using PowerShell on a Windows system.

## Steps

1. **Log in to the database as a user with a DBA role**  
   Open PowerShell and enter:  
   ```sql
   $ sqlplus ernesto/oracle
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```

2. **Create a test table and insert some data**  
   ```sql
   SQL> create table tbl (a number);
   SQL> insert into tbl values (123456);
   SQL> insert into tbl values (234567);
   SQL> insert into tbl values (345678);
   SQL> commit;
   ```

3. **Create a role and the first test user**  
   ```sql
   SQL> create role myrole;
   SQL> create user usr1 identified by oracle1;
   SQL> grant create session to usr1;
   ```

4. **Grant privileges to usr1**  
   ```sql
   SQL> grant select on ernesto.tbl to usr1;
   SQL> grant myrole to usr1;
   ```

5. **Create the second test user and grant privileges**  
   ```sql
   SQL> create user usr2 identified by oracle2;
   SQL> grant create session to usr2;
   SQL> grant select on ernesto.tbl to usr2;
   ```

6. **Create a redaction policy for partial redaction**  
   ```sql
   SQL> BEGIN
       DBMS_REDACT.ADD_POLICY(
       object_schema          => 'ernesto',
       object_name            => 'tbl',
       column_name            => 'a',
       column_description     => 'Sensitive column A',
       policy_name            => 'a_tbl_partial',
       policy_description     => 'Redact column A of tbl',
       function_type          => DBMS_REDACT.PARTIAL,
       function_parameters    => '0,1,4',
       expression             => 'SYS_CONTEXT("SYS_SESSION_ROLES", "MYROLE") = "FALSE"');
       END;
   /
   ```

7. **Test the policy with usr1**  
   ```sql
   SQL> connect usr1/oracle1
   SQL> select a from ernesto.tbl;
   ```

8. **Test the policy with usr2**  
   ```sql
   SQL> connect usr2/oracle2
   SQL> select a from ernesto.tbl;
   ```

9. **Log in as a DBA user to create a new test table**  
   ```sql
   $ sqlplus ernesto/oracle
   SQL> create table customers (name varchar2(20 CHAR), credit_card varchar2(20 CHAR));
   SQL> insert into customers values ('tom', '3455647456589132');
   SQL> insert into customers values ('steve', '3734982321225691');
   SQL> insert into customers values ('john', '3472586894975806');
   SQL> commit;
   ```

10. **Grant select privilege on the new table to usr1**  
    ```sql
    SQL> grant select on ernesto.customers to usr1;
    ```

11. **Create a redaction policy for the new table**  
    ```sql
    SQL> BEGIN
        DBMS_REDACT.ADD_POLICY(
        object_schema          => 'ernesto',
        object_name            => 'customers',
        column_name            => 'credit_card',
        column_description     => 'Credit Card numbers',
        policy_name            => 'CCN_POLICY',
        policy_description     => 'Redact column credit_card of table customers',
        function_type          => DBMS_REDACT.PARTIAL,
        function_parameters    => 'VVVVVVVVVVVVVVVV, VVVVVVVVVVVVVVVV, #, 1, 12',
        expression             => '1=1');
        END;
    /
    ```

12. **Test the new policy with usr1**  
    ```sql
    SQL> connect usr1/oracle1
    SQL> select * from ernesto.customers;
    ```
