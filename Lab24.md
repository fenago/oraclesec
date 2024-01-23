
# Lab24: Enabling, Disabling, and Dropping Redaction Policy in Oracle 19c

This lab guides you through enabling, disabling, and dropping a redaction policy in Oracle 19c. Perform these steps using PowerShell on Windows.

## Steps

1. **Connect to the Database as a User with Necessary Privileges**
   Open PowerShell and enter:
   ```sql
   $ sqlplus secmgr
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```
   
2. **Identify Existing Redaction Policies**
   Query the `redaction_policies` view:
   ```sql
   SQL> col policy_name format A20
   SQL> select policy_name, enable from redaction_policies;
   ```

3. **Grant SELECT Privilege and Verify Redacted Data**
   As the `oe` user, grant the SELECT privilege to the `secmgr` user and then verify that redacted data is displayed:
   ```sql
   SQL> connect oe
   SQL> grant select on oe.customers to secmgr;
   SQL> connect secmgr
   SQL> select customer_id, cust_last_name, income_level from oe.customers
        order by customer_id
        fetch first 10 rows only;
   ```

4. **Disable the Redaction Policy CUST_POL**
   As the `secmgr` user, disable the policy:
   ```sql
   SQL> begin
       dbms_redact.disable_policy(
       object_schema => 'OE',
       object_name => 'CUSTOMERS',
       policy_name => 'CUST_POL');
       end;
   /
   ```

5. **Verify Original Data Visibility and Policy Status**
   Verify that the `secmgr` user can now view original data and check the policy status:
   ```sql
   SQL> select customer_id, cust_last_name, income_level from oe.customers
        order by customer_id
        fetch first 10 rows only;
   SQL> col policy_name format A20
   SQL> select policy_name, enable from redaction_policies;
   ```

6. **Enable the Redaction Policy CUST_POL**
   Re-enable the policy:
   ```sql
   SQL> begin
       dbms_redact.enable_policy(
       object_schema => 'OE',
       object_name => 'CUSTOMERS',
       policy_name => 'CUST_POL');
       end;
   /
   ```

7. **Verify Redaction is Working Properly**
   Check if the data is redacted and the policy is enabled:
   ```sql
   SQL> select customer_id, cust_last_name, income_level from oe.customers
        order by customer_id
        fetch first 10 rows only;
   SQL> col policy_name format A20
   SQL> select policy_name, enable from redaction_policies;
   ```

8. **Drop the Redaction Policy CUST_POL**
   Remove the policy:
   ```sql
   SQL> begin
       dbms_redact.drop_policy(
       object_schema => 'OE',
       object_name => 'CUSTOMERS',
       policy_name => 'CUST_POL');
       end;
   /
   ```

9. **Verify the Policy has been Dropped**
   Confirm that the policy no longer exists:
   ```sql
   SQL> select customer_id, cust_last_name, income_level from oe.customers
        order by customer_id
        fetch first 10 rows only;
   SQL> col policy_name format A20
   SQL> select policy_name, enable from redaction_policies;
   ```
