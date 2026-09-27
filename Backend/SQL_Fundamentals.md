
## 1. Why Databases Exist

In your FastAPI projects so far, you likely stored data using Python data structures like dictionaries or lists in memory:

Python

```
# app/main.py
courses_db: list[dict] = [
    {"id": 1, "title": "FastAPI Basics", "credits": 3},
    {"id": 2, "title": "Python Intermediate", "credits": 4}
]
```

While this approach works for quick prototypes, it fails in production due to fundamental limitations of **volatile memory (RAM)**.

### Problems with In-Memory Storage

1. **Loss of Persistence:** RAM requires continuous electrical power. Restarting your server, deploying a code update, or experiencing a crash completely wipes out `courses_db`.
    
2. **Memory Constraints:** RAM is fast but expensive and limited in size. Storing millions of records in Python lists will cause your application to run out of memory (OOM).
    
3. **Multi-User Conflicts & Data Corruption:** If two API requests arrive at the same millisecond and attempt to modify `courses_db.append()` or update an item, race conditions can corrupt your data.
    
4. **Lack of Indexing & Search Efficiency:** Finding a single record in a Python list requires scanning elements one by one—an $O(n)$ linear operation that slows down as data grows.
    

Фрагмент кода

```
graph TD
    subgraph RAM ["Volatile Memory (RAM)"]
        A[FastAPI App Instance] --> B["courses_db = [...]"]
    end
    
    subgraph Disk ["Non-Volatile Storage (Disk)"]
        C[(Relational Database)]
    end

    D[Server Restart / Deployment] -. Clears .-> B
    A -- Persistent Read/Write --> C
```

### Core Benefits of a Database

- **Persistence:** Data is written safely to non-volatile storage (SSD/HDD) and survives system reboots.
    
- **Reliability & Consistency:** Databases use specialized logs to ensure operations complete fully or roll back if an error occurs.
    
- **Scalability & Indexing:** Databases use data structures like B-Trees to locate a single row among millions in $O(\log n)$ logarithmic time.
    
- **Multi-User Concurrency:** Built-in locking mechanisms allow thousands of concurrent users to read and write without data corruption.
    

### Python Lists vs. Database Tables

|**Feature**|**Python List (list[dict])**|**Database Table**|
|---|---|---|
|**Location**|RAM (Volatile)|Disk + Smart Cache (Persistent)|
|**Data Lifecycle**|Erased on app restart|Persists permanently|
|**Search Speed**|$O(n)$ Linear scan|$O(\log n)$ with Indexes|
|**Concurrency**|Requires manual thread locking|Automatic multi-user handling|
|**Scale Limit**|Megabytes (Limited by RAM)|Terabytes / Petabytes|

## 2. What is SQL?

**SQL** stands for **Structured Query Language**. It is a declarative programming language designed specifically for managing and manipulating data stored in relational databases.

> [!important] SQL is a Language, Not a Database System
> 
> - **Python** is a language; **CPython** or **PyPy** are runtime implementations.
>     
> - **SQL** is the standard language specification; **PostgreSQL**, **MySQL**, and **SQLite** are Database Management Systems (DBMS) that execute that language.
>     

### Imperative vs. Declarative Programming

- **Python is Imperative:** You instruct the computer _how_ to do something step by step.
    
    Python
    
    ```
    # How to get active courses:
    active_courses = []
    for course in courses_db:
        if course["is_active"] == True:
            active_courses.append(course)
    ```
    
- **SQL is Declarative:** You specify _what_ data you want, and the database engine decides the optimal execution path.
    
    SQL
    
    ```
    SELECT * FROM courses WHERE is_active = true;
    ```
    

### Popular Relational Database Engines

Фрагмент кода

```
graph LR
    SQL[SQL Standard Standardized by ANSI / ISO]
    
    SQL --> PG[PostgreSQL]
    SQL --> MY[MySQL]
    SQL --> SL[SQLite]
    SQL --> SS[SQL Server]
    SQL --> OR[Oracle]
```

|**Engine**|**Ideal Use Case**|**Key Characteristics**|
|---|---|---|
|**SQLite**|Mobile apps, local development, small edge devices|Serverless, stores database in a single local `.db` file.|
|**PostgreSQL**|Modern web backends, complex API infrastructure|Feature-rich, highly standards-compliant, open-source.|
|**MySQL**|Web apps, e-commerce platforms, CMS platforms|High performance, widely supported across hosting providers.|
|**SQL Server**|Enterprise Microsoft ecosystems|Proprietary, deeply integrated with .NET / Windows systems.|
|**Oracle**|Large enterprise financial and legacy systems|High cost, complex enterprise administration features.|

