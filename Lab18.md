
# Lab18: Oracle 19c Security - Restricting Access to Program Units Using ACCESSIBLE BY

## Objective
Learn how to restrict access to Oracle program units using the `ACCESSIBLE BY` clause.

### Step 1: Connect as a User with Procedure Creation Privilege
Connect to the database as a user who can create procedures (e.g., `ernesto`).

```sql
SQL> connect ernesto
```

### Step 2: Create Protected Package Accessible Only by a Specific Package
Create the `protected_pkg` package that is only accessible by `public_pkg`.

```sql
CREATE OR REPLACE PACKAGE protected_pkg   
   ACCESSIBLE BY (public_pkg)
IS
   PROCEDURE protected_proc;
END;
/

CREATE OR REPLACE PACKAGE BODY protected_pkg
IS
   PROCEDURE protected_proc
   IS
   BEGIN
         DBMS_OUTPUT.PUT_LINE ('This is a Protected Procedure that can only be accessed from Public Package');
   END;
END;
/
```

### Step 3: Create Public Package
Create the `public_pkg` package.

```sql
CREATE OR REPLACE PACKAGE public_pkg
IS
      PROCEDURE public_proc;
END;
/

CREATE OR REPLACE PACKAGE BODY public_pkg
IS
      PROCEDURE public_proc
      IS
      BEGIN
         DBMS_OUTPUT.PUT_LINE ('This is Public Procedure from Public Package!');
         protected_pkg.protected_proc;
      END;
END;
/
```

### Step 4: Execute Public Procedure from Public Package
Execute the `public_proc` procedure from `public_pkg`.

```sql
SQL> set serveroutput on
SQL> EXEC public_pkg.public_proc;
-- Expect "This is Public Procedure from Public Package! This is a Protected Procedure that can only be accessed from Public Package"
```

### Step 5: Attempt Direct Execution of Protected Procedure
Try to directly execute `protected_proc` from `protected_pkg` and observe the error.

```sql
SQL> EXEC protected_pkg.protected_proc;
-- Expect an ORA-06550 and PLS-00904 error due to insufficient privileges
```

### Step 6: Attempt to Create Package that Accesses Protected Procedure
Try to create another package `other_pkg` that attempts to access `protected_proc` from `protected_pkg`.

```sql
CREATE OR REPLACE PACKAGE other_pkg
IS
   PROCEDURE other_proc;
END;
/

CREATE OR REPLACE PACKAGE BODY other_pkg
IS
   PROCEDURE other_proc
   IS
   BEGIN
        DBMS_OUTPUT.PUT_LINE ('This is Other Procedure from Other Package!');
         protected_pkg.protected_proc;
   END;
END;
/
-- Expect compilation errors
```

### Step 7: Check Compilation Errors
Find the compilation errors for `other_pkg`.

```sql
SQL> show errors
-- Expect errors related to insufficient privilege to access `PROTECTED_PKG`
```
