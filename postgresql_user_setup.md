# Read-Only User
Step 1: Create the User
```SQL
CREATE USER prod_readonly WITH PASSWORD 'secure_password';
```

Step 2: Connect to the Target Database
```PSQL
\c database_name
```

Step 3: Grant Usage and Connect Permissions
Allows user to login and see the schema
```SQL
GRANT CONNECT ON DATABASE your_database_name TO prod_readonly;
GRANT USAGE ON SCHEMA public TO prod_readonly;
```

Step 4: Grant Select On Existing Tables
```SQL
GRANT SELECT ON ALL TABLES IN SCHEMA public TO prod_readonly;
```

Step 5: Configure Default Privileges
By default, the user won't have access to tables created in the future. The default privileges must be altered to fix this
```SQL
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
GRANT SELECT ON TABLES TO prod_readonly;
```

# Read-Write (App) User
Step 1: Create the User
```SQL
CREATE USER prod_app_user WITH PASSWORD 'secure_password';
```


Step 2: Connect to the Target Database
```PSQL
\c database_name
```

Step 3: Grant Usage and Connect Permissions
```SQL
GRANT CONNECT ON DATABASE your_database_name TO prod_app_user;
GRANT USAGE ON SCHEMA public TO prod_app_user;
```

Step 4: Grant Table and Sequence Permissions
```SQL
-- Allow CRUD operations on tables
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO prod_app_user;

-- Allow usage of sequences (required for auto-incrementing IDs)
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO prod_app_user;
```

Step 5: Configure Default Privileges
```SQL
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO prod_app_user;

ALTER DEFAULT PRIVILEGES IN SCHEMA public 
GRANT USAGE, SELECT ON SEQUENCES TO prod_app_user;
```

# Database Admin User
This user has full control over a specific database (can DROP tables, ALTER schema) but cannot affect other databases or server settings.
```SQL
CREATE USER prod_db_admin WITH PASSWORD 'secure_password';

-- Make this user the owner of the database
ALTER DATABASE your_database_name OWNER TO prod_db_admin;

-- Grant all privileges on the schema
GRANT ALL PRIVILEGES ON SCHEMA public TO prod_db_admin;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO prod_db_admin;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO prod_db_admin;
```

# Cluster Admin User

> [!WARNING]
> This user bypasses all permission checks. Use only for maintenance or creating new databases.

```SQL
CREATE USER sys_admin WITH PASSWORD 'secure_password';

-- Grant superuser status
ALTER USER sys_admin WITH SUPERUSER;
```