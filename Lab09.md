
## Oracle 19c Security Lab: Managing Privileges

### The syskm Privilege – How, When, and Why Should You Use It?
#### Database Authentication

The instructions for database authentication are as follows:

1. **Connect to the database as sysdba (or another user that can grant the syskm privilege):**
   ```powershell
   sqlplus / as sysdba
   ALTER SESSION SET CONTAINER = orclpdb;
   ```

2. **Grant the syskm privilege to user Jessica:**
   ```sql
   SQL> grant syskm to jessica;
   ```

3. **Connect user Jessica to the database as syskm:**
   ```sql
   SQL> connect jessica/oracle_1 as syskm
   ```

4. **View privileges:**
   ```sql
   SQL> select * from user_tab_privs;
   SQL> select * from session_privs;
   ```

## The sysdg Privilege – How, When, and Why Should You Use It?
### Database Authentication
The instructions for database authentication are as follows:

1. **Connect to the database as sysdba (or another user who can grant the sysdg privilege):**
   ```powershell
   sqlplus / as sysdba
   ```

2. **Grant SYSDG privilege to user Steve:**
   ```sql
   SQL> grant sysdg to steve;
   ```

3. **Exit SQL*Plus, connect Steve using the dgmgrl command-line interface:**
   ```powershell
   SQL> exit
   $ dgmgrl
   DGMRRL> connect steve/test_1
   ```
