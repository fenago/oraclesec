
# Lab Exercise: User Account Management in Oracle 19c

## Objective
Learn how to lock and unlock user accounts, expire user passwords, and create proxy users in Oracle 19c using SQLPlus.

## Prerequisites
- Oracle 19c Database installed in a Windows environment.
- Basic knowledge of SQL commands and Oracle database administration.
- Make sure you go into orclpdb

## Part 1: Locking and Unlocking User Accounts

### 1. Lock a User Account
- **Command**: `alter user [username] account lock;`
- **Explanation**: This command locks the specified user account, preventing the user from logging in. Replace `[username]` with the actual username.
- **Example**: `alter user jessica account lock;`

### 2. Unlock a User Account
- **Command**: `alter user [username] account unlock;`
- **Explanation**: This command unlocks the specified user account, allowing the user to log in again.
- **Example**: `alter user jessica account unlock;`

## Part 2: Expiring a User's Password

### 1. Expire a User's Password
- **Command**: `alter user [username] password expire;`
- **Explanation**: This command expires the password for the specified user. The user will be prompted to change their password at the next login.
- **Example**: `alter user tom password expire;`

## Part 3: Creating a Proxy User

### 1. Create a Proxy User
- **Step 1**: Create a new user if the proxy user does not exist.
    - **Command**: `create user proxyuser identified by password;`
    - **Explanation**: This creates a new user `proxyuser` with the specified password.

- **Step 2**: Grant connect privilege to the proxy user.
    - **Command**: `grant connect to proxyuser;`
    - **Explanation**: The `connect` privilege is necessary for the proxy user to log in.

- **Step 3**: Grant the proxy user the ability to connect as another user.
    - **Command**: `alter user [target_username] grant connect through proxyuser;`
    - **Explanation**: This command allows `proxyuser` to connect as `[target_username]`.
    - **Example**: `alter user jessica grant connect through proxyuser;`

### 2. Connect using Proxy User
- **Command**: `connect proxyuser[jessica]@orclpdb;`
- **Explanation**: This command connects to the database `orclpdb` as `jessica` using `proxyuser`. The user `jessica` is the target user in this case.

## Conclusion
This lab provides a practical understanding of how to manage user accounts in Oracle 19c, including locking and unlocking accounts, expiring passwords, and creating proxy users.

---

Note: Replace placeholders like `[username]` and `password` with actual values as per your database environment. Adjust the lab content based on the specific requirements of your course.
