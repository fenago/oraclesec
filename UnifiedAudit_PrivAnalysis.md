
# Unified Auditing in Oracle 19c

This guide provides a step-by-step approach to setting up a SQL*Plus environment in PowerShell, switching to the pluggable database (if necessary), and creating two users, Jack and Jill, with specific auditing roles in an Oracle 19c database.

## Setting up SQL*Plus Environment in PowerShell

To run SQL*Plus commands from PowerShell, you need to first set up the environment. This involves setting the Oracle environment variables and then launching SQL*Plus.

```powershell
# Set Oracle environment variables
$env:ORACLE_HOME = "C:\path\to\your\oracle\installation"
$env:PATH += ";$env:ORACLE_HOME\bin"

# Launch SQL*Plus
sqlplus / as sysdba
```

Replace `C:\path\to\your\oracle\installation` with the actual path to your Oracle installation.

## Switching to the Pluggable Database (if required)

If you need to work with a pluggable database (PDB), you can switch to it using the following SQL*Plus command:

```sql
-- Connect to the root container
CONNECT / AS SYSDBA

-- Switch to the PDB named orclpdb
ALTER SESSION SET CONTAINER = orclpdb;
```

## Creating Users Jack and Jill

As a user with the DBA role, execute the following commands in SQL*Plus to create users Jack and Jill.

### Creating User Jack

Jack will be a Unified Auditing admin.

```sql
-- Create user Jack with the auditing admin privileges
CREATE USER jack IDENTIFIED BY password;
GRANT CREATE SESSION TO jack;
GRANT UNIFIED_AUDIT_SYSTEM TO jack;
```

Replace `password` with a strong password of your choosing.

### Creating User Jill

Jill will be an auditor without admin capabilities.

```sql
-- Create user Jill with the necessary privileges to view audit data
CREATE USER jill IDENTIFIED BY password;
GRANT CREATE SESSION TO jill;
GRANT SELECT ON SYS.UNIFIED_AUDIT_TRAIL TO jill;
```

Again, replace `password` with a strong password of your choosing.

## Conclusion

You now have a SQL*Plus environment set up in PowerShell and have created two users with specific roles related to Unified Auditing in Oracle 19c.


# Part 2: Enabling Unified Auditing and Creating an Audit Policy

This section of the lab covers enabling Unified Auditing in mixed mode, creating an audit policy using one of the sample schemas, testing the policy, and documenting the behavior.

## Exiting SQL*Plus

To start, make sure you have exited the current SQL*Plus session.

```sql
-- Exit SQL*Plus
EXIT;
```

## Logging Back In to SQL*Plus

Open PowerShell and log in to SQL*Plus as a user with DBA privileges.

```powershell
sqlplus your_dba_username@orclpdb
```

Replace `your_dba_username` with your DBA user name.

## Enabling Unified Audit in Mixed Mode

Since Mixed Mode is enabled by default in Oracle 19c, there is no need to perform additional steps to enable it. However, let's verify that it's enabled.

```sql
-- Verify if Mixed Mode is enabled
SELECT VALUE FROM V$OPTION WHERE PARAMETER = 'Unified Auditing';
```

You should see `TRUE` as the value, indicating that Unified Auditing in Mixed Mode is enabled.

## Creating and Enabling an Audit Policy

Now, we will create an audit policy to audit SELECT operations on the `EMPLOYEES` table in the `HR` schema.

```sql
-- Create the audit policy
CREATE AUDIT POLICY hr_select_policy ACTIONS SELECT ON HR.EMPLOYEES;

-- Enable the audit policy
AUDIT POLICY hr_select_policy;
```

## Testing the Audit Policy

To test the audit policy, perform a SELECT operation on the `EMPLOYEES` table as a user with access to the `HR` schema.

```sql
-- Test the audit policy
SELECT * FROM HR.EMPLOYEES;
```

After executing the SELECT statement, you can view the audit trail to confirm that the operation was audited.

```sql
-- View the audit trail
SELECT * FROM UNIFIED_AUDIT_TRAIL WHERE ACTION_NAME = 'SELECT' AND OBJECT_SCHEMA = 'HR' AND OBJECT_NAME = 'EMPLOYEES';
```

Look for a new entry in the audit trail corresponding to the SELECT operation you just performed.

