
# Lab: Install Sample Schemas in Oracle 19c

## Objective
Learn how to install sample schemas in Oracle 19c by downloading and setting up the necessary files.

### Step 1: Download Sample Schemas
Go to the GitHub repository to download the required files.

- Visit [https://github.com/fenago/oracleBI/](https://github.com/fenago/oracleBI/)
- Download `sample_schemas.zip` and `sales_history.zip`

### Step 2: Expand the Zip Files
Extract the contents of `sample_schemas.zip` and `sales_history.zip`.

- Extract `sample_schemas.zip` to a folder named `sample_schemas`.
- Extract `sales_history.zip` to a folder named `sales_history`.

### Step 3: Organize the Files
Copy the `sales_history` folder into the `sample_schemas` folder.

- Ensure all folders are in `C:\User\Administrator\Downloads\sample_schemas\`.

### Step 4: Create Oracle Schema Folders
Create the necessary folders in the Oracle directory.

- Make the directory `C:\Oracle\Middleware\Oracle_Home\demo\schema\`.

### Step 5: Copy the Schemas
Copy the contents from `C:\Users\Administrator\downloads\sample_schemas\` into `C:\Oracle\Middleware\Oracle_Home\demo\schema\`.

### Step 6: Connect to the Database as SYSDBA
Open PowerShell and connect to the database as SYSDBA.
MAKE SURE YOU CD to :
```
cd c:\Oracle\Middleware\Oracle_Home\demo\schema\
```
then
```sql
sqlplus / as sysdba
```

### Step 7: Connect to the Database with SYSTEM User
Connect to the database using the SYSTEM user.

```sql
CONNECT system/fenago
@mksample
```

### Additional Information
- Username: fenago
- Password: fenago
- Sample Schemas: hr, oe, pm, ix, sh, bi
- Users: users
- Temporary Tablespace: temp
- Schema Directory: `C:\Oracle\Middleware\Oracle_Home\demo\schema\logs\`
- Database Connection: localhost:1521/orclpdb

Follow these steps to successfully install and set up sample schemas in your Oracle 19c environment.
