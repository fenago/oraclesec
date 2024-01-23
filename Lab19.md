
# Lab 19: Data Redaction in Oracle 19c

This lab demonstrates how to create a redaction policy for full data redaction in Oracle 19c. This lab should be conducted on a Windows system using PowerShell.

## Steps

1. **Connect to the Database as a Privileged User**  
   Connect to the database as a user with sufficient privileges (e.g., the `oe` user). Open PowerShell and enter:
   ```
   $ sqlplus oe
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```

   **Explanation**: 
    This command sets the current session to use the pluggable database `orclpdb.

2. **Verify Data Visibility**  
   Check if the user can view data in the `OE.CUSTOMERS` table:
   ```sql
   select customer_id, cust_last_name, income_level from oe.customers order by customer_id fetch first 10 rows only;
   ```
   Note the data in clear text format before redaction.

3. **Create and Grant Privileges to the Secmgr User**  
   Connect as a user who can create users (e.g., `SYS`) and create the `secmgr` user:
   ```sql
   SQL> create user secmgr identified by oracle;
   SQL> grant create session to secmgr;
   SQL> grant execute on dbms_redact to secmgr;
   ```

4. **Connect as the Secmgr User**  
   Now, connect to the database as the `secmgr` user:
   ```sql
   SQL> connect secmgr/oracle
   ```

5. **Create the Redaction Policy**  
   Create the redaction policy `CUST_POL` for the `INCOME_LEVEL` column in the `OE.CUSTOMERS` table using full redaction:
   ```sql
   SQL> begin
   2    dbms_redact.add_policy
   3      (object_schema => 'OE',
   4       object_name => 'CUSTOMERS',
   5       policy_name => 'CUST_POL',
   6       column_name => 'INCOME_LEVEL',
   7       function_type => DBMS_REDACT.FULL,
   8       expression => '1=1');
   9    end;
   10  /
   ```
   You should see "PL/SQL procedure successfully completed."

6. **Verify the Redaction Policy**  
   Reconnect as the initial user (e.g., `oe`) and execute the same query as in step 2 to observe the effects of the redaction policy.