All these systems understand basic SQL commands. Minor differences in syntax between engines are called **SQL Dialects**.

## 3. Relational Databases

Relational databases structure data into rigid, organized grids called **tables**.

### Core Structural Concepts

1. **Database:** A high-level container holding a collection of related tables, schemas, and permissions.
    
2. **Table:** A structured collection of data organized into rows and columns, representing an entity (e.g., `students`, `courses`).
    
3. **Column (Field / Attribute):** Defines a specific property of the entity, such as `first_name` or `created_at`. Each column must have a defined data type (e.g., `VARCHAR`, `INTEGER`, `BOOLEAN`).
    
4. **Row (Record / Entry):** A single instance of an entity holding concrete values for each column.
    
5. **Schema:** The structural blueprint defining tables, column names, data types, and constraints across the database.
    

### Visualizing Table Structure: `courses`

```
Table: courses
+----+-----------------------+---------+-----------+
| id | title                 | credits | is_active |  <-- Columns (Fields)
+----+-----------------------+---------+-----------+
| 1  | FastAPI Fundamentals  | 3       | true      |  <-- Row 1 (Record)
| 2  | SQL Fundamentals      | 4       | true      |  <-- Row 2 (Record)
| 3  | Legacy Systems        | 2       | false     |  <-- Row 3 (Record)
+----+-----------------------+---------+-----------+
```

Фрагмент кода

```
erDiagram
    COURSES {
        INTEGER id PK
        VARCHAR title
        INTEGER credits
        BOOLEAN is_active
    }
    STUDENTS {
        INTEGER id PK
        VARCHAR email
        VARCHAR full_name
    }
```

## 4. Primary Keys

A **Primary Key (PK)** is a column (or set of columns) that uniquely identifies each individual row in a database table.

> [!important] The Rules of Primary Keys
> 
> 1. **Uniqueness:** No two rows in the same table can share the same Primary Key value.
>     
> 2. **Non-Nullability:** A Primary Key can **never** be `NULL` (empty).
>     
> 3. **Immutability:** Primary Key values should rarely or never change once assigned.
>     

### Why Names Make Poor Identifiers

Using natural data like names, email addresses, or phone numbers as identifiers creates operational edge cases:

- Two students can share the name `"Alex Smith"`.
    
- Users frequently change their email addresses or phone numbers.
    
- Primary Key modifications require costly updates across dependent systems.
    

### Natural Keys vs. Surrogate Keys

- **Natural Key:** A real-world attribute that is unique by nature (e.g., Passport Number, Social Security Number, VIN). _Drawback:_ Regulations, privacy concerns, or unexpected format changes can complicate their use.
    
- **Surrogate Key:** An artificially generated identifier with no real-world meaning, created purely for database management (e.g., an auto-incrementing integer `1, 2, 3...` or a UUID `e3b0c442-98fc-4...`).
    

```
BAD: Identified by Name (Non-Unique)
+------------+------------------+
| full_name  | email            |
+------------+------------------+
| Alex Smith | alex1@gmail.com  |
| Alex Smith | alex2@yahoo.com  | <-- Ambiguous reference!
+------------+------------------+

GOOD: Identified by Surrogate Primary Key (Unique)
+----+------------+------------------+
| id | full_name  | email            |
+----+------------+------------------+
| 1  | Alex Smith | alex1@gmail.com  |
| 2  | Alex Smith | alex2@yahoo.com  | <-- Unambiguous
+----+------------+------------------+
```

## 5. Basic SQL Syntax

SQL code consists of declarative **statements**, which are built using **keywords**, identifiers (table/column names), and operators.

### Fundamental Syntax Rules

1. **Statements end with semicolons (`;`):** The semicolon signals to the database parser that a command is complete and ready to execute.
    
2. **SQL is Case-Insensitive (By Convention, UPPERCASE Keywords):**
    
    - Valid: `select * from courses;`
        
    - Standard Practice: `SELECT * FROM courses;`
        
    - _Best Practice:_ Write SQL keywords in **UPPERCASE** and database identifiers (tables, columns) in **lowercase_snake_case**.
        
