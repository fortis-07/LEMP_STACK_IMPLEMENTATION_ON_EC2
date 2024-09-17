# Basic SQL Commands

In the course of this project i documented some od the most important MySQL commands that helps to have a good understanding and navigation of the Database environment in creating databases

- Create a Database

Purpose: Create a new database.

```bash
CREATE DATABASE database_name;
```

- Drop a Database

Purpose: Delete an existing database and all its data.

```bash
DROP DATABASE database_name;
```

- Create a User

Purpose: Create a new user account in the database management system.

```bash
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
- Grant Privileges to a User
  
Purpose: Assign specific permissions to a user.

```bash
GRANT privilege_type ON database_name.table_name TO 'username'@'host';
```
- Revoke Privileges from a User
  
Purpose: Remove specific permissions from a user.

```bash
REVOKE privilege_type ON database_name.table_name FROM 'username'@'host';
```
- Drop a User

Purpose: Delete an existing user account.

```bash
DROP USER 'username'@'host';
```

- Flush Privileges
  
Purpose: Reload the grant tables to ensure that any changes to user privileges take effect.

```bash
FLUSH PRIVILEGES;
```

- Show Databases
  
Purpose: List all databases.

```bash
SHOW DATABASES;
```

- Show Tables
  
Purpose: List all tables in the current database.

```bash
SHOW TABLES;
```

**These commands are fundamental for managing and maintaining databases and users within a SQL-based database management system.**
