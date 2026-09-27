
## 1. What is PostgreSQL?

### First Principles: From Storage to Relational Databases

To understand PostgreSQL, we must build up from the core problem of software engineering: **data persistence**.

- **Database**: An organized, persistent collection of structured data stored electronically on a computer system. Unlike in-memory data structures (like a Python `list` or `dict`), a database survives application crashes, server restarts, and power outages.
    
- **DBMS (Database Management System)**: The software layer that interacts with end-users, applications, and the database itself to capture, analyze, and retrieve data. It enforces security, data integrity, and concurrent access.
    
- **RDBMS (Relational Database Management System)**: A DBMS based on the relational model introduced by E. F. Codd. Data is stored in formal tables composed of rows (tuples) and columns (attributes), and relationships between tables are maintained through shared values (keys).
    
- **PostgreSQL**: A free, open-source, enterprise-grade, object-relational database management system (ORDBMS) known for robust data integrity, extensive feature sets, and strict adherence to ANSI SQL standards.
    

### PostgreSQL Hierarchy

A single running instance of PostgreSQL is organized hierarchically:

```
PostgreSQL Server (Instance)
└── Databases (e.g., production_db, staging_db)
    └── Schemas (e.g., public, analytics, auth)
        ├── Tables (e.g., users, courses)
        ├── Views
        └── Functions / Triggers
```

1. **PostgreSQL Server (Instance)**: A set of background processes and allocated memory managing physical data files on disk. A server host can run multiple instances, usually bound to different network ports (default: `5432`).
    
2. **Database**: An isolated logical container inside a server instance. Queries in PostgreSQL generally operate within a single database; cross-database joins are restricted by default.
    
3. **Schema**: A namespace within a database. It contains database objects such as tables, views, indexes, and data types.
    
4. **Table**: The foundational relational structure composed of defined columns with explicit data types and zero or more rows of records.
    

### Why PostgreSQL for Backend Engineering?

Modern backend applications require extreme reliability, performance under concurrency, and complex querying capabilities. PostgreSQL is the industry standard for production systems because:

- **ACID Compliance**: Guarantees valid transactions even during power failures or server crashes.
    
- **Advanced Data Types**: Native support for JSON/JSONB, UUIDs, arrays, spatial data (PostGIS), and custom ENUMs.
    
- **High Concurrency**: Uses Multi-Version Concurrency Control (MVCC) to allow concurrent reads and writes without locking the entire database.
    
- **Extensibility**: Custom functions, procedural languages (PL/pgSQL), and custom extensions (e.g., `pgvector` for AI vector embeddings).
    

### PostgreSQL in the Backend Architecture

In a standard web stack, PostgreSQL sits behind your application layer, acting as the durable source of truth.

Фрагмент кода

```
graph TD
    Client[Client / Browser / Mobile] -->|HTTP Request / JSON| FastAPI[FastAPI App]
    FastAPI -->|Validation & Parsing| BusinessLogic[Business Logic Layer]
    BusinessLogic -->|Raw SQL Query| PostgreSQL[PostgreSQL Database Server]
    PostgreSQL -->|I/O Operations| Disk[(Persistent Storage / Disk)]
    Disk -->|Data Blocks| PostgreSQL
    PostgreSQL -->|Result Sets / Tuples| BusinessLogic
    BusinessLogic -->|Pydantic Model| FastAPI
    FastAPI -->|HTTP Response / JSON| Client
```

#### Component Breakdown

- **Client**: Initiates state changes or requests data via standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`).
    
- **FastAPI**: Operates as the HTTP server, parsing requests, managing routing, and serializing outputs.
    
- **Business Logic**: Enforces application rules (e.g., calculating discounts, validating user permissions) before interacting with storage.
    
- **PostgreSQL**: Handles query compilation, execution, transaction management, constraint enforcement, and index searches.
    
- **Disk**: The underlying block storage (NVMe, SSD, persistent cloud volume) where data files, Write-Ahead Logs (WAL), and system catalogs are physically stored.
    

## 2. PostgreSQL vs SQL

Understanding the distinction between SQL and PostgreSQL is fundamental:

- **SQL (Structured Query Language)**: An ANSI/ISO standard declarative domain-specific language designed for managing data in an RDBMS. SQL defines _what_ data to retrieve or manipulate, not _how_ the engine physical retrieves it.
    
- **PostgreSQL**: An actual engine and complete software implementation that parses, plans, executes, and persists SQL commands.
    

### The Analogy

Think of **SQL** as the **C++ Language Specification** and **PostgreSQL** as the **GCC or Clang compiler**. Alternately, SQL is the driving manual (rules of the road), while PostgreSQL, MySQL, and SQLite are distinct automobiles built by different manufacturers that all follow those core rules, each offering different engine capacities, interiors, and features.

### Feature Matrix

|**Feature / Property**|**Standard SQL**|**PostgreSQL**|**MySQL**|**SQLite**|
|---|---|---|---|---|
|**Type**|Specification|Client-Server RDBMS|Client-Server RDBMS|Embedded File-based Engine|
|**Primary Focus**|Standard syntax|Data integrity, standards, extensibility|Speed, web application ubiquity|Zero config, local/mobile apps|
|**Concurrency Model**|N/A|MVCC (Row-level)|MVCC (InnoDB)|File-locking (Single writer)|
|**JSON Support**|Basic syntax specs|High performance native binary JSON (`JSONB`)|Native `JSON` type|Text-based JSON functions|
|**Extension System**|None|Robust (`pg_trgm`, `PostGIS`, `vector`)|Limited|Loadable extensions|
|**Custom Types**|Standard types only|FULL support (ENUMs, Composites, Domains)|Limited|Type affinity only (Flexible)|

## 3. PostgreSQL Architecture — High Level

When an application connects to PostgreSQL, it interacts with a sophisticated multi-process engine.

```
+-----------------------------------------------------------------------+
|                       POSTGRESQL SERVER INSTANCE                      |
|                                                                       |
|  +--------------------+                                               |
|  |   Postmaster /     | <--- Listens on TCP Port 5432                 |
|  |   Main Process     |                                               |
|  +--------------------+                                               |
|            |                                                          |
|            | Spawns dedicated process per connection                  |
|            v                                                          |
|  +--------------------+    +--------------------+                     |
|  |  Backend Process 1 |    |  Backend Process 2 |  ...                 |
|  +--------------------+    +--------------------+                     |
|            |                                                          |
|            +------------------+                                       |
|                               v                                       |
|  +-----------------------------------------------------------------+  |
|  |                      SHARED MEMORY (Buffer Pool)                |  |
|  +-----------------------------------------------------------------+  |
+-----------------------------------------------------------------------+
```

- **Postmaster (Main Process)**: The central supervisor process. It initializes memory, listens on TCP port `5432`, and forks a dedicated backend process for every new client connection.
    
- **Client Connection & Session**: A client (like a FastAPI app instance or `psql`) opens a TCP connection. A dedicated **backend process** is assigned to handle that connection's entire **session**.
    
- **Query Execution Engine**: Composed of:
    
    - _Parser_: Validates SQL syntax and converts the raw string into a Query Tree.
        
    - _Analyzer/Rewriter_: Checks object existence (do tables/columns exist?) and applies views or security policies.
        
    - _Optimizer/Planner_: Generates execution strategies (e.g., Index Scan vs. Sequential Scan) and calculates cost metrics.
        
    - _Executor_: Executes the chosen plan, reading/writing blocks via the shared memory buffer.
        

### Execution Flow: `SELECT * FROM users;`

Фрагмент кода

```
sequenceDiagram
    autonumber
    participant App as FastAPI App
    participant Conn as Connection / Session
    participant Parser as Query Parser & Analyzer
    participant Planner as Query Planner
    participant Exec as Query Executor
    participant Buffer as Shared Buffer Pool
    participant Disk as Physical Storage

    App->>Conn: Transmit string "SELECT * FROM users;"
    Conn->>Parser: Send raw SQL
    Parser->>Parser: Parse syntax & build Query Tree
    Parser->>Planner: Pass analyzed Query Tree
    Planner->>Planner: Calculate costs & generate plan (Seq Scan)
    Planner->>Exec: Output plan execution tree
    Exec->>Buffer: Request pages containing 'users' table
    alt Page in Buffer
        Buffer-->>Exec: Return tuples directly from RAM
    else Page missing from Buffer
        Buffer->>Disk: Read block(s) from data file
        Disk-->>Buffer: Load data page into RAM
        Buffer-->>Exec: Return requested tuples
    end
    Exec-->>Conn: Stream result set (rows)
    Conn-->>App: Deliver serialized SQL result set