3. **Spacing and Line Breaks are Ignored:** SQL engines ignore extra whitespace, so you can format queries across multiple lines for readability.
    

SQL

```
-- This is a single-line SQL comment

/*
   This is a multi-line SQL comment.
   Use it to document complex queries.
*/

SELECT
    id,
    title,
    credits
FROM
    courses;
```

## 6. Reading Data: The `SELECT` Statement

The `SELECT` statement retrieves rows and columns from one or more tables.

### Selecting All Columns (`SELECT *`)

SQL

```
SELECT * FROM courses;
```

#### Line-by-Line Breakdown:

- `SELECT`: Instructs the database that this is a read query.
    
- `*`: A wildcard symbol meaning _"retrieve all columns available in this table"_.
    
- `FROM courses`: Specifies the target table (`courses`) from which data will be fetched.
    
- `;`: Ends the SQL statement.
    

> [!warning] Avoid `SELECT *` in Production APIs
> 
> Fetching all columns via `*` wastes memory and bandwidth. It can break backend bindings if columns are reordered or added, and prevents the database engine from using performance optimizations. Always explicitly select only the required columns.

### Selecting Specific Columns

SQL

```
SELECT title, credits FROM courses;
```

#### Line-by-Line Breakdown:

- `SELECT title, credits`: Tells the database engine to extract and return _only_ the `title` and `credits` columns.
    
- `FROM courses`: Specifies the target source table.
    
- `;`: Ends the SQL statement.
    

### What the Database Engine Does Internally

1. **Parsing:** Parses syntax to verify correctness.
    
2. **Validation:** Confirms that the `courses` table and the columns `title` and `credits` exist.
    
3. **Execution Plan:** Determines the most efficient way to access data on disk.
    
4. **Data Extraction:** Reads the specified columns from storage into memory.
    
5. **Result Set Generation:** Formats the extracted data into a tabular response and returns it to the client.
    

## 7. Filtering Data: The `WHERE` Clause

The `WHERE` clause filters rows based on criteria, returning only records where the specified condition evaluates to `TRUE`.

### Comparison Operators

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`=`|Equal to|`credits = 3`|
|`!=` or `<>`|Not equal to|`is_active != false`|
|`<`|Less than|`credits < 4`|
|`<=`|Less than or equal to|`credits <= 3`|
|`>`|Greater than|`credits > 2`|
|`>=`|Greater than or equal to|`credits >= 3`|

### Example Query

SQL

```
SELECT id, title, credits
FROM courses
WHERE credits = 3;
```

#### Line-by-Line Breakdown:

- `SELECT id, title, credits`: Specifies the three columns to return.
    
- `FROM courses`: Targets the `courses` table.
    
- `WHERE credits = 3`: Evaluates each row individually; only rows where `credits` equals `3` are retained.
    
- `;`: Terminating semicolon.
    

### Python Mental Model

Filtering in SQL with `WHERE` works similarly to a Python list comprehension or `filter()` block:

Python

```
# Equivalent logic in Python:
results = [
    {"id": course["id"], "title": course["title"], "credits": course["credits"]}
    for course in courses_db
    if course["credits"] == 3
]
```

### Filtering Flow Representation

```
Input Rows (courses table)
+----+-----------------------+---------+
| id | title                 | credits |
+----+-----------------------+---------+
| 1  | FastAPI Fundamentals  | 3       | ---> Evaluates 3 = 3 (TRUE)  ---> Included
| 2  | SQL Fundamentals      | 4       | ---> Evaluates 4 = 3 (FALSE) ---> Filtered Out
| 3  | Intro to Web          | 3       | ---> Evaluates 3 = 3 (TRUE)  ---> Included
+----+-----------------------+---------+
```

## 8. Logical Operators (`AND`, `OR`, `NOT`)

Logical operators combine multiple conditions in a `WHERE` clause.

### Operator Behavior

- **`AND`**: Returns `TRUE` only if **all** combined conditions are `TRUE`.
    
- **`OR`**: Returns `TRUE` if **at least one** condition is `TRUE`.
    
- **`NOT`**: Inverts a boolean condition (`TRUE` becomes `FALSE`, `FALSE` becomes `TRUE`).
    

### Truth Table Reference

