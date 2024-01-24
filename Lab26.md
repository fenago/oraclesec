
# Lab 26: Registering Database Vault in Oracle Database

## Objective
This lab guides you through the process of registering Database Vault with Oracle Database 12c when the database is already installed.

## Prerequisites
- Ensure you have administrative access to an Oracle 12c database.
- This lab is to be performed on a Windows environment using PowerShell.

## Steps

### 1. Connect to the Root Container
Open PowerShell and connect to the root container as a user who has privileges to create users and grant create session and set container privileges. For example, use the user `c##maja`:

```bash
sqlplus c##maja
```

### 2. Create Database Vault Users
Create two users, `c##dbv_owner` and `c##dbv_acctmgr`, and grant them necessary privileges:

```sql
create user c##dbv_owner identified by oraDVO123 CONTAINER = ALL;
grant create session, set container to c##dbv_owner CONTAINER = ALL;

create user c##dbv_acctmgr identified by oraDVA123 CONTAINER = ALL;
grant create session, set container to c##dbv_acctmgr CONTAINER = ALL;
```

### 3. Connect as SYS User
Connect to the root as a SYS user:

```sql
connect sys as sysdba
```

### 4. Configure Database Vault Users
Configure the Database Vault users with the following command:

```sql
begin
    DVSYS.CONFIGURE_DV (
        dvowner_uname => 'c##dbv_owner',
        dvacctmgr_uname => 'c##dbv_acctmgr'
    );
end;
/
```

### 5. Execute utlrp.sql Script
Execute the `utlrp.sql` script:

```sql
@?/rdbms/admin/utlrp.sql
```

### 6. Connect as Database Vault Owner
Connect to the root as the Database Vault Owner user:

```sql
connect c##dbv_owner/oraDVO123
```

### 7. Enable Oracle Database Vault
Enable Oracle Database Vault:

```sql
exec DBMS_MACADM.ENABLE_DV
```

### 8. Connect as SYS User and Restart Database
Connect as a SYS user and then restart the database:

```sql
CONNECT / AS SYSDBA
```

*Restart the database.*

### 9. Apply to Each PDB
For each Pluggable Database (PDB), repeat steps 3 through 8. Then close and reopen the pluggable database. For example, for `orclpdb`:

```sql
alter pluggable database orclpdb close immediate;
alter pluggable database orclpdb open;
```

## Conclusion
Upon completion of these steps, Oracle Database Vault should be successfully registered with your Oracle Database 12c.
