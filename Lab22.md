
# Lab22: Creating a Redaction Policy Using Regular Expression Redaction in Oracle 19c

This lab demonstrates how to create a redaction policy using regular expression redaction in Oracle 19c. Perform these steps using PowerShell on Windows.

## Steps

1. **Connect to the Database as a User with SELECT Privilege**
   Open PowerShell and enter:
   ```sql
   $ sqlplus sh
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```

2. **Verify Data Visibility**
   Execute the following query to view data in clear text format:
   ```sql
   select cust_id, cust_main_phone_number from sh.customers
   order by cust_id
   fetch first 10 rows only;
   ```

3. **Connect as the secmgr User**
   ```sql
   SQL> connect secmgr/oracle
   ```

4. **Create the Redaction Policy SHORT_POL**
   This policy applies regular expression redaction to the cust_main_phone_number column:
   ```sql
   SQL> begin
       dbms_redact.add_policy(
       object_schema => 'SH',
       object_name => 'CUSTOMERS',
       policy_name => 'SHORT_POL',
       column_name => 'CUST_MAIN_PHONE_NUMBER',
       function_type => DBMS_REDACT.REGEXP,
       expression => '1=1',
       regexp_pattern => DBMS_REDACT.RE_PATTERN_US_PHONE,
       regexp_replace_string => DBMS_REDACT.RE_REDACT_US_PHONE_L7,
       regexp_position => DBMS_REDACT.RE_BEGINNING,
       regexp_occurrence => DBMS_REDACT.RE_FIRST);
       end;
   /
   ```

5. **Test the Redaction Policy**
   Connect to the database as the same user as in step 1 (e.g., sh) and execute the same query as in step 2 to observe the redaction policy in action.

   ```sql
   SQL> connect sh
   SQL> select cust_id, cust_main_phone_number from sh.customers
        order by cust_id
        fetch first 10 rows only;
   ```

   After applying the redaction policy, the cust_main_phone_number column data should appear redacted based on the regular expression.