|**Condition A**|**Condition B**|**A AND B**|**A OR B**|**NOT A**|
|---|---|---|---|---|
|`TRUE`|`TRUE`|**`TRUE`**|**`TRUE`**|`FALSE`|
|`TRUE`|`FALSE`|`FALSE`|**`TRUE`**|`FALSE`|
|`FALSE`|`TRUE`|`FALSE`|**`TRUE`**|`TRUE`|
|`FALSE`|`FALSE`|`FALSE`|`FALSE`|`TRUE`|

### Example Using `AND`

SQL

```
SELECT title, credits, is_active
FROM courses
WHERE credits >= 3 AND is_active = true;
```

#### Line-by-Line Breakdown:

- `SELECT title, credits, is_active`: Selects three columns.
    
- `FROM courses`: Reads from the `courses` table.
    
- `WHERE credits >= 3 AND is_active = true`: Filters for rows where `credits` is 3 or more **AND** `is_active` is explicitly `true`.
    
- `;`: Ends statement.
    

### Evaluation Order & Parentheses

Like algebraic expressions, SQL evaluates `AND` before `OR`. Use parentheses `()` to enforce the desired logical grouping.

SQL

```
-- Unintended evaluation due to default precedence (AND before OR):
SELECT * FROM courses
WHERE is_active = true AND credits = 3 OR credits = 4;

-- Clear, explicit grouping using Parentheses:
SELECT * FROM courses
WHERE is_active = true AND (credits = 3 OR credits = 4);
```

## 9. Pattern Matching (`LIKE`, Wildcards)

The `LIKE` operator enables string searching using wildcards.

### SQL Wildcard Symbols

- **`%` (Percent):** Matches **zero or more** characters.
    
- **`_` (Underscore):** Matches **exactly one** character.
    

### Practical Examples

#### 1. Prefix Match (Starts With)

Find all courses starting with the letters `"Intro"`:

SQL

```
SELECT title FROM courses
WHERE title LIKE 'Intro%';
```

#### 2. Suffix Match (Ends With)

Find all courses ending with the string `"101"`:

SQL

```
SELECT title FROM courses
WHERE title LIKE '%101';
```

#### 3. Substring Match (Contains)

Find all courses containing the term `"Data"` anywhere in the title:

SQL

```
SELECT title FROM courses
WHERE title LIKE '%Data%';
```

#### 4. Exact Character Count Match

Find 4-letter instructor codes starting with `'A'` and ending with `'x'`:

SQL

```
SELECT full_name FROM instructors
WHERE code LIKE 'A__x'; -- 'Alex', 'Apex', 'Amsx'
```

> [!note] Case Sensitivity Note
> 
> In standard SQL, `LIKE` is case-sensitive. Some systems offer `ILIKE` for case-insensitive matching.

## 10. Sorting Results: `ORDER BY`

Relational tables treat rows as unordered sets. To view output in a specific sequence, use the `ORDER BY` clause.

### Ordering Keywords

- **`ASC` (Ascending):** Sorts smallest to largest, A-Z, or earliest date to latest date (Default).
    
- **`DESC` (Descending):** Sorts largest to smallest, Z-A, or latest date to earliest date.
    

### Example: Single Column Sorting

SQL

```
SELECT title, credits
FROM courses
ORDER BY credits DESC;
```

#### Line-by-Line Breakdown:

- `SELECT title, credits`: Selects requested columns.
    
- `FROM courses`: Identifies the target table.
    
- `ORDER BY credits DESC`: Sorts output rows from the highest number of `credits` down to the lowest.
    
- `;`: Ends the query statement.
    

### Example: Multi-Column Sorting

SQL

```
SELECT title, credits, instructor
FROM courses
ORDER BY credits DESC, title ASC;
```

#### Execution Logic:

1. Rows are sorted primarily by `credits` in **descending** order.
    
2. If multiple rows share the same `credits` value, those tied records are sorted alphabetically by `title` in **ascending** order.
    

## 11. Limiting Results: `LIMIT`

The `LIMIT` clause restricts the maximum number of rows returned by a query.

SQL

```
SELECT id, title, credits
FROM courses
ORDER BY credits DESC
LIMIT 3;
```

#### Line-by-Line Breakdown:

- `SELECT id, title, credits`: Projects target attributes.
    
- `FROM courses`: Reads from `courses`.
    
- `ORDER BY credits DESC`: Orders courses starting from highest credit count.
    