## Monitoring and Documenting Behavior

It's important to regularly monitor the audit trail and document any findings or unusual behavior. This can help in compliance and security investigations.

## Thinking Up Other Policies

Consider what other operations or database schemas might require auditing. Think about creating policies for UPDATE or DELETE operations, or for sensitive tables in other schemas.

For assistance with creating audit policies, refer to the Oracle documentation on Unified Auditing:

[Oracle Unified Auditing Documentation](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/introduction-to-auditing.html#GUID-ADA1F6DF-6C64-4BD6-B1B6-8B02BEF0A6FA)


# Privilege Analysis Lab Using Oracle 19c

This lab guides you through the process of performing privilege analysis in an Oracle 19c database using the sample users Jack and Jill. Privilege analysis helps you identify which privileges are being used and which are not, allowing you to enforce the principle of least privilege.

## Prerequisites

Before you start with privilege analysis, ensure that the following prerequisites are met:

- Unified Auditing is enabled (as covered in the previous lab).
- Users Jack and Jill have been created.
- Jack is granted the `CAPTURE_ADMIN` role to manage privilege captures.
- Jill has been granted access to view the audit data.

## Granting CAPTURE_ADMIN Role to Jack

Log in to SQL*Plus as a user with DBA privileges and grant Jack the `CAPTURE_ADMIN` role.

```sql
-- Grant CAPTURE_ADMIN role to Jack
GRANT CAPTURE_ADMIN TO jack;
```

## Starting Privilege Analysis

As user Jack, you will create a privilege analysis policy to determine which privileges are used by the user Jill when accessing the `HR.EMPLOYEES` table.

```sql
-- Connect as Jack
CONNECT jack@orclpdb

-- Create a privilege analysis policy
BEGIN
  DBMS_PRIVILEGE_CAPTURE.CREATE_CAPTURE (
    name           => 'emp_table_priv_analysis',
    description    => 'Privilege analysis for HR.EMPLOYEES table',
    type           => DBMS_PRIVILEGE_CAPTURE.G_DATABASE,
    condition      => 'USER=''JILL'' AND OBJECT_SCHEMA=''HR'' AND OBJECT_NAME=''EMPLOYEES'''
  );
END;
/
```

## Enabling the Privilege Analysis Policy

Enable the privilege analysis policy to start capturing privilege usage data.

```sql
-- Enable the privilege analysis policy
EXEC DBMS_PRIVILEGE_CAPTURE.ENABLE_CAPTURE ('emp_table_priv_analysis');
```

## Performing Operations as Jill

Log in as Jill and perform some operations on the `HR.EMPLOYEES` table.

```sql
-- Connect as Jill
CONNECT jill@orclpdb

-- Perform operations
SELECT * FROM HR.EMPLOYEES;
UPDATE HR.EMPLOYEES SET EMAIL = EMAIL WHERE EMPLOYEE_ID = 100;
```

## Generating Privilege Analysis Results

Switch back to user Jack to generate the analysis report.

```sql
-- Connect as Jack
CONNECT jack@orclpdb

-- Generate the results
EXEC DBMS_PRIVILEGE_CAPTURE.GENERATE_RESULT ('emp_table_priv_analysis');
```

## Viewing the Privilege Analysis Results

Query the DBA_USED_PRIVS view to see which privileges Jill has used.

```sql
-- View the used privileges
SELECT * FROM DBA_USED_PRIVS WHERE CAPTURE_NAME = 'emp_table_priv_analysis';
```

## Identifying Unused Privileges

Query the DBA_UNUSED_PRIVS view to identify any privileges that were not used by Jill.

```sql
-- View the unused privileges
SELECT * FROM DBA_UNUSED_PRIVS WHERE CAPTURE_NAME = 'emp_table_priv_analysis';
```

## Conclusion

This analysis helps you understand which privileges are necessary for Jill's activities. Based on this data, you can revoke unnecessary privileges to adhere to the principle of least privilege.

## Lab Exercise

Ask the students to repeat the privilege analysis for other schemas and tables. They should identify unused privileges and write a report on their findings.

For more information on privilege analysis, refer to the Oracle documentation:

[Oracle Database Privilege Analysis](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/database-privilege-analysis.html)