```

1. **FastAPI Application** sends the query string `SELECT * FROM users;` over an established TCP connection socket.
    
2. The dedicated PostgreSQL **Backend Process** receives the packet.
    
3. The **Parser & Analyzer** verifies that the syntax is valid SQL and confirms that the table `users` exists in the active schema.
    
4. The **Planner** evaluates paths to retrieve the data. Since there is no `WHERE` clause, it selects a **Sequential Scan** (`Seq Scan`).
    
5. The **Executor** requests table blocks from the **Shared Buffer Pool** (RAM allocated to PostgreSQL).
    
6. If blocks are not currently in RAM, the buffer manager reads the necessary 8KB pages from the physical **Disk**.
    
7. The Executor formats the raw binary records into SQL result rows and streams them back across the network to FastAPI.
    

## 4. Installing PostgreSQL

### Components of the Installation Package

- **PostgreSQL Server**: The core service running background tasks, managing memory buffers, and writing data to disk.
    
- **pgAdmin**: A graphical user interface (GUI) web application designed to manage, query, and monitor PostgreSQL instances.
    
- **psql**: The native, lightweight command-line interface (CLI) client for interactive administration and SQL execution.
    
- **PostgreSQL Service**: The operating system background daemon (Windows Service) configured to launch automatically on system startup.
    
- **Postgres Superuser (`postgres`)**: The default root administrative account possessing full privileges over all server objects.
    
- **Port 5432**: The default TCP port exposed by PostgreSQL for network traffic.
    

### Installation Steps (Windows)

1. Download the interactive installer from the official PostgreSQL site (EnterpriseDB builds).
    
2. Run the executable and accept default installation paths.
    
3. Select all components: _PostgreSQL Server_, _pgAdmin 4_, _Command Line Tools_, and _Stack Builder_.
    
4. Set a strong administrative password for the `postgres` superuser account. **Store this securely.**
    
5. Keep the default port set to `5432`.
    
6. Select the locale (Default Locale recommended). Complete the wizard.
    

### Verifying Service Operation

Open Windows PowerShell or Command Prompt and run:

PowerShell

```
Get-Service -Name postgresql*
```

If the status shows `Running`, your service is operational.

You can also check if the port is bound and listening:

PowerShell

```
netstat -ano | findstr 5432
```

> [!warning] Common Installation Troubleshooting
> 
> - **Port 5432 in Use**: If another service or existing installation uses `5432`, choose `5433` during installation or stop the competing service.
>     
> - **Password Authentication Failures**: Ensure you record the password chosen during setup. If lost, you must alter `pg_hba.conf` locally from `scram-sha-256` to `trust` mode temporarily to reset it.
>     
> - **Path Variable Missing**: If `psql` is not recognized in terminal, add `C:\Program Files\PostgreSQL\<version>\bin` to your Windows System `PATH` Environment Variables.
>     

## 5. Connecting to PostgreSQL

### Method 1: Interactive CLI (`psql`)

To establish a session via terminal, run the following command:

Bash

```
psql -U postgres -h localhost -p 5432 -d postgres
```

#### Syntax Breakdown

- `psql`: Launches the terminal client program.
    
- `-U postgres`: Specifies the user identity (`postgres` superuser).
    
- `-h localhost`: Defines the destination host IP or domain name (local machine).
    
- `-p 5432`: Declares the target TCP port where PostgreSQL listens.
    
- `-d postgres`: Defines the initial target database to connect to on login.
    

Upon entering your password, you will see the interactive terminal prompt:

Plaintext

```
postgres=#
```

> [!tip] Useful `psql` Meta-Commands
> 
> - `\l` : List all databases on the server.
>     
> - `\c database_name` : Connect to (switch focus to) a different database.
>     
> - `\dt` : List all tables in the current schema.
>     
> - `\d table_name` : Describe the structure of a specific table (columns, types, constraints).
>     
> - `\dn` : List all schemas in the active database.
>     
> - `\q` : Quit/Exit the `psql` terminal.
>     

### Method 2: Graphical Client (`pgAdmin`)

1. Launch **pgAdmin 4** from your desktop applications menu.
    
2. Set up your master password for the pgAdmin client application itself.
    
3. Right-click **Servers** in the left browser tree $\rightarrow$ **Register** $\rightarrow$ **Server...**
    
4. Under the **General** tab:
    
    - `Name`: `Localhost Dev`
        
5. Under the **Connection** tab:
    
    - `Host name/address`: `localhost`
        
    - `Port`: `5432`
        
    - `Maintenance database`: `postgres`
        
    - `Username`: `postgres`
        
    - `Password`: `<Your Password Superuser System>`
        
6. Click **Save**. You are now connected via a graphical client.
    

### Server Connection vs. Database Connection

- **Server Connection**: The underlying TCP socket connection established between your application/client software and the PostgreSQL master process.
    
- **Database Connection**: Once authenticated on the server, your session must be attached explicitly to **one** specific database in the server instance context. SQL commands execute exclusively against the context of that single target database.
    

## 6. PostgreSQL Databases

A database in PostgreSQL acts as a logical domain separation boundary.

### Managing Databases via SQL

#### Creating a Database

SQL

```
CREATE DATABASE university;
```

- `CREATE DATABASE`: Instructs the engine to provision a new isolated database environment.
    
- `university`: The unique name identifier assigned to the target database.
    

#### Listing Databases

In SQL standard query format:

SQL

```
SELECT datname FROM pg_database WHERE datistemplate = false;
```

- `SELECT datname`: Selects the name column from the system catalog table `pg_database`.
    
- `WHERE datistemplate = false`: Filters out system templates (`template0`, `template1`).
    

#### Connecting to a Database

In `psql` terminal environment:

Plaintext

```
\c university
```

- `\c`: Executes a client-level database context switch to `university`.
    

#### Dropping a Database

SQL

```
DROP DATABASE university;
```

- `DROP DATABASE`: Immediately removes the target database from the disk along with **all** contained schemas, tables, and data files.
    

> [!warning] Destructive Command Risks
> 
> Executing `DROP DATABASE` is **irreversible**. There is no "recycle bin." Unless a physical backup or point-in-time WAL recovery pipeline exists, dropped databases cannot be restored. Production deployment service accounts must never be granted `DROP` level privileges.

## 7. Schemas

### Understanding the Dual Meaning of "Schema"

1. **Conceptual/Relational Schema (Database Architecture)**: The structural design of your entire database layout (e.g., "The user management schema has 4 tables with FK links").
    
2. **PostgreSQL Schema (Namespace Feature)**: A explicit organizational folder/namespace _inside_ a single database context.
    

### Namespace Structural Hierarchy

Plaintext

```
PostgreSQL Server Instance
│
└── Database: university
    │
    ├── Schema: public (Default)
    │   ├── Table: students
    │   ├── Table: courses
    │   └── Table: enrollments
    │
    ├── Schema: audit
    │   └── Table: access_logs
    │
    └── Schema: billing
        └── Table: invoices