- `LIMIT 3`: Instructs the engine to return only the first 3 rows of that sorted output.
    
- `;`: Statement termination.
    

### API Pagination Use Case

In REST APIs, returning large datasets at once can degrade performance. Backend applications use `LIMIT` to implement paginated responses:

Python

```
# Example FastAPI Endpoint pattern translated to SQL logic
# GET /api/v1/courses?limit=10
@app.get("/courses")
def get_courses(limit: int = 10):
    query = f"SELECT * FROM courses ORDER BY id ASC LIMIT {limit};"
    return execute_sql(query)
```

## 12. Inserting Data: `INSERT INTO`

The `INSERT INTO` statement creates new rows in a database table.

SQL

```
INSERT INTO courses (title, credits, is_active)
VALUES ('FastAPI Advanced', 4, true);
```

#### Line-by-Line Breakdown:

- `INSERT INTO courses`: Specifies the target table to insert data into.
    
- `(title, credits, is_active)`: An explicit list of target columns receiving values. Note that `id` is omitted because auto-incrementing Primary Keys are handled automatically by the database.
    
- `VALUES`: Introduces the concrete data row to inject.
    
- `('FastAPI Advanced', 4, true)`: Provides values matching the exact sequence and data type of specified columns (string, integer, boolean).
    
- `;`: Ends statement.
    

### Python Mental Model

Inserting a record into a SQL table is equivalent to appending a structured dictionary to a list in Python:

Python

```
# Equivalent operation in Python:
courses_db.append({
    "id": generate_next_id(), # Generated automatically by DB engine
    "title": "FastAPI Advanced",
    "credits": 4,
    "is_active": True
})
```

## 13. Updating Data: `UPDATE`

The `UPDATE` statement modifies values in existing rows.

SQL

```
UPDATE courses
SET credits = 5, is_active = true
WHERE id = 1;
```

#### Line-by-Line Breakdown:

- `UPDATE courses`: Identifies the table containing target records.
    
- `SET credits = 5, is_active = true`: Defines the columns to modify and assigns their new values.
    
- `WHERE id = 1`: Scopes the operation so that **only** the row where Primary Key `id` equals `1` is updated.
    
- `;`: Statement termination.
    

> [!danger] The Omitted `WHERE` Clause Disaster
> 
> Omitting the `WHERE` clause applies the change to **every single row** in the table!
> 
> SQL
> 
> ```
> -- DANGER: Updates the credit count of ALL courses to 5!
> UPDATE courses SET credits = 5;
> ```

### Python Mental Model

Updating a SQL table with a `WHERE` clause is equivalent to finding a dictionary in a list by key and updating its properties:

Python

```
# Equivalent operation in Python:
for course in courses_db:
    if course["id"] == 1:
        course["credits"] = 5
        course["is_active"] = True
        break
```

## 14. Deleting Data: `DELETE`

The `DELETE` statement removes existing rows from a table.

SQL

```
DELETE FROM courses
WHERE id = 3;
```

#### Line-by-Line Breakdown:

- `DELETE FROM courses`: Specifies the target table from which records will be removed.
    
- `WHERE id = 3`: Restricts deletion exclusively to the record where Primary Key `id` equals `3`.
    
- `;`: Ends statement execution.
    

> [!warning] Critical Safety Precaution
> 
> If you run `DELETE FROM courses;` without a `WHERE` clause, the database will delete **all records** in the table while keeping the table structure intact.
> 
> **Safeguard Rule:** Always write your `SELECT * FROM table WHERE condition;` query first to verify which rows will be affected before converting it into a `DELETE` command.

## 15. Aggregate Functions

Aggregate functions run calculations across a set of column values and return a single summary value.

|**Function**|**Purpose**|**Example Output**|
|---|---|---|
|`COUNT()`|Counts total number of rows/non-null values|`42`|
|`SUM()`|Calculates total sum of numeric column|`120.50`|
|`AVG()`|Computes arithmetic average|`3.14`|
|`MIN()`|Identifies lowest value|`1`|
|`MAX()`|Identifies highest value|`100`|

### Combined Aggregation Example

SQL

```
SELECT 
    COUNT(*) AS total_courses,
    AVG(credits) AS average_credits,
    MAX(credits) AS max_credits
FROM courses;
```

#### Line-by-Line Breakdown:

- `SELECT`: Begins calculation projection.
    
