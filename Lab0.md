
# Lab Exercise: Managing Oracle 19c Users and Passwords

## Objective
To provide practical experience in managing Oracle 19c database users and passwords using PowerShell on Windows and SQLPlus.

## Prerequisites
- Windows environment with Oracle 19c Database installed in default locations.
- Basic understanding of PowerShell, SQL commands, and Oracle database administration.

## Part 1: Basic Database Operations using SQLPlus

### 1. Open PowerShell
- **Step**: Navigate to the Start menu, type `PowerShell`, and open the PowerShell application.
- **Explanation**: PowerShell is a command-line shell used for task automation and configuration management.

### 2. Connect to Oracle Database as SYSDBA
- **Command**: `sqlplus / as sysdba`
- **Explanation**: This command uses Operating System (OS) authentication to connect to the Oracle database with `SYSDBA` privileges, which are required for high-level administrative tasks.

### 3. Open the Pluggable Database (PDB)
- **Command**: `alter pluggable database orclpdb open;`
- **Explanation**: This SQL command opens the pluggable database named `orclpdb`, allowing it to be accessed by users and applications.

### 4. Save the State of the Pluggable Database
- **Command**: `alter pluggable database orclpdb save state;`
- **Explanation**: This command ensures that the pluggable database `orclpdb` will automatically open whenever the Oracle instance is started.

### 5. Exit SQLPlus
- **Command**: `exit;`
- **Explanation**: Exits SQLPlus and returns to the PowerShell prompt.

## Part 2: User Management in Oracle Database

### 1. Use OS Authentication to Connect to the Database
- **Command**: `sqlplus / as sysdba`
- **Explanation**: Again, connect to the database using OS authentication with `SYSDBA` privileges.
- Then switch your session to the pluggable database (PDB) named orclpdb
`ALTER SESSION SET CONTAINER = orclpdb;`

### 2. Create User Jessica
- **Command**: `create user jessica identified by fenago;`
- **Explanation**: This command creates a new user named 'jessica' with a password 'fenago'. The password is simpler, adhering to the default password profile settings.

### 3. Create User Tom with a Complex Password
- **Command**: `create user tom identified by "Fen@g0Comp!ex";`
- **Explanation**: User 'tom' is created with a more complex password. The complexity can vary depending on the organization's password policy.

### 4. Create User with a Specific Password Profile
- **Command**: `create user userprofile identified by fenago;`
- **Explanation**: This creates a user with the name 'userprofile' and a simple password, adhering to the default password profile.

### 5. Create User John with Password Change Requirement
- **Command**: `create user john identified by fenago password expire;`
- **Explanation**: User 'john' must change his password upon the first login, as indicated by the `password expire` clause.

### 6. Create User Richard with Unlimited Quota
- **Command**: `create user richard identified by fenago quota unlimited on users;`
- **Explanation**: This allows 'richard' to use unlimited space in the 'users' tablespace. The 'users' tablespace must already exist.

### 7. Privileged User Changing Jessica's Password
- **Step**: Log in as a user with the `ALTER USER` privilege.
- **Command**: `alter user jessica identified by newpassword;`
- Do the same for tom.  change his to fenago
- **Explanation**: This command changes Jessica's password. Replace `newpassword` with the desired new password.

### 8. User Tom Changing His Own Password
- **Step**: Log in as 'tom'.
- `sqlplus tom/fenago@localhost:1521/orclpdb`
- **Command**: `alter user tom identified by newpassword;`
- **Explanation**: Tom can change his own password using this command. Again, replace `newpassword` with the new password.
- This will fail until you add grant permissions to tom.
- `sqlplus / as sysdba`
- `GRANT CREATE SESSION to tom;`
- `sqlplus tom/fenago@localhost:1521/orclpdb`

## Conclusion
This lab guides students through the process of managing users and passwords in Oracle 19c using PowerShell and SQLPlus. The exercises cover various scenarios, including creating users with different password complexities, assigning quotas, and changing passwords.

---

The default password 'fenago' is used throughout for consistency, but feel free to change it as needed for your lab environment.
