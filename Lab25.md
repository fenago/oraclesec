
# Lab25: Exempting Users from Data Redaction Policies in Oracle 19c

This lab demonstrates how to exempt specific users from data redaction policies in Oracle 19c. Use PowerShell on Windows to perform these tasks.

## Steps

1. **Connect to the Database as a User with DBA Role**
   Open PowerShell and enter:
   ```sql
   $ sqlplus ernesto/oracle
   ```

   **Next**:

   ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
   ```

2. **Create a New User and Grant Privileges**
   Create a user (e.g., vipuser) and grant the necessary privileges:
   ```sql
   SQL> create user vipuser identified by oracle;
   SQL> grant create session to vipuser;
   SQL> grant select on ernesto.customers to vipuser;
   ```

3. **Connect as the New User and Select from Table**
   Attempt to select data from the `ernesto.customers` table as `vipuser`:
   ```sql
   SQL> connect vipuser/oracle
   SQL> select * from ernesto.customers;
   ```

   The data should be redacted for this user.

4. **Grant EXEMPT REDACTION POLICY Privilege**
   As `ernesto`, grant the EXEMPT REDACTION POLICY privilege to `vipuser`:
   ```sql
   SQL> connect ernesto/oracle
   SQL> grant exempt redaction policy to vipuser;
   ```

5. **Verify Access to Unredacted Data**
   As `vipuser`, now try to select from the `ernesto.customers` table:
   ```sql
   SQL> connect vipuser/oracle
   SQL> select * from ernesto.customers;
   ```

   The `vipuser` should now be able to see the unredacted data.