- `COUNT(*) AS total_courses`: Counts all rows matching criteria and renames output column to `total_courses` using the `AS` alias keyword.
    
- `AVG(credits) AS average_credits`: Computes average of the numeric `credits` column.
    
- `MAX(credits) AS max_credits`: Determines the highest value present in `credits`.
    
- `FROM courses`: Source table.
    
- `;`: Ends query statement.
    

## 16. Removing Duplicates: `DISTINCT`

The `DISTINCT` keyword filters out duplicate values from your query output, returning only unique values.

SQL

```
SELECT DISTINCT credits
FROM courses;
```

#### Example Breakdown:

If your `courses` table has rows with credit values `[3, 4, 3, 2, 4, 3]`, executing `SELECT DISTINCT credits` suppresses repeats and returns:

```
+---------+
| credits |
+---------+
| 3       |
| 4       |
| 2       |
+---------+
```

## 17. SQL Query Execution Order

While SQL queries are written starting with `SELECT`, the database engine processes clauses in a different logical order.

### Written Order vs Logical Execution Order

```
Written SQL Query Sequence:
1. SELECT
2. FROM
3. WHERE
4. ORDER BY
5. LIMIT
```

Фрагмент кода

```
flowchart TD
    Step1["1. FROM (Identify & load source table)"]
    Step2["2. WHERE (Filter individual rows)"]
    Step3["3. SELECT (Project requested columns)"]
    Step4["4. DISTINCT (Eliminate duplicate values)"]
    Step5["5. ORDER BY (Sort final result set)"]
    Step6["6. LIMIT (Restrict number of output rows)"]

    Step1 --> Step2 --> Step3 --> Step4 --> Step5 --> Step6
```

### Why Logical Execution Order Matters

Understanding execution sequence helps prevent common SQL errors:

SQL

```
-- THIS WILL FAIL:
SELECT title AS course_title
FROM courses
WHERE course_title = 'FastAPI Fundamentals';
```

- **Why it fails:** The `WHERE` clause is evaluated **before** `SELECT`. When the database runs `WHERE`, the alias `course_title` does not exist yet!
    

## 18. SQL vs Python CRUD Mapping

This table shows how standard backend operations map between Python data structures and SQL database queries.

|**CRUD Operation**|**Python List Approach (list[dict])**|**SQL Database Command**|
|---|---|---|
|**Create**|`courses_db.append({"id": 1, ...})`|`INSERT INTO courses (...) VALUES (...);`|
|**Read (All)**|`return courses_db`|`SELECT * FROM courses;`|
|**Read (Filter)**|`[c for c in courses_db if c["id"] == 1]`|`SELECT * FROM courses WHERE id = 1;`|
|**Update**|`courses_db[0]["title"] = "Updated"`|`UPDATE courses SET title = 'Updated' WHERE id = 1;`|
|**Delete**|`courses_db.pop(index)`|`DELETE FROM courses WHERE id = 1;`|
|**Count**|`len(courses_db)`|`SELECT COUNT(*) FROM courses;`|

## 19. Common Beginner Mistakes

### 1. The Missing `WHERE` Clause

- **Mistake:** Executing `UPDATE` or `DELETE` without scoping conditions.
    
- **Impact:** Unexpectedly modifies or deletes every record in the table.
    
- **Fix:** Draft the query using `SELECT * FROM table WHERE condition;` to verify target rows before modifying data.
    

### 2. Single Quotes vs Double Quotes

- **Mistake:** Using double quotes (`"FastAPI"`) for literal string values.
    
- **Impact:** Standard SQL treats double quotes as identifiers (like table or column names) and single quotes (`'FastAPI'`) as literal strings.
    
- **Fix:** Use single quotes `'...'` for string values: `WHERE title = 'FastAPI';`.
    

### 3. Misinterpreting `NULL`

- **Mistake:** Writing `WHERE column = NULL` or `WHERE column != NULL`.
    
- **Impact:** In SQL, `NULL` represents the complete absence of a value (unknown/missing). Comparing anything to `NULL` using `=` or `!=` evaluates to `UNKNOWN` (treated as `FALSE`).
    
- **Fix:** Always use the dedicated operators `IS NULL` or `IS NOT NULL`.
    
    SQL
    
    ```
    -- Correct Usage:
    SELECT * FROM courses WHERE description IS NULL;
    ```
    

## 20. Practical Exercises

