
# Lab Exercise: Creating and Assigning a Password Profile in Oracle 19c

## Objective
Create a custom password profile with specific restrictions and assign it to a user in Oracle 19c.

## Prerequisites
- Oracle 19c Database installed in a Windows environment.
- Administrative privileges to create and manage database profiles.

## Instructions

### Step 0: Connect to the Database and Set the Environment
- **Command**:
    ```sql
    sqlplus / as sysdba
    ```
- **Explanation**: 
    Open PowerShell, execute the command above to connect to the Oracle Database as `SYSDBA` using OS Authentication.
- **Next**:
    ```sql
    ALTER SESSION SET CONTAINER = orclpdb;
    ```
- **Explanation**: 
    This command sets the current session to use the pluggable database `orclpdb`, where you will create and assign the password profile.

### Step 1: Create a Custom Password Profile
- **Command**: 
    ```sql
    CREATE PROFILE userprofile LIMIT 
    FAILED_LOGIN_ATTEMPTS 4 
    PASSWORD_LOCK_TIME 2 
    PASSWORD_LIFE_TIME 180;
    ```
- **Explanation**: 
    This SQL command creates a new password profile named `userprofile` with the following restrictions:
    - `FAILED_LOGIN_ATTEMPTS 4`: The account will be locked after 4 consecutive failed login attempts.
    - `PASSWORD_LOCK_TIME 2`: Once locked, the account will remain locked for 2 days.
    - `PASSWORD_LIFE_TIME 180`: The password must be changed every 180 days. If the password is not changed within this time, it will expire and the user will be prompted to change it at the next login.

### Step 2: Assign the Custom Profile to a User
- **Command**: 
    ```sql
    ALTER USER [username] PROFILE userprofile;
    ```
- **Example**: 
    ```sql
    ALTER USER jessica PROFILE userprofile;
    ```
- **Explanation**: 
    Replace `[username]` with the actual username to whom you want to assign the `userprofile`. This assigns the newly created password profile to the user, enforcing the defined password policies for that account.

### Step 3: Alter the Default Password Profile
- **Command**: 
    ```sql
    ALTER PROFILE DEFAULT LIMIT FAILED_LOGIN_ATTEMPTS 4;
    ```
- **Explanation**: 
    This command modifies the default password profile to set the `FAILED_LOGIN_ATTEMPTS` restriction to 4. This means that any user assigned to the default profile will have their account locked after 4 consecutive failed login attempts.

## Conclusion
This lab demonstrates how to create a custom password profile with specific password policies and assign it to a user. Additionally, it shows how to alter the default password profile to change the failed login attempt limit.

Remember to replace `[username]` with the actual username when executing these commands in your own Oracle environment.