```

### The `public` Schema

By default, every newly generated PostgreSQL database contains an automatically generated schema named `public`. Unless specified otherwise, any `CREATE TABLE` command puts the object inside the `public` schema.

SQL

```
-- Explicitly creating a new namespace schema
CREATE SCHEMA accounting;

-- Creating a table inside a non-default schema namespace
CREATE TABLE accounting.invoices (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    amount NUMERIC(10, 2) NOT NULL
);
```

## 8. Creating Tables (DDL Fundamentals)

Data Definition Language (DDL) manages the structural definitions of your database system.

### Table Creation Example

SQL

```
CREATE TABLE courses (
    id INTEGER PRIMARY KEY,
    code VARCHAR(20) NOT NULL,
    title VARCHAR(100) NOT NULL,
    credits INTEGER NOT NULL
);
```

#### Line-by-Line Code Explanation

- `CREATE`: DDL command word instructing PostgreSQL to instantiate a new structural object.
    
- `TABLE`: Specifies that the target object type to build is a relational data table.
    
- `courses`: The unique identifier name assigned to this table object inside the active schema.
    
- `(`: Opens the column definition block parameter list.
    
- `id INTEGER PRIMARY KEY,`:
    
    - `id`: Column identifier name.
        
    - `INTEGER`: Data type specifying a 4-byte signed integer value range.
        
    - `PRIMARY KEY`: Implicitly adds `UNIQUE` and `NOT NULL` constraints and marks this column as the official unique row identifier.
        
- `code VARCHAR(20) NOT NULL,`:
    
    - `code`: Column identifier.
        
    - `VARCHAR(20)`: Variable character string with a maximum limit of 20 characters.
        
    - `NOT NULL`: Constraint dictating that this column cannot hold empty/NULL values.
        
- `title VARCHAR(100) NOT NULL,`: Column that stores text string titles up to 100 characters long; cannot be NULL.
    
- `credits INTEGER NOT NULL`: Column storing integer credit hours; cannot be NULL.
    
- `);`: Closes the column definition block and finishes the DDL statement.
    

### Internal Actions Executed by PostgreSQL

When you run this statement, PostgreSQL:

1. Validates user permission to create tables in the current schema.
    
2. Creates a new entry in the `pg_class` system catalog table.
    
3. Registers column data types, positioning, and nullability rules in `pg_attribute`.
    
4. Generates a unique primary key B-Tree index on disk (`courses_pkey`).
    
5. Allocates an underlying storage file (relfilenode) on the host file system.
    

## 9. PostgreSQL Data Types

Choosing proper data types ensures **data integrity**, **optimal memory utilization**, and **query speed**.

### Data Type Categories Overview

|**Category**|**PostgreSQL Type**|**Storage Size**|**Allowed Range / Characteristics**|**Best Use Case**|
|---|---|---|---|---|
|**Numeric**|`INTEGER`|4 Bytes|$-2,147,483,648$ to $+2,147,483,647$|Standard IDs, counts, quantities|
||`BIGINT`|8 Bytes|$-9 \times 10^{18}$ to $+9 \times 10^{18}$|High-volume primary keys|
||`NUMERIC(p,s)`|Variable|Exact fractional accuracy up to 1000 digits|Financial values, money|
||`DOUBLE PRECISION`|8 Bytes|IEEE 754 Floating point (15 decimal digits precision)|Scientific math, GPS coordinates|
|**Text**|`VARCHAR(n)`|Variable|Max length explicitly bounded to $n$ characters|Short fixed limits (e.g., ZIP codes)|
||`TEXT`|Variable|Unlimited length variable string|Free-form notes, descriptions|
||`CHAR(n)`|Fixed|Always padded with spaces to length $n$|Legacy compatibility (Rarely used)|
|**Boolean**|`BOOLEAN`|1 Byte|`TRUE`, `FALSE`, or `NULL`|State flags (`is_active`, `is_paid`)|
|**Temporal**|`DATE`|4 Bytes|Calendar dates (Year, Month, Day)|Birthdays, event dates|
||`TIMESTAMP`|8 Bytes|Date + Time without timezone information|Local context times (Avoid for APIs)|
||`TIMESTAMPTZ`|8 Bytes|Date + Time stored in UTC timezone format|System logs, application timestamps|
|**Specialized**|`UUID`|16 Bytes|128-bit globally unique identifiers|Distributed system keys, API IDs|
||`JSONB`|Variable|Binary-parsed JSON structure with indexing|Flexible dynamic payload storage|

> [!important] Crucial Rule: Financial Calculations and Floating Point Issues
> 
> **Never** use `REAL` or `DOUBLE PRECISION` to store monetary amounts (e.g., product prices, bank balances). Floating-point arithmetic follows IEEE 754 standards, which can introduce imprecise binary rounding errors:
> 
> $$0.1 + 0.2 = 0.30000000000000004$$
> 
> Always use `NUMERIC(precision, scale)` or `DECIMAL(precision, scale)` for money.
> 
> - `precision`: Total count of digits across the entire number (both sides of the decimal).
>     
> - `scale`: Count of digits allowed to the right of the decimal point.
>     
> - _Example_: `NUMERIC(10, 2)` permits numbers up to `99,999,999.99`.
>     

## 10. Primary Keys

### Purpose of Primary Keys

A **Primary Key** is a column (or combination of columns) that uniquely identifies every distinct row in a table. A table can have **only one** primary key.

- **Uniqueness**: No two rows can contain matching primary key values.
    
- **Non-Nullity**: Primary key columns are automatically enforced as `NOT NULL`.
    

### Surrogate Keys vs. Natural Keys

- **Natural Key**: A column with a real-world unique attribute that naturally exists in the business context (e.g., Social Security Number, Vehicle Identification Number).
    
    - _Drawback_: Business definitions change. Even natural identifiers occasionally duplicate or change formats, leading to difficult cascade updates across foreign keys.
        
- **Surrogate Key**: An artificial primary key assigned by the database engine (e.g., an auto-incrementing integer or a randomly generated UUID).
    
    - _Advantage_: Highly performant, invariant, completely decoupled from business domain shifts.
        

### Modern Primary Key Generation: SQL Standard Identity Columns

Historically, PostgreSQL utilized non-standard `SERIAL` types backed by implicit sequence generators. Modern PostgreSQL implementations use standard **Identity Columns**.

SQL

```
CREATE TABLE departments (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);
```

#### Line-by-Line Code Explanation

- `id BIGINT`: Defines the surrogate key using an 8-byte integer to avoid running out of IDs.
    
- `GENERATED ALWAYS AS IDENTITY`: Directs PostgreSQL to manage an underlying internal sequence counter automatically. The system raises an explicit error if a client manually attempts to pass an explicit ID value into an `INSERT` statement without explicitly overriding protection settings.
    
- `PRIMARY KEY`: Enforces `NOT NULL` and `UNIQUE` constraints and generates the primary B-tree search index on `id`.
    

> [!note] `GENERATED ALWAYS` vs. `GENERATED BY DEFAULT`
> 
> - `GENERATED ALWAYS`: Prevents manual ID inserts unless `OVERRIDING SYSTEM VALUE` is added to the query. Best for preventing human error.
>     
> - `GENERATED BY DEFAULT`: Allows explicit manual insertions while generating auto-incremented values if an ID is omitted. Useful for data migration scripts.
>     

## 11. Constraints

Constraints enforce business and relational integrity rules directly within the database layer.

Фрагмент кода

```
graph TD
    Data[Data Operations / INSERT / UPDATE] --> Constraints{PostgreSQL Constraint Check}
    Constraints -->|Passes All Checks| Store[Persist Data To Disk]
    Constraints -->|Fails Any Check| Reject[Abort Transaction & Raise Error]