Use the following schema and data setup to complete the exercises below:

```
Table: books
+----+----------------------------------+-------------------+---------------+-------+
| id | title                            | author            | price_cents   | stock |
+----+----------------------------------+-------------------+---------------+-------+
| 1  | Designing Data-Intensive Apps    | Martin Kleppmann  | 4500          | 12    |
| 2  | Clean Code                       | Robert Martin     | 3800          | 0     |
| 3  | The Pragmatic Programmer         | Andrew Hunt       | 4200          | 8     |
| 4  | Head First Design Patterns       | Eric Freeman      | 3500          | 15    |
| 5  | Modern Software Engineering      | Dave Farley       | 4000          | 3     |
+----+----------------------------------+-------------------+---------------+-------+
```

### Exercise 1: Basic Reading & Projection

Write a query that retrieves only the `title` and `price_cents` of all books.

SQL

```
SELECT title, price_cents
FROM books;
```

### Exercise 2: Filtering Out-of-Stock Items

Write a query returning all details for books that are currently out of stock (`stock = 0`).

SQL

```
SELECT *
FROM books
WHERE stock = 0;
```

### Exercise 3: Pattern Matching & Multiple Filters

Find the `title` and `author` of all books whose title starts with `'The'` AND have a price greater than `4000` cents.

SQL

```
SELECT title, author
FROM books
WHERE title LIKE 'The%' AND price_cents > 4000;
```

### Exercise 4: Sorting & Pagination

Retrieve the top 2 most expensive books in stock (`stock > 0`), showing `title` and `price_cents`, ordered from highest to lowest price.

SQL

```
SELECT title, price_cents
FROM books
WHERE stock > 0
ORDER BY price_cents DESC
LIMIT 2;
```

### Exercise 5: Data Mutation

Add a new book titled `'Database Internals'` by author `'Alex Petrov'`, priced at `5000` cents, with a stock count of `5`.

SQL

```
INSERT INTO books (title, author, price_cents, stock)
VALUES ('Database Internals', 'Alex Petrov', 5000, 5);
```

## 21. Interview Questions

### Q1: What is the fundamental difference between declarative and imperative programming in the context of SQL vs. Python?

**Answer:** In an imperative language like Python, you write the step-by-step logic required to fetch, loop over, and filter data. In a declarative language like SQL, you describe the criteria of the desired output set using clauses like `SELECT` and `WHERE`. The underlying database engine uses its query optimizer to handle indexing, memory management, and data retrieval automatically.

### Q2: Why is storing application state in a database preferable to using global Python data structures?

**Answer:** Global variables in Python exist only in RAM. They are lost if the application crashes or restarts, cannot scale beyond a single machine's memory, and lead to race conditions under concurrent access. Databases persist data safely to non-volatile disk storage, manage concurrent access safely, and optimize lookups over massive datasets.

### Q3: What is a Primary Key and why should it generally be non-business data (surrogate)?

**Answer:** A Primary Key is a column or group of columns that uniquely identifies each record in a table. It must be unique and non-null. Using natural business data (like names or emails) as a primary key can cause issues if those values change or are not guaranteed to be unique. Artificial surrogate keys (like auto-incrementing integers or UUIDs) provide stable, immutable references that improve data integrity and query performance.

### Q4: Explain the difference between `LIKE 'A%'` and `LIKE 'A_'`.

**Answer:** The `%` wildcard matches zero or more arbitrary characters (so `'A%'` matches `'A'`, `'Alex'`, or `'Architecture'`). The `_` wildcard matches exactly one character (so `'A_'` matches only two-character strings like `'An'`, `'At'`, or `'Ax'`).

### Q5: What happens if an `UPDATE` statement is executed without a `WHERE` clause?

**Answer:** The `UPDATE` operation will be applied unconditionally to every single row in the table, overwriting existing column values across the entire dataset.

### Q6: Why can you not use a column alias defined in `SELECT` within a `WHERE` clause?

**Answer:** Because of SQL's logical execution order: the `WHERE` clause is evaluated before the `SELECT` clause projections are processed. As a result, column aliases defined in `SELECT` do not exist yet when `WHERE` executes.

### Q7: What is the primary operational difference between `COUNT(*)` and `COUNT(column_name)`?

**Answer:** `COUNT(*)` returns the total count of all rows matching the criteria, regardless of row contents. `COUNT(column_name)` counts only the rows where the specified column value is NOT `NULL`.

