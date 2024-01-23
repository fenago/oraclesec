
# Oracle 19c Security Lab: Multitenant Environment

## Security Considerations in Multitenant Environment

### Creating a Common User
1. **Connect to the root container as a common user who has create user privilege granted commonly (for example, c##ernesto or system user):**
   ```sql
   sqlplus / as sysdba
   SELECT name FROM v$database;
   SQL> connect c##ernesto@cdb1
   ```

2. **Create a common user (for example, c##maja):**
   ```sql
   c##ernesto@CDB1> create user c##maja identified by oracle1 container=all;
   ```

### Creating a Local User
1. **Connect to PDB (for example, pdb1) as a common user or local user who has create user privilege in that PDB (for example, c##ernesto or system user):**
   ```sql
   SQL> connect c##ernesto@localhost:1521/orclpdb
   ```

2. **Create a local user (for example, steve):**
   ```sql
   c##ernesto@PDB1> create user steve identified by pa3t5brii
   container=current;
   ```

### Creating a Common Role
1. **Connect to the root container as a common user who has create role privilege granted commonly (for example, c##ernesto or system user):**
   ```sql
   SQL> connect c##ernesto@localhost:1521/cdb1
   ```

2. **Create a common role (for example, c##role1):**
   ```sql
   SQL> create role c##role1 container=all;
   ```

### Creating a Local Role
1. **Connect to PDB (for example, pdb1) as a common or local user who has create role privilege in that PDB (for example, c##maja):**
   ```sql
   SQL> connect c##maja@pdb1
   ```

2. **Create a local role (for example, local_role1):**
   ```sql
   c##maja@PDB1> create role local_role1 container=current;
   ```
