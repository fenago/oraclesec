
# Oracle 19c Security Lab: Granting Privileges and Roles

## Granting Privileges and Roles Commonly
1. **Connect to the root container as a common user who can grant these privileges and roles (for example, c##maja or system user):**
   ```sql
   SQL> connect c##maja@cdb1
   ```

2. **Grant a privilege (for example, create session) to a common user (for example, c##john) commonly:**
   ```sql
   c##maja@CDB1> grant create session to c##john container=all;
   ```

3. **Grant a privilege (for example, select any table) to a common role (for example, c##role1) commonly:**
   ```sql
   c##maja@CDB1> grant select any table to c##role1 container=all;
   ```

4. **Grant a common role (for example, c##role1) to another common role (for example, c##role2) commonly:**
   ```sql
   c##maja@CDB1> grant c##role1 to c##role2 container=all;
   ```

5. **Grant a common role (for example, c##role2) to a common user (for example, c##john) commonly:**
   ```sql
   c##maja@CDB1> grant c##role2 to c##john container=all;
   ```

## Granting Privileges and Roles Locally
1. **Connect to the container (root or pluggable database) in which you want to grant the privilege as a common or local user who can grant that privilege (for example, c##maja):**
   ```sql
   SQL> connect c##maja@pdb1
   ```

2. **Grant a privilege (for example, create synonym) to a common user (for example, c##john) locally:**
   ```sql
   c##maja@PDB1> grant create synonym to c##john container=current;
   ```

3. **Grant a privilege (for example, create view) to a local user (for example, steve) locally:**
   ```sql
   c##maja@PDB1> grant create view to steve container=current;
   ```

4. **Grant a privilege (for example, create table) to a common role (for example, c##role1) locally:**
   ```sql
   c##maja@PDB1> grant create table to c##role1 container=current;
   ```

5. **Grant a privilege (for example, create procedure) to a local role (for example, local_role1) locally:**
   ```sql
   c##maja@PDB1> grant create procedure to local_role1 container=current;
   ```

6. **Grant a common role (for example, c##role2) to another common role (for example, c##role3) locally:**
   ```sql
   c##maja@PDB1> grant c##role2 to c##role3 container=current;
   ```

7. **Grant a common role (for example, c##role3) to a local role (for example, local_role1) locally:**
   ```sql
   c##maja@PDB1> grant c##role3 to local_role1 container=current;
   ```

8. **Grant a local role (for example, local_role1) to a common role (for example, c##role4) locally:**
   ```sql
   c##maja@PDB1> grant local_role1 to c##role4 container=current;
   ```

9. **Grant a common role (for example, c##role4) to a common user (for example, c##john) locally:**
   ```sql
   c##maja@PDB1> grant c##role4 to c##john container=current;
   ```




### Effects of Plugging/Unplugging Operations on Users, Roles, and Privileges

This lab guides you through the process of plugging and unplugging operations in Oracle 19c and observing their effects on users, roles, and privileges. Ensure you have Oracle 19c installed and accessible via PowerShell on Windows.

### Steps

1. **Connect to the Root Container of cdb1 as User SYS**  
   Open PowerShell and connect to your Oracle database:
   ```sql
   SQL> connect sys@cdb1 as sysdba
   ```

2. **Unplug pdb1 by Creating an XML Metadata File**  
   Execute the following command to unplug the PDB and create an XML metadata file:
   ```sql
   SQL> alter pluggable database pdb1 unplug into '/u02/oradata/pdb1.xml';
   ```

3. **Drop pdb1 and Keep the Datafiles**  
   Now, drop the pluggable database but keep the data files:
   ```sql
   SQL> drop pluggable database pdb1 keep datafiles;
   ```

4. **Connect to the Root Container of cdb2 as User SYS**  
   Connect to a different container (cdb2) as the SYS user:
   ```sql
   SQL> connect sys@cdb2 as sysdba
   ```

5. **Create (Plug) pdb1 to cdb2 Using the Previously Created Metadata File**  
   Finally, create (or plug) pdb1 into cdb2 using the metadata file:
   ```sql
   SQL> create pluggable database pdb1 using '/u02/oradata/pdb1.xml' nocopy;
   ```

## Notes

- Replace `cdb1`, `cdb2`, and `pdb1` with your actual container and pluggable database names.
- Ensure the file paths used are correct and accessible on your system.
- Follow your organization's best practices for database operations and security.