### Q8: How does the `LIMIT` clause support backend API pagination design?

**Answer:** `LIMIT` caps the maximum number of rows returned in a single query execution. Backend systems use this clause to return small chunks (pages) of data per request, preventing memory exhaustion and reducing network payload size.

### Q9: Why is standard equity (`=`) invalid when querying for `NULL` values?

**Answer:** In SQL, `NULL` represents an absent or unknown value. Under three-valued logic, any comparison against `NULL` using standard comparison operators (like `= NULL` or `!= NULL`) returns an `UNKNOWN` state rather than `TRUE`. You must use `IS NULL` or `IS NOT NULL` instead.

### Q10: Why is `SELECT *` considered an anti-pattern in production backend development?

**Answer:** `SELECT *` retrieves all columns, including unnecessary or large data fields, wasting CPU time, network bandwidth, and application memory. It can also break API contracts if table structures change and prevents the database from using query optimizations like index-only scans.

## 22. Knowledge Check

Try answering these conceptual questions to evaluate your understanding of relational database principles:

1. How does a database engine guarantee that primary key values remain unique across high-concurrency environments?
    
2. If a table contains 10,000 records, how many rows will be evaluated by a `WHERE` condition if no indexing is present?
    
3. What is the fundamental conceptual difference between an empty string (`""`) and a `NULL` value in a database cell?
    
4. If you combine both `AND` and `OR` operators in a single `WHERE` clause without using parentheses, how does the engine decide execution priority?
    
5. Which query execution step runs first: `SELECT`, `WHERE`, or `FROM`?
    
6. Why are database engines generally faster at searching indexed columns than Python is at scanning lists?
    
7. In what operational scenario would using `DISTINCT` significantly increase query response latency?
    
8. What is the explicit technical difference between an `ASC` sort and a `DESC` sort when applied to dates?
    
9. How does an auto-incrementing surrogate Primary Key behave when an `INSERT` command fails or is rolled back?
    
10. Why is string comparison using `LIKE` with a leading percentage sign (`'%text'`) slower than using a trailing percentage sign (`'text%'`)?
    
11. If you run `SELECT COUNT(email) FROM users;` and 5 users out of 100 have a `NULL` email, what integer value is returned?
    
12. What distinguishes a DBMS dialect from standard ANSI/ISO SQL specifications?
    
13. Why does updating an existing record take longer than appending a row when considering disk read/write cycles?
    
14. How does setting a column to `NOT NULL` change how data must be sent in an `INSERT` statement?
    
15. Why is it bad practice to store formatted output values (like `$100.00` as text) instead of raw numerical values (`10000` integer cents) in database columns?
    
16. How does SQL handle sorting when the target column contains multiple `NULL` entries alongside values?
    
17. What breaks in an API route if a column selected via `SELECT *` is dropped from the underlying table?
    
18. What structural components form a table's schema definition?
    
19. Why should primary key values remain unchanged once assigned to a record?
    
20. Why do backend frameworks separation patterns keep raw database access queries isolated from API router files?
    

## 23. Summary

### Key Concepts Mastered

- **Why Databases Exist:** Persistent, reliable, and indexed disk storage replaces volatile in-memory Python structures like `list[dict]`.
    
- **SQL Language Standard:** SQL is a declarative language used to query relational engines like PostgreSQL, MySQL, and SQLite.
    
- **Core Table Terminology:** Databases contain **Tables** made of **Columns** (attributes) and **Rows** (records), structured by a **Schema**.
    
- **Primary Keys:** Uniquely identify each record in a table. Immutable surrogate keys (such as IDs) are generally preferred over natural keys.
    
- **Core CRUD Operations:**
    
    - **Create:** `INSERT INTO table (cols) VALUES (vals);`
        
    - **Read:** `SELECT cols FROM table WHERE cond ORDER BY col LIMIT n;`
        
    - **Update:** `UPDATE table SET col = val WHERE cond;`
        
    - **Delete:** `DELETE FROM table WHERE cond;`
        
- **Logical Execution Order:** Queries are executed internally in the order `FROM` $\rightarrow$ `WHERE` $\rightarrow$ `SELECT` $\rightarrow$ `ORDER BY` $\rightarrow$ `LIMIT`.
    

[[Git_&_Github]]
[[Database_design_&_Relational_modeling]]