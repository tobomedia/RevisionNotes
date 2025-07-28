# PostgreSQL Cheatsheet

*A comprehensive reference for PostgreSQL's most commonly used features*

📚 **Official Documentation:** <a href="https://www.postgresql.org/docs/" target="_blank">PostgreSQL Documentation</a>

## Table of Contents

### 🚀 Getting Started
- [Installation & Setup](#installation--setup)
- [Connection & Basics](#connection--basics)
- [Database Operations](#database-operations)

### 🎯 Core SQL Operations
- [Data Types](#data-types)
- [Tables](#tables)
- [CRUD Operations](#crud-operations)
- [Queries & Filtering](#queries--filtering)

### 🔧 Advanced Features
- [Joins](#joins)
- [Indexes](#indexes)
- [Views](#views)
- [Functions & Procedures](#functions--procedures)

### 🛠️ Database Design
- [Constraints](#constraints)
- [Triggers](#triggers)
- [Transactions](#transactions)

### ⚙️ Administration
- [User Management](#user-management)
- [Backup & Restore](#backup--restore)
- [Performance](#performance)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Installation & Setup

📖 <a href="https://www.postgresql.org/docs/current/installation.html" target="_blank">Installation Documentation</a>

```bash
# Install PostgreSQL (Ubuntu/Debian)
sudo apt update
sudo apt install postgresql postgresql-contrib

# Install PostgreSQL (macOS with Homebrew)
brew install postgresql
brew services start postgresql

# Install PostgreSQL (CentOS/RHEL)
sudo yum install postgresql-server postgresql-contrib
sudo postgresql-setup initdb
sudo systemctl start postgresql

# Create user (Linux)
sudo -u postgres createuser --interactive

# Set password for postgres user
sudo -u postgres psql
ALTER USER postgres PASSWORD 'your_password';
```

## Connection & Basics

📖 <a href="https://www.postgresql.org/docs/current/app-psql.html" target="_blank">psql Documentation</a>

```bash
# Connect to PostgreSQL
psql -U username -d database_name -h host -p port

# Connect as postgres user
sudo -u postgres psql

# Connect to specific database
psql -d mydatabase

# Common psql commands
\q                      # Quit
\l                      # List databases
\c database_name        # Connect to database
\dt                     # List tables
\du                     # List users
\dp or \z               # List table permissions
\d table_name           # Describe table
\df                     # List functions
\dv                     # List views
\i filename.sql         # Execute SQL file
\o filename             # Output to file
\h COMMAND              # Help for SQL command
\?                      # Help for psql commands

# Connection string format
postgresql://username:password@host:port/database
```

## Database Operations

📖 <a href="https://www.postgresql.org/docs/current/manage-ag-createdb.html" target="_blank">Database Management Documentation</a>

```sql
-- Create database
CREATE DATABASE mydatabase;
CREATE DATABASE mydatabase OWNER myuser;

-- Drop database
DROP DATABASE mydatabase;

-- List databases
SELECT datname FROM pg_database;

-- Get current database
SELECT current_database();

-- Database size
SELECT pg_size_pretty(pg_database_size('mydatabase'));

-- Switch database (in psql)
\c mydatabase

-- Create schema
CREATE SCHEMA myschema;

-- Drop schema
DROP SCHEMA myschema CASCADE;

-- List schemas
\dn
SELECT schema_name FROM information_schema.schemata;
```

## Data Types

📖 <a href="https://www.postgresql.org/docs/current/datatype.html" target="_blank">Data Types Documentation</a>

```sql
-- Numeric Types
SMALLINT                -- 2-byte signed integer
INTEGER or INT          -- 4-byte signed integer
BIGINT                  -- 8-byte signed integer
DECIMAL(p,s)           -- Exact numeric
NUMERIC(p,s)           -- Exact numeric (same as DECIMAL)
REAL                   -- 4-byte floating point
DOUBLE PRECISION       -- 8-byte floating point
SERIAL                 -- Auto-incrementing integer
BIGSERIAL             -- Auto-incrementing bigint

-- Character Types
CHAR(n)               -- Fixed-length character string
VARCHAR(n)            -- Variable-length character string
TEXT                  -- Variable-length character string

-- Date/Time Types
DATE                  -- Date (no time)
TIME                  -- Time (no date)
TIMESTAMP             -- Date and time
TIMESTAMPTZ           -- Date and time with timezone
INTERVAL              -- Time interval

-- Boolean
BOOLEAN               -- true/false/null

-- Binary Data
BYTEA                 -- Binary data

-- JSON
JSON                  -- JSON data
JSONB                 -- Binary JSON (more efficient)

-- Arrays
INTEGER[]             -- Array of integers
TEXT[]               -- Array of text
VARCHAR(50)[]        -- Array of varchar

-- UUID
UUID                  -- Universally Unique Identifier

-- Network Address Types
INET                  -- IPv4 or IPv6 address
CIDR                  -- Network address
MACADDR              -- MAC address

-- Examples
CREATE TABLE example (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INTEGER,
    salary DECIMAL(10,2),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    tags TEXT[],
    metadata JSONB
);
```

## Tables

📖 <a href="https://www.postgresql.org/docs/current/ddl-basics.html" target="_blank">Table Basics Documentation</a>

```sql
-- Create table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create table with constraints
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    total DECIMAL(10,2) CHECK (total > 0),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Alter table
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users DROP COLUMN phone;
ALTER TABLE users ALTER COLUMN email TYPE TEXT;
ALTER TABLE users RENAME COLUMN username TO user_name;
ALTER TABLE users RENAME TO customers;

-- Drop table
DROP TABLE orders;
DROP TABLE IF EXISTS orders CASCADE;

-- Temporary table
CREATE TEMP TABLE temp_data (
    id INTEGER,
    value TEXT
);

-- Table from query
CREATE TABLE new_table AS 
SELECT * FROM existing_table WHERE condition;

-- Copy table structure
CREATE TABLE new_table (LIKE existing_table INCLUDING ALL);
```

## CRUD Operations

📖 <a href="https://www.postgresql.org/docs/current/dml.html" target="_blank">Data Manipulation Documentation</a>

```sql
-- INSERT
INSERT INTO users (username, email, password_hash) 
VALUES ('john_doe', 'john@example.com', 'hashed_password');

-- Insert multiple rows
INSERT INTO users (username, email, password_hash) VALUES 
    ('user1', 'user1@example.com', 'hash1'),
    ('user2', 'user2@example.com', 'hash2'),
    ('user3', 'user3@example.com', 'hash3');

-- Insert from another table
INSERT INTO archive_users SELECT * FROM users WHERE created_at < '2023-01-01';

-- Insert with RETURNING
INSERT INTO users (username, email, password_hash) 
VALUES ('new_user', 'new@example.com', 'hash') 
RETURNING id, created_at;

-- SELECT
SELECT * FROM users;
SELECT username, email FROM users;
SELECT COUNT(*) FROM users;
SELECT DISTINCT status FROM orders;

-- UPDATE
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;
UPDATE users SET updated_at = CURRENT_TIMESTAMP WHERE id = 1;

-- Update with RETURNING
UPDATE users SET email = 'updated@example.com' 
WHERE id = 1 
RETURNING id, email, updated_at;

-- DELETE
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE created_at < '2023-01-01';

-- Delete with RETURNING
DELETE FROM users WHERE id = 1 RETURNING *;

-- UPSERT (INSERT ... ON CONFLICT)
INSERT INTO users (id, username, email, password_hash) 
VALUES (1, 'john_doe', 'john@example.com', 'new_hash')
ON CONFLICT (id) 
DO UPDATE SET 
    email = EXCLUDED.email,
    password_hash = EXCLUDED.password_hash,
    updated_at = CURRENT_TIMESTAMP;
```

## Queries & Filtering

📖 <a href="https://www.postgresql.org/docs/current/queries.html" target="_blank">Queries Documentation</a>

```sql
-- WHERE conditions
SELECT * FROM users WHERE age > 18;
SELECT * FROM users WHERE username LIKE 'john%';
SELECT * FROM users WHERE email ILIKE '%GMAIL%'; -- Case insensitive
SELECT * FROM users WHERE created_at BETWEEN '2023-01-01' AND '2023-12-31';
SELECT * FROM users WHERE id IN (1, 2, 3, 4, 5);
SELECT * FROM users WHERE email IS NOT NULL;

-- Logical operators
SELECT * FROM users WHERE age > 18 AND status = 'active';
SELECT * FROM users WHERE age < 18 OR status = 'premium';
SELECT * FROM users WHERE NOT status = 'banned';

-- Pattern matching
SELECT * FROM users WHERE username ~ '^[a-z]+$'; -- Regex
SELECT * FROM users WHERE username !~ '[0-9]';   -- Not matching regex

-- Array operations
SELECT * FROM posts WHERE tags && ARRAY['postgresql', 'database'];
SELECT * FROM posts WHERE 'postgresql' = ANY(tags);

-- JSON operations (JSONB)
SELECT * FROM users WHERE metadata->>'city' = 'New York';
SELECT * FROM users WHERE metadata @> '{"premium": true}';
SELECT * FROM users WHERE metadata ? 'phone';

-- ORDER BY
SELECT * FROM users ORDER BY created_at DESC;
SELECT * FROM users ORDER BY age ASC, username DESC;
SELECT * FROM users ORDER BY RANDOM(); -- Random order

-- LIMIT and OFFSET
SELECT * FROM users LIMIT 10;
SELECT * FROM users LIMIT 10 OFFSET 20; -- Pagination
SELECT * FROM users ORDER BY id LIMIT 5;

-- GROUP BY and HAVING
SELECT status, COUNT(*) FROM users GROUP BY status;
SELECT status, COUNT(*) FROM users GROUP BY status HAVING COUNT(*) > 10;
SELECT DATE(created_at), COUNT(*) FROM users GROUP BY DATE(created_at);

-- Window functions
SELECT username, 
       ROW_NUMBER() OVER (ORDER BY created_at) as row_num,
       RANK() OVER (ORDER BY age DESC) as age_rank
FROM users;

-- Common Table Expressions (CTE)
WITH active_users AS (
    SELECT * FROM users WHERE status = 'active'
)
SELECT COUNT(*) FROM active_users;

-- Recursive CTE
WITH RECURSIVE employee_hierarchy AS (
    SELECT id, name, manager_id, 1 as level
    FROM employees 
    WHERE manager_id IS NULL
    
    UNION ALL
    
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    INNER JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

## Joins

📖 <a href="https://www.postgresql.org/docs/current/tutorial-join.html" target="_blank">Joins Documentation</a>

```sql
-- INNER JOIN
SELECT u.username, o.total, o.created_at
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN (LEFT OUTER JOIN)
SELECT u.username, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- RIGHT JOIN (RIGHT OUTER JOIN)
SELECT u.username, o.total
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;

-- FULL OUTER JOIN
SELECT u.username, o.total
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;

-- CROSS JOIN
SELECT u.username, p.name
FROM users u
CROSS JOIN products p;

-- Self JOIN
SELECT e1.name as employee, e2.name as manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;

-- Multiple JOINs
SELECT u.username, o.total, oi.quantity, p.name
FROM users u
INNER JOIN orders o ON u.id = o.user_id
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id;

-- JOIN with conditions
SELECT u.username, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id AND o.status = 'completed'
GROUP BY u.id, u.username;
```

## Indexes

📖 <a href="https://www.postgresql.org/docs/current/indexes.html" target="_blank">Indexes Documentation</a>

```sql
-- Create index
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);

-- Unique index
CREATE UNIQUE INDEX idx_users_email_unique ON users(email);

-- Composite index
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Partial index
CREATE INDEX idx_active_users ON users(username) WHERE status = 'active';

-- Expression index
CREATE INDEX idx_users_lower_email ON users(LOWER(email));

-- GIN index for arrays and JSONB
CREATE INDEX idx_posts_tags ON posts USING GIN(tags);
CREATE INDEX idx_users_metadata ON users USING GIN(metadata);

-- Full-text search index
CREATE INDEX idx_posts_content_fts ON posts USING GIN(to_tsvector('english', content));

-- Drop index
DROP INDEX idx_users_email;
DROP INDEX IF EXISTS idx_users_email;

-- List indexes
\di
SELECT indexname, tablename FROM pg_indexes WHERE tablename = 'users';

-- Index usage statistics
SELECT schemaname, tablename, indexname, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes;

-- Unused indexes
SELECT schemaname, tablename, indexname
FROM pg_stat_user_indexes
WHERE idx_tup_read = 0 AND idx_tup_fetch = 0;
```

## Views

📖 <a href="https://www.postgresql.org/docs/current/tutorial-views.html" target="_blank">Views Documentation</a>

```sql
-- Create view
CREATE VIEW active_users AS
SELECT id, username, email, created_at
FROM users
WHERE status = 'active';

-- Use view
SELECT * FROM active_users;

-- Create or replace view
CREATE OR REPLACE VIEW user_order_summary AS
SELECT u.id, u.username, COUNT(o.id) as order_count, SUM(o.total) as total_spent
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.username;

-- Materialized view
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT DATE_TRUNC('month', created_at) as month, SUM(total) as total_sales
FROM orders
GROUP BY DATE_TRUNC('month', created_at);

-- Refresh materialized view
REFRESH MATERIALIZED VIEW monthly_sales;

-- Drop view
DROP VIEW active_users;
DROP MATERIALIZED VIEW monthly_sales;

-- List views
\dv
SELECT viewname FROM pg_views WHERE schemaname = 'public';
```

## Functions & Procedures

📖 <a href="https://www.postgresql.org/docs/current/sql-createfunction.html" target="_blank">Functions Documentation</a>

```sql
-- Simple function
CREATE OR REPLACE FUNCTION get_user_count()
RETURNS INTEGER AS $$
BEGIN
    RETURN (SELECT COUNT(*) FROM users);
END;
$$ LANGUAGE plpgsql;

-- Function with parameters
CREATE OR REPLACE FUNCTION get_user_orders(user_id INTEGER)
RETURNS TABLE(order_id INTEGER, total DECIMAL, created_at TIMESTAMP) AS $$
BEGIN
    RETURN QUERY
    SELECT o.id, o.total, o.created_at
    FROM orders o
    WHERE o.user_id = get_user_orders.user_id;
END;
$$ LANGUAGE plpgsql;

-- Function with default parameters
CREATE OR REPLACE FUNCTION create_user(
    p_username VARCHAR(50),
    p_email VARCHAR(100),
    p_status VARCHAR(20) DEFAULT 'active'
)
RETURNS INTEGER AS $$
DECLARE
    new_user_id INTEGER;
BEGIN
    INSERT INTO users (username, email, status)
    VALUES (p_username, p_email, p_status)
    RETURNING id INTO new_user_id;
    
    RETURN new_user_id;
END;
$$ LANGUAGE plpgsql;

-- Stored procedure
CREATE OR REPLACE PROCEDURE update_user_status(
    p_user_id INTEGER,
    p_new_status VARCHAR(20)
)
LANGUAGE plpgsql AS $$
BEGIN
    UPDATE users 
    SET status = p_new_status, updated_at = CURRENT_TIMESTAMP
    WHERE id = p_user_id;
    
    IF NOT FOUND THEN
        RAISE EXCEPTION 'User with id % not found', p_user_id;
    END IF;
END;
$$;

-- Call function
SELECT get_user_count();
SELECT * FROM get_user_orders(1);

-- Call procedure
CALL update_user_status(1, 'inactive');

-- Drop function/procedure
DROP FUNCTION get_user_count();
DROP PROCEDURE update_user_status(INTEGER, VARCHAR);

-- List functions
\df
SELECT routine_name FROM information_schema.routines 
WHERE routine_type = 'FUNCTION' AND routine_schema = 'public';
```

## Constraints

📖 <a href="https://www.postgresql.org/docs/current/ddl-constraints.html" target="_blank">Constraints Documentation</a>

```sql
-- Primary key
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50)
);

-- Foreign key
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    total DECIMAL(10,2)
);

-- Unique constraint
ALTER TABLE users ADD CONSTRAINT uk_users_email UNIQUE (email);

-- Check constraint
ALTER TABLE users ADD CONSTRAINT chk_age CHECK (age >= 18);
ALTER TABLE orders ADD CONSTRAINT chk_total CHECK (total > 0);

-- Not null constraint
ALTER TABLE users ALTER COLUMN username SET NOT NULL;

-- Add constraint to existing table
ALTER TABLE users ADD CONSTRAINT fk_users_department 
FOREIGN KEY (department_id) REFERENCES departments(id);

-- Drop constraint
ALTER TABLE users DROP CONSTRAINT uk_users_email;
ALTER TABLE users DROP CONSTRAINT chk_age;

-- Exclusion constraint
CREATE TABLE reservations (
    id SERIAL PRIMARY KEY,
    room_id INTEGER,
    during TSRANGE,
    EXCLUDE USING GIST (room_id WITH =, during WITH &&)
);

-- List constraints
SELECT conname, contype FROM pg_constraint WHERE conrelid = 'users'::regclass;
```

## Triggers

📖 <a href="https://www.postgresql.org/docs/current/sql-createtrigger.html" target="_blank">Triggers Documentation</a>

```sql
-- Create trigger function
CREATE OR REPLACE FUNCTION update_modified_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Create trigger
CREATE TRIGGER trigger_users_updated_at
    BEFORE UPDATE ON users
    FOR EACH ROW
    EXECUTE FUNCTION update_modified_column();

-- Audit trigger function
CREATE OR REPLACE FUNCTION audit_table_changes()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'DELETE' THEN
        INSERT INTO audit_log (table_name, operation, old_values, timestamp)
        VALUES (TG_TABLE_NAME, TG_OP, row_to_json(OLD), CURRENT_TIMESTAMP);
        RETURN OLD;
    ELSIF TG_OP = 'UPDATE' THEN
        INSERT INTO audit_log (table_name, operation, old_values, new_values, timestamp)
        VALUES (TG_TABLE_NAME, TG_OP, row_to_json(OLD), row_to_json(NEW), CURRENT_TIMESTAMP);
        RETURN NEW;
    ELSIF TG_OP = 'INSERT' THEN
        INSERT INTO audit_log (table_name, operation, new_values, timestamp)
        VALUES (TG_TABLE_NAME, TG_OP, row_to_json(NEW), CURRENT_TIMESTAMP);
        RETURN NEW;
    END IF;
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Create audit trigger
CREATE TRIGGER audit_users_changes
    AFTER INSERT OR UPDATE OR DELETE ON users
    FOR EACH ROW
    EXECUTE FUNCTION audit_table_changes();

-- Drop trigger
DROP TRIGGER trigger_users_updated_at ON users;

-- List triggers
\dt+ users
SELECT trigger_name FROM information_schema.triggers WHERE event_object_table = 'users';
```

## Transactions

📖 <a href="https://www.postgresql.org/docs/current/tutorial-transactions.html" target="_blank">Transactions Documentation</a>

```sql
-- Basic transaction
BEGIN;
    INSERT INTO users (username, email) VALUES ('test_user', 'test@example.com');
    UPDATE users SET status = 'active' WHERE username = 'test_user';
COMMIT;

-- Transaction with rollback
BEGIN;
    DELETE FROM orders WHERE total < 10;
    -- Something went wrong
ROLLBACK;

-- Savepoints
BEGIN;
    INSERT INTO users (username, email) VALUES ('user1', 'user1@example.com');
    SAVEPOINT sp1;
    
    INSERT INTO users (username, email) VALUES ('user2', 'user2@example.com');
    SAVEPOINT sp2;
    
    DELETE FROM users WHERE username = 'user1';
    -- Rollback to savepoint
    ROLLBACK TO sp1;
    
    -- user1 insert is preserved, user2 insert and delete are rolled back
COMMIT;

-- Transaction isolation levels
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Check current transaction status
SELECT txid_current();
SELECT txid_current_if_assigned();

-- Lock tables
BEGIN;
LOCK TABLE users IN ACCESS EXCLUSIVE MODE;
-- Perform operations
COMMIT;
```

## User Management

📖 <a href="https://www.postgresql.org/docs/current/user-manag.html" target="_blank">User Management Documentation</a>

```sql
-- Create user/role
CREATE USER myuser WITH PASSWORD 'mypassword';
CREATE ROLE myrole;

-- Create user with options
CREATE USER developer WITH 
    PASSWORD 'dev_password'
    CREATEDB
    VALID UNTIL '2025-12-31';

-- Alter user
ALTER USER myuser WITH PASSWORD 'newpassword';
ALTER USER myuser CREATEDB;
ALTER USER myuser VALID UNTIL '2025-12-31';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
GRANT SELECT, INSERT, UPDATE ON users TO myuser;
GRANT ALL ON ALL TABLES IN SCHEMA public TO myuser;
GRANT USAGE ON SCHEMA public TO myuser;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO myuser;

-- Revoke privileges
REVOKE ALL PRIVILEGES ON DATABASE mydatabase FROM myuser;
REVOKE INSERT ON users FROM myuser;

-- Role membership
GRANT developer TO myuser;
REVOKE developer FROM myuser;

-- List users and roles
\du
SELECT usename FROM pg_user;
SELECT rolname FROM pg_roles;

-- Check permissions
\dp table_name
SELECT grantee, privilege_type 
FROM information_schema.role_table_grants 
WHERE table_name = 'users';

-- Drop user
DROP USER myuser;
DROP ROLE myrole;

-- Current user info
SELECT current_user;
SELECT session_user;
SELECT current_role;
```

## Backup & Restore

📖 <a href="https://www.postgresql.org/docs/current/backup.html" target="_blank">Backup Documentation</a>

```bash
# Database dump
pg_dump mydatabase > mydatabase.sql
pg_dump -U username -h host mydatabase > mydatabase.sql

# Dump with custom format (recommended)
pg_dump -Fc mydatabase > mydatabase.dump

# Dump only schema
pg_dump --schema-only mydatabase > schema.sql

# Dump only data
pg_dump --data-only mydatabase > data.sql

# Dump specific tables
pg_dump --table=users --table=orders mydatabase > tables.sql

# Dump all databases
pg_dumpall > all_databases.sql

# Restore from SQL dump
psql mydatabase < mydatabase.sql

# Restore from custom format
pg_restore -d mydatabase mydatabase.dump

# Restore with options
pg_restore -d mydatabase --clean --if-exists mydatabase.dump

# Restore specific table
pg_restore -d mydatabase --table=users mydatabase.dump

# Copy table data
COPY users TO '/path/to/users.csv' CSV HEADER;
COPY users FROM '/path/to/users.csv' CSV HEADER;

# Copy with specific columns
COPY users (id, username, email) TO '/path/to/users.csv' CSV HEADER;
```

## Performance

📖 <a href="https://www.postgresql.org/docs/current/performance-tips.html" target="_blank">Performance Tips Documentation</a>

```sql
-- Explain query plan
EXPLAIN SELECT * FROM users WHERE email = 'john@example.com';
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'john@example.com';
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM users WHERE email = 'john@example.com';

-- Query statistics
SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;

-- Table statistics
SELECT schemaname, tablename, n_tup_ins, n_tup_upd, n_tup_del
FROM pg_stat_user_tables;

-- Index usage
SELECT schemaname, tablename, indexname, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_tup_read DESC;

-- Vacuum and analyze
VACUUM users;
VACUUM ANALYZE users;
VACUUM FULL users; -- Reclaims space but requires exclusive lock

-- Auto vacuum settings
SHOW autovacuum;
SELECT name, setting FROM pg_settings WHERE name LIKE 'autovacuum%';

-- Connection statistics
SELECT datname, numbackends, xact_commit, xact_rollback
FROM pg_stat_database;

-- Long running queries
SELECT pid, now() - pg_stat_activity.query_start AS duration, query
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';

-- Kill query
SELECT pg_cancel_backend(pid);
SELECT pg_terminate_backend(pid);

-- Database size
SELECT pg_size_pretty(pg_database_size('mydatabase'));

-- Table sizes
SELECT schemaname, tablename, 
       pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size
FROM pg_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

---

## Quick Reference Links

### Essential Resources
- 📚 <a href="https://www.postgresql.org/docs/" target="_blank">PostgreSQL Documentation</a> - Complete documentation
- 🎮 <a href="https://pgexercises.com/" target="_blank">PostgreSQL Exercises</a> - Interactive SQL practice
- 📝 <a href="https://www.postgresql.org/docs/current/sql-commands.html" target="_blank">SQL Commands Reference</a> - All PostgreSQL commands
- 🔧 <a href="https://wiki.postgresql.org/wiki/Main_Page" target="_blank">PostgreSQL Wiki</a> - Community resources

### Learning Resources
- 🎓 <a href="https://www.postgresql.org/docs/current/tutorial.html" target="_blank">PostgreSQL Tutorial</a> - Official tutorial
- 📖 <a href="https://postgresqlco.nf/doc/en/param/" target="_blank">PostgreSQL Configuration</a> - Parameter reference
- 🧪 <a href="https://explain.depesz.com/" target="_blank">Depesz Explain</a> - Query plan analyzer

### Tools & Administration
- 🔧 <a href="https://www.pgadmin.org/" target="_blank">pgAdmin</a> - Web-based administration tool
- 🎨 <a href="https://dbeaver.io/" target="_blank">DBeaver</a> - Universal database tool
- 💻 <a href="https://www.postgresql.org/docs/current/app-psql.html" target="_blank">psql Reference</a> - Command-line client

### Performance & Optimization
- 📊 <a href="https://pgtune.leopard.in.ua/" target="_blank">PGTune</a> - Configuration tuning
- 🔍 <a href="https://explain.dalibo.com/" target="_blank">Dalibo Explain</a> - Query plan visualization
- 🚀 <a href="https://www.postgresql.org/docs/current/monitoring-stats.html" target="_blank">Monitoring Stats</a> - Performance monitoring

### Extensions & Ecosystem
- 🧩 <a href="https://www.postgresql.org/docs/current/contrib.html" target="_blank">Additional Supplied Modules</a> - Official extensions
- 🔌 <a href="https://pgxn.org/" target="_blank">PGXN</a> - PostgreSQL Extension Network
- 🌐 <a href="https://postgis.net/" target="_blank">PostGIS</a> - Geospatial extension

---

*This cheatsheet covers PostgreSQL 15+ features. For the latest updates, always refer to the <a href="https://www.postgresql.org/docs/" target="_blank">official documentation</a>.*