```

> [!important] Database Constraints vs. Application Logic
> 
> Never rely solely on application code (e.g., FastAPI/Pydantic validation) for data integrity. Concurrent requests can bypass application-level validation due to race conditions. Constraints act as an unbreakable safeguard at the database level.

### Detailed Constraint Types and Examples

SQL

```
CREATE TABLE users (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    username VARCHAR(30) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL,
    age INTEGER CHECK (age >= 18),
    account_status VARCHAR(20) DEFAULT 'pending',
    CONSTRAINT unq_email_address UNIQUE (email)
);
```

#### Detailed Breakdown of Constraints

1. **`PRIMARY KEY`**: Combines `UNIQUE` and `NOT NULL` constraints into a single column identifier.
    
2. **`NOT NULL`**: Guarantees that the target column cannot accept `NULL` values during an `INSERT` or `UPDATE`.
    
3. **`UNIQUE`**: Guarantees that all non-null values in the target column are distinct across all rows.
    
4. **`CHECK`**: Evaluates a boolean expression before committing a row. If the expression yields `FALSE`, the insert/update transaction aborts with a constraint violation error.
    
5. **`DEFAULT`**: Automatically provides a pre-configured value if the client omits that column during an `INSERT`.
    

## 12. Foreign Keys

A **Foreign Key** is a column (or group of columns) in one table that references the primary key of another table, establishing a parent-child relationship.

### Referencing Infrastructure Example

SQL

```
-- Parent Table
CREATE TABLE instructors (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- Child Table
CREATE TABLE courses (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    instructor_id BIGINT REFERENCES instructors(id) ON DELETE SET NULL
);
```

Фрагмент кода

```
erDiagram
    instructors ||--o{ courses : "teaches"
    instructors {
        BIGINT id PK
        VARCHAR name
    }
    courses {
        BIGINT id PK
        VARCHAR title
        BIGINT instructor_id FK
    }
```

#### Line-by-Line Code Explanation

- `instructor_id BIGINT`: Defines the child table foreign key column. _Note_: The data type must match the primary key type of the target parent table (`BIGINT`).
    
- `REFERENCES instructors(id)`: Declares that values entered into `instructor_id` must already exist within the `id` column of the `instructors` table.
    
- `ON DELETE SET NULL`: Defines the **Referential Integrity Referential Action**. If a parent instructor row is deleted from the database, PostgreSQL automatically updates all corresponding `instructor_id` fields in child `courses` records to `NULL` to prevent orphan references.
    

> [!tip] Foreign Key Referential Actions
> 
> - `ON DELETE RESTRICT` (Default): Rejects deletion of a parent row if child records reference it.
>     
> - `ON DELETE CASCADE`: Deleting a parent row automatically deletes all linked child rows.
>     
> - `ON DELETE SET NULL`: Sets the foreign key column in child rows to `NULL` when the parent row is deleted.
>     

## 13. One-to-Many Relationships

A **One-to-Many (1:N)** relationship occurs when a single record in a parent table relates to multiple child records in another table, but each child record links back to only one parent.

### Example Domain

- **1** Instructor can teach **Many** Courses.
    
- Each Course is taught by **1** Instructor.
    

Plaintext

```
[ instructors Table ]             [ courses Table ]
+----+----------------+           +----+-------------------+---------------+
| id | name           |           | id | title             | instructor_id |
+----+----------------+           +----+-------------------+---------------+
| 1  | Dr. Aris Thorne| <-------+--| 101| Database Systems  | 1             |
| 2  | Prof. Ellen Ripley|       +--| 102| Operating Systems | 1             |
+----+----------------+           | 103| Machine Learning  | 2             |
                                  +----+-------------------+---------------+
```

### Table Implementation and Insert Sequence

SQL

```
-- 1. Create Parent Table
CREATE TABLE instructors (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- 2. Create Child Table containing the Foreign Key
CREATE TABLE courses (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    code VARCHAR(20) NOT NULL UNIQUE,
    title VARCHAR(100) NOT NULL,
    instructor_id BIGINT REFERENCES instructors(id) ON DELETE RESTRICT
);

-- 3. Populate Parent Data
INSERT INTO instructors (name) VALUES ('Dr. Aris Thorne');

-- 4. Populate Child Data referencing Parent Key = 1
INSERT INTO courses (code, title, instructor_id) 
VALUES ('CS101', 'Database Systems', 1),
       ('CS102', 'Operating Systems', 1);
```

## 14. Many-to-Many Relationships

A **Many-to-Many (M:N)** relationship occurs when a single record in Table A relates to multiple records in Table B, and vice versa.

### Resolving M:N with Junction Tables

Relational engines cannot directly connect two tables in a many-to-many model. Instead, we convert an M:N relationship into two 1:N relationships using a **Junction Table** (also known as a Join Table or Associative Table).

Фрагмент кода

```
erDiagram
    students ||--o{ enrollments : "places"
    courses ||--o{ enrollments : "contains"
    
    students {
        BIGINT id PK
        VARCHAR full_name
    }
    enrollments {
        BIGINT student_id PK,FK
        BIGINT course_id PK,FK
        TIMESTAMPTZ enrolled_at
    }
    courses {
        BIGINT id PK
        VARCHAR title
    }
```

### Junction Table Implementation

SQL

```
CREATE TABLE students (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL
);

CREATE TABLE courses (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    title VARCHAR(100) NOT NULL
);

-- Junction Table
CREATE TABLE enrollments (
    student_id BIGINT REFERENCES students(id) ON DELETE CASCADE,
    course_id BIGINT REFERENCES courses(id) ON DELETE CASCADE,
    enrolled_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    
    -- Composite Primary Key
    PRIMARY KEY (student_id, course_id)
);
```

#### Line-by-Line Code Explanation

- `student_id BIGINT REFERENCES students(id)...`: Foreign key referencing the `students` table.
    
- `course_id BIGINT REFERENCES courses(id)...`: Foreign key referencing the `courses` table.
    
- `PRIMARY KEY (student_id, course_id)`: Creates a **Composite Primary Key**. This ensures that the combination of `student_id` and `course_id` is unique, preventing a student from enrolling in the same course twice.
    

## 15. CRUD with PostgreSQL

Data Manipulation Language (DML) manages data retrieval and updates within tables.

### 1. INSERT (Create)

SQL

```
INSERT INTO courses (code, title, credits)
VALUES ('CS101', 'Database Fundamentals', 4)
RETURNING id, code;
```

- `INSERT INTO courses`: Identifies the target table.
    
- `(code, title, credits)`: Defines the target columns for the insert operation.
    
- `VALUES ('CS101', 'Database Fundamentals', 4)`: Provides the corresponding data values.
    
- `RETURNING id, code;`: A **PostgreSQL-specific extension** that immediately returns the specified generated fields (like the auto-incremented `id`) without requiring a separate follow-up query.
    

### 2. SELECT (Read)

SQL

```
SELECT id, code, title, credits 
FROM courses
WHERE credits >= 3
ORDER BY code ASC;
```

- `SELECT id, code, title, credits`: Specifies the list of columns to retrieve.
    
- `FROM courses`: Identifies the target table.
    
- `WHERE credits >= 3`: Filters rows, returning only those where `credits` is greater than or equal to 3.
    
- `ORDER BY code ASC`: Sorts the returned rows alphabetically by `code` in ascending order.
    

### 3. UPDATE (Update)

SQL

```
UPDATE courses
SET credits = 5
WHERE id = 1;
```

- `UPDATE courses`: Identifies the target table.
    
- `SET credits = 5`: Specifies the column modifications to apply.
    
- `WHERE id = 1`: **Crucial filter clause.** Restricts the update operation strictly to the row matching `id = 1`.
    

### 4. DELETE (Delete)

SQL

```
DELETE FROM courses
WHERE id = 1;
```

- `DELETE FROM courses`: Identifies the target table.
    
- `WHERE id = 1`: Restricts the deletion operation strictly to the row matching `id = 1`.
    

> [!warning] Dangerous Queries: Missing `WHERE` Clauses
> 
> Executing an `UPDATE` or `DELETE` statement without a `WHERE` clause applies the operation to **every single row** in the table!
> 
> SQL
> 
> ```
> -- DANGER: SETS ALL CREDITS IN THE TABLE TO 5
> UPDATE courses SET credits = 5;
> 
> -- DANGER: PURGES ALL DATA FROM THE TABLE
> DELETE FROM courses;
> ```

## 16. ALTER TABLE (Schema Evolution)

As software requirements evolve over time, database schemas must be updated using migrations.

SQL

```
-- Adding a column with a default constraint value
ALTER TABLE courses 
ADD COLUMN is_active BOOLEAN DEFAULT TRUE;

-- Removing a column from a table
ALTER TABLE courses 
DROP COLUMN credits;

-- Modifying a column's data type
ALTER TABLE courses 
ALTER COLUMN title TYPE VARCHAR(150);

-- Renaming a column
ALTER TABLE courses 
RENAME COLUMN code TO course_code;
```

> [!important] Production Risks: Schema Alterations and Locks
> 
> Changing table structures (`ALTER TABLE`) requires PostgreSQL to acquire an **Exclusive Lock** (`ACCESS EXCLUSIVE`) on the target table. During an exclusive lock, all incoming `SELECT`, `INSERT`, `UPDATE`, and `DELETE` queries must wait in a queue until the structural change completes.
> 
> Adding a column with a default value to a table with millions of rows can cause service downtime if not handled carefully.

## 17. Indexes

### What is an Index?

An **Index** is a dedicated side structure (most commonly a **B-Tree**) that maintains a sorted pointer list of column values referencing their corresponding physical row locations on disk.

Plaintext

```
Without Index (Sequential Scan)      With B-Tree Index (Index Scan)
   [ Search for code = 'CS101' ]       [ Search for code = 'CS101' ]
               │                                   │
               ▼                                   ▼
      +-----------------+                 +-----------------+
      | Row 1: CS305    |                 |  B-Tree Node    |
      | Row 2: MA101    |                 |   /        \    |
      | Row 3: CS101    | (Finds row 3)   | CS101     MA101 |
      | ... (1M rows)   |                 +-----------------+
      +-----------------+                          │
      Scans every row!                  Direct Pointer Lookup!
```

- **Sequential Scan (`Seq Scan`)**: Reads through every page of a table on disk sequentially to locate matching rows. Very slow for large tables ($O(N)$ time complexity).
    
- **Index Scan**: Navigates the B-Tree structure to instantly locate row addresses ($O(\log N)$ time complexity).
    

### Index Creation Syntax

SQL

```
CREATE INDEX idx_courses_code ON courses(code);
```

- `CREATE INDEX`: Instructs PostgreSQL to build an auxiliary search index structure.
    
- `idx_courses_code`: The unique identifier name assigned to the index object.
    
- `ON courses(code)`: Specifies the target table (`courses`) and column (`code`) to index.
    

### The Index Trade-Off Matrix

Фрагмент кода

```
graph LR
    Index[B-Tree Index Created] --> Read[Read Queries SELECT]
    Index --> Write[Write Queries INSERT UPDATE DELETE]
    Read -->|FASTER| ReadBenefit[Logarithmic Lookup Speed]
    Write -->|SLOWER| WriteCost[Must Update Table AND Rebalance B-Tree]
```

- **When to create indexes**: On primary keys, foreign keys, columns used frequently in `WHERE` clauses, and columns used in `JOIN` conditions.
    
- **When NOT to create indexes**: On tiny tables (where sequential scans are already fast), on columns that are rarely queried, or on columns updated continuously in high-throughput write workloads.
    

## 18. Transactions & ACID Properties

A **Transaction** is a logical unit of work that bundles multiple SQL operations into a single execute-or-rollback block.

### Example: Bank Transfer

SQL

```
BEGIN;

UPDATE accounts 
SET balance = balance - 100 
WHERE id = 1;

UPDATE accounts 
SET balance = balance + 100 
WHERE id = 2;

COMMIT;
```

#### Code Breakdown

- `BEGIN;`: Starts the explicit transaction boundary block.
    
- `UPDATE accounts ...`: First operation in the transaction.
    
- `UPDATE accounts ...`: Second operation in the transaction.
    
- `COMMIT;`: Persists all changes made within the transaction block permanently to disk.
    

If a failure occurs before `COMMIT` (e.g., system crash, network interruption, or constraint violation), you issue a `ROLLBACK`:

SQL

```
ROLLBACK;
```

This cancels all changes made within the uncommitted transaction block, restoring the database to its previous state.

### High-Level ACID Guarantees

- **Atomicity** ("All or Nothing"): Every statement in a transaction succeeds, or the entire transaction is rolled back.
    
- **Consistency**: A transaction can only transition the database from one valid state to another, maintaining all schema constraints.
    
- **Isolation**: Concurrent transactions execute independently without interfering with each other's uncommitted data.
    
- **Durability**: Once a transaction is committed, its changes survive any subsequent power loss or server crash (enforced using Write-Ahead Logging / WAL).
    

## 19. NULL in PostgreSQL

`NULL` represents **missing**, **unknown**, or **unassigned** data. It is not equivalent to zero, an empty string `""`, or boolean `FALSE`.

### Three-Valued Logic Matrix

Because `NULL` signifies "unknown", comparisons involving `NULL` yield `UNKNOWN` rather than `TRUE` or `FALSE`.

|**Expression**|**Evaluates To**|**Explanation**|
|---|---|---|
|`NULL = NULL`|`UNKNOWN` (treated as false)|Is an unknown value equal to another unknown value? Unknown!|
|`1 = NULL`|`UNKNOWN`|Cannot compare a known value to an unknown value.|
|`NULL IS NULL`|`TRUE`|Explicit syntax used to check for the presence of NULL.|
|`NULL IS NOT NULL`|`FALSE`|Explicit syntax used to check for non-NULL values.|

### Correct Query Syntax for NULL

SQL

```
-- INCORRECT: Will yield zero results due to Three-Valued Logic
SELECT * FROM users WHERE email = NULL;

-- CORRECT: Proper syntax for finding NULL records
SELECT * FROM users WHERE email IS NULL;

-- CORRECT: Finding populated records
SELECT * FROM users WHERE email IS NOT NULL;
```

## 20. PostgreSQL Users and Permissions

PostgreSQL uses **Roles** to manage authentication and access control across database objects. A role can function as either a user (can log in) or a group (a collection of permissions).

Фрагмент кода

```
graph TD
    Superuser[postgres Superuser] --> RoleDev[dev_app_user Role]
    RoleDev -->|GRANT SELECT INSERT UPDATE| Tables[university Database Tables]
```

### Creating Roles and Granting Permissions

SQL

```
-- Create a limited application user role
CREATE ROLE dev_user WITH LOGIN PASSWORD 'SecureAppPassword123!';

-- Grant connection access to the university database
GRANT CONNECT ON DATABASE university TO dev_user;

-- Grant table schema permissions
GRANT USAGE ON SCHEMA public TO dev_user;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO dev_user;
```

> [!important] The Principle of Least Privilege
> 
> Never use the `postgres` superuser account for routine application backend connections. If your application code is compromised via SQL injection, an attacker using a superuser connection gains full control over the database system, including the ability to run system commands on the underlying host operating system. Always create a dedicated application role with limited permissions.

## 21. PostgreSQL and FastAPI Integration Concept

When integrating PostgreSQL with a Web Framework like FastAPI, PostgreSQL replaces temporary in-memory data structures (like Python dictionaries or lists) with persistent, ACID-compliant storage.

### Architectural Layering

Plaintext

```
Client Application (Browser / Mobile)
       │ (HTTP Request JSON)
       ▼
[ FastAPI Router Layer ]
       │ (Validates payload via Pydantic Models)
       ▼
[ Service / Controller Layer ]
       │ (Applies core Business Logic rules)
       ▼
[ Database Access Layer ]
       │ (Constructs & sends Raw SQL queries over TCP socket)
       ▼
[ PostgreSQL Server Engine ]
```

### Transitioning from In-Memory Data Structures to PostgreSQL

#### 1. The In-Memory Approach (Temporary / Development Only)

Python

```
# Temporary in-memory list storage (Data is lost when application restarts)
courses_db = []

@app.post("/courses")
def create_course(course: CourseSchema):
    courses_db.append(course.dict())
    return {"status": "success", "data": course}
```

#### 2. The Persistent PostgreSQL Approach

Python

```
# Production persistent model using raw SQL driver
@app.post("/courses")
def create_course(course: CourseSchema):
    query = """
        INSERT INTO courses (code, title, credits) 
        VALUES (%s, %s, %s) 
        RETURNING id;
    """
    # Execute query over database connection context...
    # Commit transaction...
    # Return persisted database object...
```

## 22. Practical Project: University Database System

In this section, we will build a complete database schema for a university system. First, study the completed SQL schema below. Then, complete the practical exercises that follow.

### Complete DDL Script

SQL

```
-- 1. CLEANUP PREVIOUS TARGET OBJECTS
DROP TABLE IF EXISTS enrollments;
DROP TABLE IF EXISTS courses;
DROP TABLE IF EXISTS instructors;
DROP TABLE IF EXISTS students;
DROP TABLE IF EXISTS departments;

-- 2. CREATE DEPARTMENTS TABLE
CREATE TABLE departments (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

-- 3. CREATE INSTRUCTORS TABLE (1:N with Departments)
CREATE TABLE instructors (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    department_id BIGINT REFERENCES departments(id) ON DELETE SET NULL
);

-- 4. CREATE STUDENTS TABLE
CREATE TABLE students (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    age INTEGER CHECK (age >= 16),
    enrolled_date DATE DEFAULT CURRENT_DATE
);

-- 5. CREATE COURSES TABLE (1:N with Instructors)
CREATE TABLE courses (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    code VARCHAR(20) NOT NULL UNIQUE,
    title VARCHAR(150) NOT NULL,
    credits INTEGER CHECK (credits > 0 AND credits <= 6),
    instructor_id BIGINT REFERENCES instructors(id) ON DELETE RESTRICT
);

-- 6. CREATE ENROLLMENTS JUNCTION TABLE (M:N with Composite Key)
CREATE TABLE enrollments (
    student_id BIGINT REFERENCES students(id) ON DELETE CASCADE,
    course_id BIGINT REFERENCES courses(id) ON DELETE CASCADE,
    grade NUMERIC(3, 2) CHECK (grade >= 0.00 AND grade <= 4.00),
    enrolled_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (student_id, course_id)
);

-- 7. CREATE PERFORMANCE INDEXES
CREATE INDEX idx_courses_code ON courses(code);
CREATE INDEX idx_instructors_email ON instructors(email);
```

### Practice Exercises

Write the SQL queries for each scenario below before expanding the hidden solution blocks.

#### Exercise 1: Multi-Table Data Insertion Transaction

Write a single transaction block that adds a department named `'Computer Science'`, adds an instructor named `'Dr. Alan Turing'` (`alan@university.edu`) assigned to that department, and adds a course named `'CS50'` (`Intro to CS`, 4 credits) assigned to Dr. Turing.

SQL

```
BEGIN;

INSERT INTO departments (name) 
VALUES ('Computer Science');

INSERT INTO instructors (full_name, email, department_id) 
VALUES ('Dr. Alan Turing', 'alan@university.edu', 1);

INSERT INTO courses (code, title, credits, instructor_id) 
VALUES ('CS50', 'Intro to CS', 4, 1);

COMMIT;
```

#### Exercise 2: Student Enrollment

Insert a new student named `'Ada Lovelace'` (`ada@university.edu`, age 20). Then, enroll her in the `'CS50'` course (Course ID `1`).

SQL

```
INSERT INTO students (full_name, email, age) 
VALUES ('Ada Lovelace', 'ada@university.edu', 20);

INSERT INTO enrollments (student_id, course_id) 
VALUES (1, 1);
```

## 23. Common Beginner Mistakes

Фрагмент кода

```
graph TD
    Mistake1[Using Superuser postgres] --> AntiPattern[Security Vulnerability]
    Mistake2[UPDATE / DELETE without WHERE] --> AntiPattern[Data Corruption / Loss]
    Mistake3[Over-Indexing Columns] --> AntiPattern[Degraded Write Performance]
    Mistake4[Using REAL for Money] --> AntiPattern[Rounding & Precision Errors]
    Mistake5[Treating NULL as empty string] --> AntiPattern[Logical Query Faults]
```

### 1. Using the `postgres` Superuser Account in Applications

- _Anti-Pattern_: Configuring your application connection string with `postgresql://postgres:password@localhost:5432/db`.
    
- _Correction_: Create dedicated, low-privilege application roles using `GRANT` statements.
    

### 2. Missing `WHERE` Clauses in Modifying Queries

- _Anti-Pattern_: Running `UPDATE courses SET title = 'Database Systems';` without a filter.
    
- _Correction_: Always double-check and include explicit primary key filters (`WHERE id = 1`) when running modifying queries.
    

### 3. Creating Indexes Unnecessarily

- _Anti-Pattern_: Indexing every single column in a table.
    
- _Correction_: Create indexes strategically based on measured slow queries and common access patterns (`WHERE`, `JOIN`, `ORDER BY`).
    

### 4. Choosing Inappropriate Data Types

- _Anti-Pattern_: Using `REAL` or `FLOAT` for financial data, or using `TEXT` to store dates.
    
- _Correction_: Use standard types designed for specific data kinds: `NUMERIC` for currency, `TIMESTAMPTZ` for timestamps, and `INTEGER`/`BIGINT` for counts and keys.
    

## 24. Database Engine Comparison: PostgreSQL vs. MySQL vs. SQLite

|**Feature / Criteria**|**PostgreSQL**|**MySQL**|**SQLite**|
|---|---|---|---|
|**Architecture**|Client-Server (Multi-process)|Client-Server (Multi-threaded)|Serverless (Embedded process file)|
|**Concurrency Model**|Advanced MVCC (Row-level)|MVCC via InnoDB engine|Table/File-level write lock|
|**Primary Use Cases**|Enterprise backends, complex queries, analytics|Web applications, general CMS|Mobile apps, local CLI tools, prototyping|
|**Data Integrity Standards**|Strict standard compliance|Historic permissive conversions|Flexible type affinities|
|**JSON Support**|Exceptional (Native binary `JSONB` + GIN indexing)|Native `JSON` document support|Text-based JSON functions|
|**ACID Compliance**|Full ACID compliance always|Full ACID compliance (InnoDB engine)|Full ACID compliance|
|**Scalability Target**|Medium to massive enterprise backends|Medium to massive web platforms|Small apps / edge environments|

### Why PostgreSQL is Selected for Modern Backend Architectures

PostgreSQL offers an ideal foundation for modern backend applications. Its strict type checking and constraint systems catch data issues at the database level. Native support for `JSONB` allows developers to combine relational and document data patterns when needed, while extensions like `pgvector` enable modern AI integrations directly inside the database.

## 25. Interview Preparation Questions

#### 1. What is the fundamental difference between standard SQL and PostgreSQL?

SQL is an ANSI/ISO standard database query language specification. PostgreSQL is an actual relational database engine implementation that executes SQL queries while offering additional features like JSONB support, custom types, and extensions.

#### 2. Why is `TIMESTAMPTZ` preferred over `TIMESTAMP` in production environments?

`TIMESTAMPTZ` converts timestamps to UTC upon storage and converts them to the client's local timezone upon retrieval. Plain `TIMESTAMP` ignores timezone offsets entirely, which can lead to data inconsistencies across different server regions.

#### 3. What is the difference between a Surrogate Key and a Natural Key?

A natural key uses an existing business attribute (like an SSN or ISBN) to identify rows. A surrogate key uses an arbitrary, database-generated value (like an auto-incrementing integer or UUID) with no external business meaning.

#### 4. How does `JSONB` differ from standard `JSON` in PostgreSQL?

`JSON` stores the exact raw text representation of JSON documents, requiring re-parsing on every query. `JSONB` stores JSON data in a decomposed binary format, which makes querying and index placement much faster at the cost of a tiny processing overhead during writes.

#### 5. What are the four guarantees of ACID?

- **Atomicity**: All operations in a transaction complete, or all are rolled back.
    
- **Consistency**: Data always satisfies all schema constraints.
    
- **Isolation**: Concurrent transactions do not observe each other's uncommitted data changes.
    
- **Durability**: Committed changes are written to persistent storage and survive system failures.
    

#### 6. What is the operational cost of adding a B-Tree index to a table?

While indexes accelerate search operations (`SELECT` statements with `WHERE` clauses), they slow down write operations (`INSERT`, `UPDATE`, `DELETE`) because the database must update both the target table and the B-Tree index structure on disk.

#### 7. Why shouldn't floating-point types (`REAL`/`DOUBLE PRECISION`) be used for currency?

Floating-point data types use binary representation (IEEE 754), which cannot represent certain base-10 fractions exactly. This leads to rounding errors in arithmetic operations. `NUMERIC`/`DECIMAL` types store exact numbers and avoid these rounding errors.

#### 8. What is a Composite Primary Key, and where is it used?

A composite primary key is a primary key that consists of two or more columns combined. It is most commonly used in junction tables for Many-to-Many relationships to ensure that unique pairs of keys cannot be duplicated.

#### 9. What is the purpose of the `RETURNING` clause in PostgreSQL?

The `RETURNING` clause allows an `INSERT`, `UPDATE`, or `DELETE` statement to return specified column values from affected rows immediately, eliminating the need for an extra `SELECT` query.

#### 10. How does PostgreSQL handle `NULL` in comparison operations?

PostgreSQL uses Three-Valued Logic (`TRUE`, `FALSE`, `UNKNOWN`). Because `NULL` represents an unknown value, direct comparisons like `NULL = NULL` evaluate to `UNKNOWN`. Developers must use `IS NULL` or `IS NOT NULL` syntax instead.

#### 11. What is the difference between `DELETE` and `TRUNCATE`?

`DELETE` is a DML statement that removes rows one by one, firing triggers and checking constraints. `TRUNCATE` is a DDL command that instantly empties a table by reallocating its data storage pages, which is much faster but bypasses individual row-level triggers.

#### 12. What does `ON DELETE CASCADE` do on a foreign key constraint?

It automatically deletes child rows in a dependent table when the corresponding parent row is deleted from the referenced table.

#### 13. What is the difference between `VARCHAR(n)` and `TEXT` in PostgreSQL?

`VARCHAR(n)` restricts string length to a maximum of `n` characters. `TEXT` allows strings of unlimited length. In PostgreSQL, both performance characteristics are identical under the hood.

#### 14. What is a PostgreSQL Schema?

A schema is an organizational namespace within a database. It allows tables, views, and types to be grouped logically into separate folders (e.g., `public`, `audit`, `billing`) inside a single database.

#### 15. What is the difference between `GENERATED ALWAYS AS IDENTITY` and `SERIAL`?

Identity columns follow ANSI SQL standards, whereas `SERIAL` is a legacy PostgreSQL type. Identity columns prevent unintentional manual overrides of auto-incrementing ID values unless explicit override clauses are provided.

#### 16. What is the function of the Write-Ahead Log (WAL)?

The WAL records all database changes to disk _before_ they are written to main data files. This ensures Durability, allowing PostgreSQL to recover unwritten data changes following an unexpected system crash.

#### 17. How do you list all tables using `psql`?

Run the `\dt` meta-command in the `psql` terminal.

#### 18. What is the default network port for a PostgreSQL server?

TCP Port `5432`.

#### 19. What is a Partial Index?

An index built over a subset of rows in a table defined by a `WHERE` clause (e.g., `CREATE INDEX ON users(email) WHERE is_active = true;`). This saves disk space and speeds up updates.

#### 20. What is Multi-Version Concurrency Control (MVCC)?

MVCC is a concurrency control mechanism where PostgreSQL maintains multiple versions of a row concurrently. This allows readers to access stable snapshots of data without blocking writers, and writers to modify data without blocking readers.

#### 21. What happens if you run `UPDATE` without a `WHERE` clause?

The update operation applies to **every row** in the table, potentially overwriting critical data.

#### 22. What is the role of the Query Planner?

The Query Planner analyzes parsed SQL statements and evaluates different execution paths (e.g., Sequential Scan vs. Index Scan) to choose the most efficient plan based on table statistics.

#### 23. What command switches the database context in `psql`?

The `\c database_name` meta-command.

#### 24. What is a Check Constraint?

A check constraint evaluates a boolean expression for inserted or updated values. If the expression evaluates to `FALSE`, the transaction is rejected with an error.

#### 25. Why should an application connect using a restricted role rather than the superuser?

To enforce the principle of least privilege. Using a restricted role limits the potential damage from security vulnerabilities (like SQL injection) by preventing unauthorized schema changes or administrative access.

## 26. Conceptual Knowledge Assessment

Use these self-assessment scenarios to test your understanding of PostgreSQL concepts.

### Scenario Questions

1. **System Design**: You are designing an e-commerce platform. Explain which PostgreSQL data types you would select for `order_id`, `total_amount`, `is_paid`, and `created_at`, and justify each choice.
    
2. **Concurrency Evaluation**: Imagine User A initiates a transaction modifying a record, but has not yet executed `COMMIT`. User B runs a `SELECT` query on that same record. What data does User B see? Which ACID principle governs this behavior?
    
3. **Constraint Logic**: A table contains an `age` column with a `CHECK (age >= 18)` constraint. What happens if a client attempts to execute an `INSERT` statement with an explicit `NULL` value for the `age` column? (Hint: Consider Three-Valued Logic).
    
4. **Database Layout**: Describe the physical hierarchy of database objects starting from the PostgreSQL Server Instance down to a column in a table.
    
5. **Index Diagnostics**: A query containing `WHERE UPPER(email) = 'USER@DOMAIN.COM'` is running slowly despite having a standard index on `email`. Why is the index being bypassed, and how can you fix it?
    

### Schema Debugging Exercise

Analyze the following SQL DDL script. Identify **three structural errors or poor design choices**:

SQL

```
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_email VARCHAR(255) REFERENCES customers(email),
    order_total REAL NOT NULL,
    status VARCHAR(20) DEFAULT 'pending'
);
```

1. **Use of `REAL` for Currency**: `order_total` uses a floating-point type (`REAL`), which can introduce rounding errors. It should use `NUMERIC(10, 2)` instead.
    
2. **Foreign Key to Non-Unique Field**: `customer_email` references `customers(email)`. Unless `email` in `customers` is explicitly enforced with a `UNIQUE` or `PRIMARY KEY` constraint, this foreign key creation will fail. Foreign keys should typically target surrogate primary keys (`customer_id BIGINT`).
    
3. **Legacy Serial Type**: Using `SERIAL` is considered legacy syntax. Modern designs should prefer standard Identity Columns (`id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY`).
    

## 27. Hands-on Practical Exercises

### Level 1: Core Operations

1. Launch `psql` or `pgAdmin` and create a database named `company_db`.
    
2. Create a table named `employees` with columns: `id` (Identity Primary Key), `first_name` (VARCHAR 50, NOT NULL), `last_name` (VARCHAR 50, NOT NULL), `salary` (NUMERIC 10,2), and `hire_date` (DATE, Default CURRENT_DATE).
    
3. Insert three employee records into the table.
    
4. Write a query retrieving all employees hired in the current year with a salary greater than $50,000.
    
5. Update an employee's salary using their primary key ID.
    
6. Delete one employee record using their primary key ID.
    

### Level 2: Relationships and Integrity Constraints

1. Create a parent table named `projects` with columns `id` and `project_name`.
    
2. Add a foreign key column named `project_id` to the `employees` table that references `projects(id)` with `ON DELETE SET NULL`.
    
3. Add a `CHECK` constraint to the `employees` table ensuring `salary > 0`.
    
4. Create a junction table named `employee_skills` to link `employees` to a new `skills` table (Many-to-Many relationship).
    
5. Build a B-Tree index on `last_name` in the `employees` table.
    
6. Execute a transaction that creates a new project and assigns two existing employees to it. If any query fails, roll back the entire transaction.
    

### Level 3: Advanced Architecture Migration Scenario

> [!tip] Practical Exercise Context
> 
> Suppose you are transitioning a FastAPI application from temporary in-memory Python lists to persistent PostgreSQL storage:
> 
> Python
> 
> ```
> # Legacy In-Memory Application State
> users_db = [
>     {"id": 1, "username": "dev_user", "email": "dev@test.com"},
> ]
> posts_db = [
>     {"id": 101, "author_id": 1, "content": "Hello World", "published": True},
> ]
> ```

#### Goal

Design and write a complete PostgreSQL SQL DDL script that implements persistent tables to replace `users_db` and `posts_db`.

#### Requirements

- Choose appropriate data types for primary keys, text fields, and state flags.
    
- Enforce `NOT NULL` and `UNIQUE` constraints where appropriate.
    
- Implement a Foreign Key connecting `posts` to `users`, ensuring that deleting a user automatically removes all of their associated posts.
    
- Add a performance index to accelerate queries filtering posts by author ID.
    

SQL

```
-- 1. Create Target Users Table
CREATE TABLE users (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

-- 2. Create Target Posts Table
CREATE TABLE posts (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    author_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

-- 3. Create Performance Index on Foreign Key Search Column
CREATE INDEX idx_posts_author_id ON posts(author_id);
```

## 28. Core Summary & Skill Checklist

### Summary Map

Plaintext

```
PostgreSQL Core Knowledge Architecture
├── Server Infrastructure
│   ├── Instance (Engine processes & Port 5432 listening)
│   ├── Databases (Logical isolation boundaries)
│   └── Schemas (Namespaces like 'public')
├── Data Definition (DDL)
│   ├── Tables & Columns
│   ├── Data Types (INTEGER, NUMERIC, TIMESTAMPTZ, JSONB)
│   └── Identity Primary Keys & Constraints (UNIQUE, CHECK)
├── Relational Engineering
│   ├── Foreign Key References
│   ├── One-to-Many (1:N) & Junction Many-to-Many (M:N)
│   └── Referential Integrity (CASCADE, RESTRICT)
└── Data Operations & Control
    ├── CRUD Queries (INSERT, SELECT, UPDATE, DELETE)
    ├── B-Tree Search Indexes (Index Scan vs Seq Scan)
    └── Transactions & ACID Compliance (BEGIN, COMMIT, ROLLBACK)
```

### Pre-ORM Mastery Checklist

Before moving on to Object-Relational Mapping libraries (like SQLAlchemy), ensure you can confidently complete the following checklist:

- [ ] Explain the difference between standard SQL syntax and the PostgreSQL engine implementation.
    
- [ ] Connect to a local PostgreSQL instance using both `psql` CLI and `pgAdmin` GUI.
    
- [ ] Create databases, custom schemas, and tables using DDL commands.
    
- [ ] Choose appropriate PostgreSQL data types for monetary values, timestamps, and primary keys.
    
- [ ] Implement Primary Keys using modern Identity Columns (`GENERATED ALWAYS AS IDENTITY`).
    
- [ ] Enforce data integrity using `NOT NULL`, `UNIQUE`, `CHECK`, and `DEFAULT` constraints.
    
- [ ] Model One-to-Many and Many-to-Many relationships using Foreign Keys and Junction Tables.
    
- [ ] Execute standard CRUD operations (`INSERT ... RETURNING`, `SELECT`, `UPDATE`, `DELETE`) with proper `WHERE` filtering.
    
- [ ] Create B-Tree indexes and explain their impact on read and write performance.
    
- [ ] Wrap multiple SQL statements inside ACID-compliant transaction blocks (`BEGIN`, `COMMIT`, `ROLLBACK`).
    
- [ ] Explain how PostgreSQL handles `NULL` values using Three-Valued Logic.
    
- [ ] Create custom database roles and apply permissions using the Principle of Least Privilege.


[[Database_design_&_Relational_modeling]]