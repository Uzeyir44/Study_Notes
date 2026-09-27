
## 1. Why Database Design Matters

Writing backend code with FastAPI, Pydantic, and CRUD endpoints is only half of the story. In real-world software engineering, application code changes often, but **data lives forever**. If your database schema is poorly constructed, your application code will become convoluted, bug-prone, and slow.

### Storing Data vs. Designing Data

- **Storing Data:** Simply dumping unstructured records into a persistent file or table (e.g., keeping everything in a single massive spreadsheet or JSON file).
    
- **Designing Data:** Systematically defining the shape, types, rules, boundaries, and relationships of data to reflect business rules accurately.
    

### Core Goals of Database Design

> [!note]
> 
> The primary objective of database design is to make invalid states **impossible** to represent while keeping queries fast and predictable.

1. **Data Integrity:** Ensuring data remains accurate, valid, and trustworthy throughout its lifecycle.
    
2. **Data Consistency:** Eliminating conflicting states where the same fact is recorded differently in two places.
    
3. **Data Redundancy Control:** Eliminating unnecessary duplication to minimize storage waste and prevent updates from missing duplicate entries.
    
4. **Scalability:** Ensuring query response times stay fast as table row counts grow from thousands to millions.
    
5. **Maintainability:** Structuring tables so new business features can be added without rewriting the entire database schema.
    

### Comparing Well-Designed vs. Poorly Designed Databases

Imagine an e-commerce platform recording customer orders.

#### Flawed Single-Table Approach (Unstructured Spreadsheet Style)

|**order_id**|**customer_name**|**customer_email**|**shipping_address**|**item_names**|**total_price**|
|---|---|---|---|---|---|
|`101`|Alice Smith|alice@example.com|123 Main St, NY|Laptop, Mouse|$1,250.00|
|`102`|Alice Smith|alice_new@example.com|123 Main St, NY|Keyboard|$75.00|
|`103`|Bob Jones|bob@example.com|456 Elm St, CA|Laptop|$1,200.00|

#### Consequences of the Flawed Approach

> [!warning]
> 
> **Data Anomaly Risks:**
> 
> - **Update Anomaly:** If Alice changes her email, updating row `101` but missing row `102` creates a system conflict.
>     
> - **Insertion Anomaly:** You cannot store a new customer's contact details until they place an order.
>     
> - **Deletion Anomaly:** If Bob cancels order `103` and you delete the row, you lose all record of Bob's existence in your system.
>     

#### Well-Designed Relational Approach (Separated Entities)

Фрагмент кода

```
graph LR
    Customers["Customers Table"]
    Orders["Orders Table"]
    OrderItems["Order Items Table"]
    Products["Products Table"]

    Customers -->|1 to Many| Orders
    Orders -->|1 to Many| OrderItems
    Products -->|1 to Many| OrderItems
```

- **`Customers` Table:** Manages customer profiles (`customer_id`, `name`, `email`).
    
- **`Orders` Table:** Tracks order headers (`order_id`, `customer_id`, `created_at`, `shipping_address`).
    
- **`Products` Table:** Stores catalog items (`product_id`, `name`, `price`).
    
- **`OrderItems` Table:** Connects individual ordered products to specific orders (`order_id`, `product_id`, `quantity`, `unit_price`).
    

### The Cost of Changing Schema Later

Changing application code takes minutes or hours. Changing a production database schema takes days or weeks:

1. **Downtime & Lock Risk:** Adding, deleting, or altering columns on tables with millions of rows can lock tables, causing site outages.
    
2. **Data Migration Overhead:** You must write complex scripts to transform old data into the new structure without losing historical records.
    
3. **API & Code Breaks:** Altering column names or table structures breaks existing backend code, requiring synchronized deployments across multiple services.
    

## 2. From Objects to Tables

As a backend developer building FastAPI applications, you are already comfortable working with Python classes and Pydantic models. Relational databases store data in a structure that directly mirrors object-oriented concepts.

### Conceptual Translation

```
Python World                       Database World
+--------------------+             +--------------------+
|   Class Definition |  ========>  |    Table Schema    |
|   Class Instance   |  ========>  |    Table Row       |
|   Object Attribute |  ========>  |    Table Column    |
+--------------------+             +--------------------+
```

### Code Example vs. Relational Mapping

Consider a Python class representing an academic course:

Python

```
class Course:
    id: int
    title: str
    instructor: str
```

In a relational database, this translates into a **Table** named `courses`:

|**id (Column)**|**title (Column)**|**instructor (Column)**|
|---|---|---|
|`1` _(Row 1)_|Data Structures|Dr. Aris|
|`2` _(Row 2)_|Web Development|Prof. Elena|

### Comparison Table: Python vs. Relational Database

|**Concept**|**Python / FastAPI Context**|**Relational Database Context**|
|---|---|---|
|**Structure Definition**|`class Course(BaseModel):`|`CREATE TABLE courses (...);`|
|**Individual Unit**|Object Instance (`course = Course(...)`)|Record / Row / Tuple|
|**Data Property**|Class Attribute (`course.title`)|Column / Field|
|**Unique Identifier**|Object Memory Address or ID attribute|Primary Key (`id`)|
|**Collection**|`List[Course]`|Table (`courses`)|

## 3. Entities

### What is an Entity?

An **Entity** represents a real-world or abstract concept about which your software system needs to collect and store information.

- An entity is typically a **noun**: `Student`, `Course`, `Invoice`, `Payment`.
    
- In database terminology, an **Entity Set** corresponds to a table, and a single **Entity Instance** corresponds to a row in that table.
    

### Identifying Entities in Business Requirements

> [!tip]
> 
> When reading software requirements, underline the **nouns**. Nouns representing distinct concepts that possess properties and independent lifecycles are prime candidates for database entities.

#### Business Requirement Text

> _"Our university system lets **Students** enroll in **Courses** taught by **Teachers**. Each student places an **Order** for physical **Textbooks**."_

Candidate Entities: `Student`, `Course`, `Teacher`, `Order`, `Textbook`.

### Strong vs. Weak Entities

Фрагмент кода

```
graph TD
    A[Entities] --> B[Strong Entities]
    A --> C[Weak Entities]
    B -->|Has Independent Existence| D[e.g., Customer, Product, Student]
    C -->|Depends on Strong Entity| E[e.g., OrderItem, Dependent, Address]
```

- **Strong Entity:** Exists independently of any other entity in the database.
    
    - _Example:_ `Customer`. A customer profile exists whether or not they have placed an order.
        
- **Weak Entity:** Cannot exist without a parent or owner entity. It is identified through its relationship with a strong entity.
    
    - _Example:_ `OrderItem`. An line item inside a cart cannot exist without an overarching `Order`.
        

## 4. Attributes

An **Attribute** is a property or characteristic describing an entity. Columns in a table represent attributes.

### Classification of Attributes

Фрагмент кода

```
graph LR
    Attributes --> Simple
    Attributes --> Composite
    Attributes --> Required
    Attributes --> Optional
    Attributes --> Derived
    Attributes --> MultiValued["Multi-Valued"]
```

1. **Required Attributes (`NOT NULL`):** Must contain a value for every record.
    
    - _Example:_ A user's `hashed_password` or `email`.
        
2. **Optional Attributes (`NULL`):** Allowed to be empty or unknown at creation time.
    
    - _Example:_ A user's `middle_name` or `bio`.
        
3. **Simple Attributes:** Cannot be broken down into smaller atomic sub-parts.
    
    - _Example:_ `age = 22`, `price = 19.99`.
        
4. **Composite Attributes:** Attributes composed of multiple logical sub-fields.
    
    - _Example:_ `full_name` (composed of `first_name` and `last_name`), `address` (composed of `street`, `city`, `zip_code`, `country`).
        
    - _Rule:_ Decompose composite attributes into individual simple attributes (columns) inside your table.
        
5. **Derived Attributes:** Attributes calculated from other existing stored values.
    
    - _Example:_ `age` calculated from `date_of_birth`, or `total_price` calculated from `unit_price * quantity`.
        
    - _Rule:_ Do **not** store derived attributes in tables unless performance requires it. Calculate them on-the-fly in Python/SQL.
        
6. **Multi-Valued Attributes:** Attributes that hold multiple values for a single entity instance.
    
    - _Example:_ A student having multiple `phone_numbers` or a blog post having multiple `tags`.
        
    - _Rule:_ Never store multiple values inside a single column (e.g., `"050-123, 055-456"`). Move multi-valued attributes into a separate child table.
        

## 5. Data Types

Selecting correct data types ensures data accuracy, optimizes disk and memory storage, and prevents application bugs.

### Common SQL Data Types Overview

|**Standard SQL Data Type**|**Python Equivalent**|**Typical Storage Size**|**Ideal Use Case**|
|---|---|---|---|
|**`INTEGER` / `INT`**|`int`|4 Bytes|Standard auto-incrementing IDs, counts.|
|**`BIGINT`**|`int`|8 Bytes|High-volume primary keys (millions+ rows), financial sub-units.|
|**`VARCHAR(N)`**|`str`|Variable (up to $N$ chars)|Short bounded strings: names, emails, titles.|
|**`TEXT`**|`str`|Variable (large)|Unbounded long text: article bodies, reviews, JSON logs.|
|**`BOOLEAN`**|`bool`|1 Byte|True/False flags: `is_active`, `email_verified`.|
|**`DATE`**|`datetime.date`|4 Bytes|Calendar dates without time: `date_of_birth`, `hire_date`.|
|**`TIMESTAMP`**|`datetime.datetime`|8 Bytes|Specific points in time: `created_at`, `updated_at`.|
|**`DECIMAL(P, S)`**|`Decimal`|Variable|Exact numeric accuracy: prices, currency calculations.|
|**`UUID`**|`uuid.UUID`|16 Bytes|Globally unique identifiers across distributed systems.|

> [!important]
> 
> **Never use floating-point types (`FLOAT`, `DOUBLE`) for financial data.** Floats introduce binary rounding errors (e.g., $0.1 + 0.2 = 0.30000000000000004$). Always use `DECIMAL` or `NUMERIC` for monetary values.

## 6. Constraints

Constraints enforce business rules at the database engine level, acting as a safeguard regardless of bugs in backend application code.

Фрагмент кода

```
graph TD
    Sub[Database Engine] --> PK[PRIMARY KEY: Uniquely identifies row]
    Sub --> FK[FOREIGN KEY: Ensures valid relationship]
    Sub --> NN[NOT NULL: Prevents empty values]
    Sub --> UNQ[UNIQUE: Prevents duplicate entries]
    Sub --> CHK[CHECK: Validates value expressions]
    Sub --> DEF[DEFAULT: Fallback when omitted]
```

### Detailed Breakdown of Constraints

1. **`PRIMARY KEY`:** Uniquely identifies every record in a table. Implies `UNIQUE` and `NOT NULL`.
    
2. **`FOREIGN KEY`:** Ensures referential integrity by forcing values in one table to match an existing primary key in another table.
    
3. **`NOT NULL`:** Prevents missing values from being inserted into essential columns.
    
4. **`UNIQUE`:** Guarantees that all values in a specified column are distinct across all rows.
    
5. **`CHECK`:** Validates that data inserted into a column satisfies a specified logical boolean expression.
    
6. **`DEFAULT`:** Assigns an automatic fallback value if no value is explicitly supplied during insertion.
    

### Illustrative SQL Code Example

> [!note]
> 
> The following SQL code is provided for conceptual clarity to demonstrate how constraints are declared in standard DDL.

SQL

```
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    age INTEGER CHECK (age >= 18),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Line-by-Line Line Explanation

- `CREATE TABLE users (`: Tells the database management system to define a new table structure named `users`.
    
- `id INTEGER PRIMARY KEY,`: Defines `id` as an integer column that uniquely identifies each user record.
    
- `username VARCHAR(50) NOT NULL UNIQUE,`: Defines `username` as a text field (max 50 characters) that cannot be empty (`NOT NULL`) and cannot duplicate existing values (`UNIQUE`).
    
- `email VARCHAR(255) NOT NULL UNIQUE,`: Defines `email` as a mandatory, unique text field with a maximum length of 255 characters.
    
- `age INTEGER CHECK (age >= 18),`: Defines `age` as an integer column, enforcing a rule (`CHECK`) that blocks any record where age is under 18.
    
- `status VARCHAR(20) DEFAULT 'pending',`: Defines `status` as a short string; if a new row omits this value, it defaults automatically to `'pending'`.
    
- `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP`: Tracks record creation time automatically using the system timestamp.
    
- `);`: Closes the table creation block.
    

## 7. Keys

Keys establish unique identification for records and enforce connections between separate tables.

### Types of Keys

Фрагмент кода

```
graph TD
    Keys[Table Keys]
    Keys --> PK[Primary Key]
    Keys --> FK[Foreign Key]
    Keys --> CK[Candidate Keys]
    CK --> Natural[Natural Key]
    CK --> Surrogate[Surrogate Key]
    CK --> Composite[Composite Key]
```

1. **Primary Key (PK):** The single column (or group of columns) chosen to uniquely identify every row in a table.
    
2. **Candidate Key:** Any column or set of columns capable of uniquely identifying a record.
    
3. **Alternate Key:** Candidate keys that were not chosen as the Primary Key.
    
4. **Composite Key:** A primary key made up of two or more combined columns (frequently used in junction tables).
    
5. **Natural Key:** A key derived from existing real-world attributes with inherent meaning (e.g., Social Security Number, Passport Number, Vehicle VIN).
    
6. **Surrogate Key:** An artificially created system key with no real-world business meaning (e.g., auto-incrementing `id` integer, auto-generated `UUID`).
    

### Comparison: Natural vs. Surrogate Keys

|**Feature**|**Natural Keys (e.g., Passport Number)**|**Surrogate Keys (e.g., Auto-increment ID / UUID)**|
|---|---|---|
|**Business Meaning**|Has real-world significance.|Completely arbitrary and system-controlled.|
|**Immutability**|Risk of changing if real-world standards change.|100% immutable throughout application life.|
|**Storage Efficiency**|Often large text fields (`VARCHAR`).|Compact fixed-length types (`INTEGER`, `BIGINT`, `UUID`).|
|**Performance**|Slower indexing and joining operations.|Highly optimized index search and fast join performance.|

> [!tip]
> 
> **Best Practice:** Use **Surrogate Keys** (`id INT` or `UUID`) as primary keys for almost every entity. If you must support real-world unique identifiers (like an email or barcode), enforce them using a `UNIQUE` constraint on a separate column.

## 8. Relationships

Entities in a business domain rarely exist in isolation. Relationships describe how data in one entity relates to data in another.

### Relationship Types Summary

```
[1-to-1]    User (1)  ==================== (1) Profile
[1-to-Many] Author (1) <=================== (*) Books
[Many-to-Many] Student (*) <=============> (*) Courses
```

### 1. One-to-One (1:1)

Each row in Table A relates to **at most one** row in Table B, and vice versa.

- **Example:** `Users` ↔ `Profiles`. A user has one profile, and a profile belongs to one user.
    
- **Implementation:** Place the foreign key on either table (usually the dependent table), and enforce a `UNIQUE` constraint on the foreign key column.
    

Фрагмент кода

```
erDiagram
    USERS ||--o| PROFILES : "has"
    USERS {
        int id PK
        string email
    }
    PROFILES {
        int id PK
        int user_id FK, UNIQUE
        string bio
    }
```

### 2. One-to-Many (1:N)

A single row in Table A can relate to **multiple** rows in Table B, but a row in Table B relates to **only one** row in Table A.

- **Example:** `Authors` ↔ `Books`. An author can write many books, but each book is attributed to one primary author.
    
- **Example:** `Customers` ↔ `Orders`. A customer can place multiple orders, but each order belongs to one specific customer.
    
- **Implementation:** Place the Foreign Key (`author_id` or `customer_id`) inside the **"Many"** side table.
    

Фрагмент кода

```
erDiagram
    CUSTOMERS ||--o{ ORDERS : "places"
    CUSTOMERS {
        int id PK
        string name
    }
    ORDERS {
        int id PK
        int customer_id FK
        timestamp order_date
    }
```

### 3. Many-to-Many (N:M)

A single row in Table A can relate to **multiple** rows in Table B, and a single row in Table B can relate to **multiple** rows in Table A.

- **Example:** `Students` ↔ `Courses`. A student can enroll in multiple courses, and a course contains multiple enrolled students.
    
- **Example:** `Orders` ↔ `Products`. An order can contain multiple products, and a product can appear in multiple orders.
    
- **Implementation:** Relational databases **cannot directly store N:M relationships**. They must be decomposed into two 1:N relationships using a **Junction Table**.
    

## 9. Junction Tables

Because relational databases do not support storing lists or arrays inside a column, a direct Many-to-Many relationship breaks down.

### Resolving N:M via a Junction Table

Фрагмент кода

```
graph LR
    Students["Students Table"] --- Enr["Enrollments (Junction Table)"]
    Enr --- Courses["Courses Table"]
```

The **Junction Table** (also called an _Association Table_ or _Bridge Table_) converts one Many-to-Many relationship into two clean One-to-Many relationships.

### Adding Contextual Attributes to Junction Tables

Junction tables do more than connect keys; they hold relationship-specific data.

- _Where does a student's `grade` or `enrollment_date` belong?_
    
    - It does not belong in `Students` (a student has many grades across different courses).
        
    - It does not belong in `Courses` (a course has many grades across different students).
        
    - It belongs directly on the link between them: inside the **`Enrollments`** junction table.
        

Фрагмент кода

```
erDiagram
    STUDENTS ||--o{ ENROLLMENTS : "makes"
    COURSES ||--o{ ENROLLMENTS : "has"

    STUDENTS {
        int id PK
        string name
    }

    ENROLLMENTS {
        int student_id FK
        int course_id FK
        date enrolled_at
        string grade
    }

    COURSES {
        int id PK
        string title
    }
```

## 10. Entity Relationship Diagrams (ERDs)

Entity Relationship Diagrams (ERDs) provide a visual representation of tables, attributes, primary/foreign keys, and cardinalities.

### Reading Crow's Foot Notation

|**Symbol**|**Name**|**Meaning**|
|---|---|---|
|`||`|
|`|o`|Zero or One|
|`}|`|One or More|
|`}o`|Zero or More|Optional connection to many items.|

### Complete Conceptual ERD Example

Фрагмент кода

```
erDiagram
    USERS ||--o| USER_PROFILES : "has"
    CATEGORIES ||--o{ PRODUCTS : "categorizes"
    CUSTOMERS ||--o{ ORDERS : "places"
    ORDERS ||--|{ ORDER_ITEMS : "contains"
    PRODUCTS ||--|{ ORDER_ITEMS : "included_in"

    CUSTOMERS {
        int id PK
        string name
        string email
    }

    ORDERS {
        int id PK
        int customer_id FK
        timestamp order_date
    }

    ORDER_ITEMS {
        int order_id PK, FK
        int product_id PK, FK
        int quantity
        decimal unit_price
    }

    PRODUCTS {
        int id PK
        int category_id FK
        string title
        decimal price
    }
```

## 11. Database Normalization

**Normalization** is a step-by-step technique for organizing attributes and tables to eliminate duplicate data, minimize redundancies, and prevent data modification anomalies.

```
Unnormalized Schema =====> 1NF =====> 2NF =====> 3NF
```

### First Normal Form (1NF): Atomic Values & No Repeating Groups

To satisfy 1NF:

1. Every column value must be **atomic** (indivisible single values).
    
2. There are no repeating groups or comma-separated lists stored in a single column.
    
3. Every table must have a Primary Key defined.
    

#### Un-normalized Table (Violates 1NF)

|**student_id**|**name**|**phone_numbers**|**courses_enrolled**|
|---|---|---|---|
|`1`|Sarah|555-0100, 555-0199|Math, Physics, CS|

_Violation:_ `phone_numbers` and `courses_enrolled` contain multi-valued lists.

#### Refactored to 1NF

|**student_id**|**name**|**phone_number**|**course_name**|
|---|---|---|---|
|`1`|Sarah|555-0100|Math|
|`1`|Sarah|555-0100|Physics|
|`1`|Sarah|555-0100|CS|
|`1`|Sarah|555-0199|Math|

Now values are atomic, but we have high data redundancy! We proceed to 2NF.

### Second Normal Form (2NF): Full Functional Dependency

To satisfy 2NF:

1. The table must already be in **1NF**.
    
2. All non-key attributes must depend on the **entire** Primary Key (applies to tables with Composite Primary Keys). No partial dependencies allowed.
    

#### Table in 1NF with Composite Key (Violates 2NF)

_Composite Key:_ (`student_id`, `course_id`)

|**student_id (PK)**|**course_id (PK)**|**student_name**|**course_building**|**grade**|
|---|---|---|---|---|
|`1`|`101`|Sarah|Science Hall|A|
|`1`|`102`|Sarah|Engineering Center|B|

_Violation:_ `student_name` depends **only** on `student_id` (a part of the composite key). `course_building` depends **only** on `course_id`. They do not depend on the combination of both!

#### Refactored to 2NF (Split into Separate Tables)

**`Students` Table**

|**student_id (PK)**|**student_name**|
|---|---|
|`1`|Sarah|

**`Courses` Table**

|**course_id (PK)**|**course_building**|
|---|---|
|`101`|Science Hall|
|`102`|Engineering Center|

**`Enrollments` Table**

|**student_id (PK, FK)**|**course_id (PK, FK)**|**grade**|
|---|---|---|
|`1`|`101`|A|
|`1`|`102`|B|

### Third Normal Form (3NF): Eliminate Transitive Dependencies

To satisfy 3NF:

1. The table must already be in **2NF**.
    
2. No non-key attribute can depend on another non-key attribute (No **Transitive Dependencies**).
    
3. Rule of thumb: _"Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key."_
    

#### Table in 2NF (Violates 3NF)

|**zip_code_id (PK)**|**city**|**state**|**zip_code**|
|---|---|---|---|
|`10`|Austin|TX|78701|

Or consider a `Students` table:

|**student_id (PK)**|**name**|**advisor_id**|**advisor_email**|
|---|---|---|---|
|`1`|Alex|`50`|dr_smith@univ.edu|

_Violation:_ `advisor_email` depends on `advisor_id` (a non-key attribute), which in turn depends on `student_id`.

#### Refactored to 3NF

**`Advisors` Table**

|**id (PK)**|**advisor_email**|
|---|---|
|`50`|dr_smith@univ.edu|

**`Students` Table**

|**id (PK)**|**name**|**advisor_id (FK)**|
|---|---|---|
|`1`|Alex|`50`|

## 12. Denormalization (Introduction)

While normalization optimizes for data integrity and eliminates redundancy, read-heavy enterprise applications sometimes intentionally break normalization rules for performance optimization.

> [!important]
> 
> **Denormalization** is the deliberate process of adding redundant data or grouping data into fewer tables to speed up read queries by reducing expensive SQL `JOIN` operations.

### Normalization vs. Denormalization Trade-off

Фрагмент кода

```
graph LR
    A[Normalized Schema] -->|Pros: Max Integrity, Small Writes| B(Optimized Writes)
    A -->|Cons: Needs Complex JOINs| C(Slower Complex Reads)
    
    D[Denormalized Schema] -->|Pros: Fast Single-Table Reads| E(Optimized Reads)
    D -->|Cons: Data Redundancy, Complex Writes| F(Slower Riskier Writes)
```

- **When to Denormalize:** High-volume analytics dashboards, read-heavy caching tables, historical invoice snapshots.
    
- **Golden Rule for Beginners:** **Normalize first.** Optimize and selectively denormalize only when measured performance data proves that standard query joins are too slow.
    

## 13. Common Database Design Mistakes

Avoid these frequent pitfalls when structuring database schemas:

1. **Comma-Separated Values inside Columns:**
    
    - _Bad:_ `tags = "python,fastapi,sql"`
        
    - _Consequence:_ Impossible to efficiently index, search, sort, or enforce integrity on individual tags.
        
2. **Duplicate Data Across Multiple Tables:**
    
    - _Bad:_ Storing full user address info on both `Users` and `Orders` tables.
        
    - _Consequence:_ High risk of update anomalies and mismatched data across tables.
        
3. **Missing Primary Keys:**
    
    - _Consequence:_ Inability to target, update, or delete single target rows safely.
        
4. **Using Real-World Names as Primary Keys:**
    
    - _Bad:_ Using `first_name + last_name` as a key.
        
    - _Consequence:_ Names change, duplicate names exist, and string keys create slow database index operations.
        
5. **Storing Unnecessary Calculated/Derived Values:**
    
    - _Bad:_ Storing `age` instead of calculating it dynamically from `date_of_birth`.
        
    - _Consequence:_ `age` becomes incorrect every day unless background scripts constantly update records.
        
6. **Incorrect Data Types:**
    
    - _Bad:_ Storing timestamps as `VARCHAR("2026-08-04")` or monetary prices as `FLOAT`.
        
    - _Consequence:_ Rounding errors on financial math, broken sorting operations.
        
7. **Missing Foreign Key Constraints:**
    
    - _Bad:_ Storing parent ID columns without formal `FOREIGN KEY` references.
        
    - _Consequence:_ "Orphaned rows" remain in child tables when a parent row is deleted.
        
8. **Overusing Nullable Columns:**
    
    - _Bad:_ Creating a 50-column table where 40 columns are set to `NULL` for most rows.
        
    - _Consequence:_ Signifies poor entity segregation; table should be split into smaller related entities.
        
9. **Inconsistent Naming Conventions:**
    
    - _Bad:_ Mixing `user_id`, `tblUsersId`, `usr_key`, `ID` across different tables.
        
    - _Consequence:_ High cognitive load and frequent developer coding errors.
        

## 14. Designing a Real System: University Management System

Let's design a relational schema for a **University Management System** from scratch.

### Step 1: Identify Business Requirements

1. Students enroll in the university and are assigned a primary major Department.
    
2. Departments employ Professors. A professor heads exactly one department.
    
3. Professors teach Courses.
    
4. Students can register for multiple Courses per semester.
    
5. Track grades and registration dates for each student enrollment.
    

### Step 2: Define Entities & Core Attributes

- **`Departments`**: `id`, `name`, `code`
    
- **`Professors`**: `id`, `first_name`, `last_name`, `email`, `department_id`
    
- **`Students`**: `id`, `first_name`, `last_name`, `email`, `department_id`
    
- **`Courses`**: `id`, `title`, `code`, `credits`, `professor_id`
    
- **`Enrollments`** (Junction): `student_id`, `course_id`, `enrolled_at`, `grade`
    

### Step 3: Complete Entity Relationship Diagram (ERD)

Фрагмент кода

```
erDiagram
    DEPARTMENTS ||--o{ STUDENTS : "belongs_to"
    DEPARTMENTS ||--o{ PROFESSORS : "employs"
    PROFESSORS ||--o{ COURSES : "teaches"
    STUDENTS ||--o{ ENROLLMENTS : "registers"
    COURSES ||--o{ ENROLLMENTS : "includes"

    DEPARTMENTS {
        int id PK
        string name UNIQUE
        string code UNIQUE
    }

    PROFESSORS {
        int id PK
        string email UNIQUE
        int department_id FK
    }

    STUDENTS {
        int id PK
        string email UNIQUE
        int department_id FK
    }

    COURSES {
        int id PK
        string code UNIQUE
        int professor_id FK
    }

    ENROLLMENTS {
        int student_id PK, FK
        int course_id PK, FK
        date enrolled_at
        string grade
    }
```

## 15. Designing an E-Commerce Database

Let's construct a complete relational structure for an **E-Commerce Application**.

### Critical E-Commerce Design Principle: Price Preservation

> [!warning]
> 
> **Important E-Commerce Rule:**
> 
> Product catalog prices change over time (e.g., a product price increases from $10 to $15).
> 
> An historical `Order` placed when the item cost $10 **must never** automatically update its cost to $15!
> 
> Therefore, line-item price snapshots must be copied directly into `OrderItems.unit_price` upon purchase creation.

### Database Schema Design

Фрагмент кода

```
erDiagram
    CATEGORIES ||--o{ PRODUCTS : "contains"
    CUSTOMERS ||--o{ ORDERS : "places"
    ORDERS ||--|{ ORDER_ITEMS : "contains"
    PRODUCTS ||--|{ ORDER_ITEMS : "referenced_in"
    ORDERS ||--o| PAYMENTS : "settled_by"

    CATEGORIES {
        int id PK
        string name
    }

    PRODUCTS {
        int id PK
        int category_id FK
        string name
        decimal current_price
    }

    CUSTOMERS {
        int id PK
        string email UNIQUE
        string full_name
    }

    ORDERS {
        int id PK
        int customer_id FK
        timestamp order_date
        string status
    }

    ORDER_ITEMS {
        int order_id PK, FK
        int product_id PK, FK
        int quantity
        decimal unit_price
    }

    PAYMENTS {
        int id PK
        int order_id FK, UNIQUE
        decimal amount
        string status
        timestamp payment_date
    }
```

## 16. Applying Concepts to FastAPI

Now let's trace how relational modeling maps directly to your journey building **FastAPI** applications.

### The Full Stack Architecture Mapping

```
[ Pydantic Schema ] <---> [ FastAPI Endpoint ] <---> [ DB Layer / SQL ] <---> [ Relational Table ]
```

### From Python Pydantic Model to Relational Schema

When building CRUD endpoints in FastAPI, you create input and output Pydantic classes:

Python

```
# Pydantic Request Validation Model
from pydantic import BaseModel, EmailStr
from typing import Optional

class UserCreate(BaseModel):
    username: str
    email: EmailStr
    age: Optional[int] = None
```

This Python/Pydantic validation contract maps directly to our relational table schema:

SQL

```
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    age INTEGER CHECK (age >= 18)
);
```

### Transitioning from In-Memory Lists to Relational Persistence

In introductory FastAPI lessons, backend state is often stored in temporary global Python lists:

Python

```
# Temporary In-Memory Storage (Flawed)
db_users = []

@app.post("/users")
def create_user(user: UserCreate):
    db_users.append(user.dict())
    return user
```

With relational database integration:

1. **Pydantic** validates incoming JSON requests from Postman or client browsers.
    
2. The endpoint converts validated data into a SQL query or **SQLAlchemy ORM** model.
    
3. The database engine enforces schema rules (`NOT NULL`, `UNIQUE`, `FOREIGN KEY`).
    
4. Data is stored safely on persistent storage and survives server reboots.
    

## 17. Thinking Like a Database Designer

Follow this systematic step-by-step checklist whenever you are handed software requirements for a new feature or application:

Фрагмент кода

```
flowchart TD
    Step1[1. Identify Business Entities] --> Step2[2. Define Entity Attributes]
    Step2 --> Step3[3. Select Primary Keys]
    Step3 --> Step4[4. Map Entity Relationships]
    Step4 --> Step5[5. Determine Cardinalities]
    Step5 --> Step6[6. Normalize Schema to 3NF]
    Step6 --> Step7[7. Apply Constraints & Types]
    Step7 --> Step8[8. Review for Integrity & Performance]
```

### The 8-Step Blueprint

1. **Identify Entities:** Group real-world nouns into logical collections (`User`, `Product`).
    
2. **Define Attributes:** List necessary fields describing each entity. Eliminate composite or multi-valued fields.
    
3. **Select Primary Keys:** Assign an immutable surrogate key (`id INT` or `UUID`) to every entity table.
    
4. **Map Relationships:** Identify which entities interact with each other.
    
5. **Determine Cardinalities:** Classify connections as 1:1, 1:N, or N:M. Introduce junction tables for all N:M connections.
    
6. **Apply Normalization:** Verify 1NF, 2NF, and 3NF compliance to remove unnecessary redundancy and dependencies.
    
7. **Add Constraints:** Layer database-level protections (`NOT NULL`, `UNIQUE`, `CHECK`, `FOREIGN KEY`).
    
8. **Final Integrity Review:** Walk through common write operations (INSERT, UPDATE, DELETE) to verify that data cannot enter an invalid state.
    

## 18. Common Interview Questions

### Q1: What is data integrity, and how do databases enforce it?

**Answer:** Data integrity refers to the accuracy, consistency, and reliability of data across its lifecycle. Databases enforce it using constraints: Entity Integrity (`PRIMARY KEY`), Referential Integrity (`FOREIGN KEY`), Domain Integrity (`CHECK`, Data Types, `NOT NULL`), and User-Defined Integrity (`UNIQUE`, `DEFAULT`).

### Q2: What is the difference between a Primary Key and a Unique Key?

**Answer:** A `PRIMARY KEY` uniquely identifies every row in a table, automatically enforces `NOT NULL`, and a table can only have **one** primary key. A `UNIQUE` constraint guarantees all non-null values in a column are distinct, and a table can have **multiple** unique constraints. In many database systems, unique constraints allow a null value unless explicitly marked `NOT NULL`.

### Q3: Why should we avoid storing comma-separated values in a database column?

**Answer:** Storing lists as comma-separated text violates First Normal Form (1NF). It breaks data atomicity, makes foreign key referencing impossible, prevents effective indexing, and requires complex and slow text-parsing operations for simple queries or filter operations.

### Q4: Explain the difference between 2NF and 3NF.

**Answer:** 2NF eliminates **partial dependencies** (where a non-key column depends on only part of a composite primary key). 3NF eliminates **transitive dependencies** (where a non-key column depends on another non-key column).

### Q5: What is a Surrogate Key, and why is it preferred over a Natural Key?

**Answer:** A surrogate key is an artificially created unique identifier (like an auto-incrementing integer or UUID) with no business meaning. It is preferred because real-world natural keys (like emails, phone numbers, or passport numbers) can change, are expensive to index, and increase join complexity when propagated as foreign keys.

### Q6: What is a Foreign Key and what happens when referential integrity is violated?

**Answer:** A Foreign Key is a column or set of columns in a child table that references a Primary Key in a parent table. It enforces referential integrity by rejecting any write operation (`INSERT` or `UPDATE`) in the child table that refers to a non-existent parent key, and blocking deletions of parent records that still have associated child records.

### Q7: What is a Junction Table and when must you use one?

**Answer:** A junction table is an intermediate table containing primary keys from two different tables. You must use one to resolve a Many-to-Many (N:M) relationship into two clean One-to-Many (1:N) relationships.

### Q8: What is the difference between `VARCHAR` and `TEXT` data types?

**Answer:** `VARCHAR(N)` stores variable-length character strings up to a specified maximum length limit ($N$), which allows for optimized query memory allocations and input length validations. `TEXT` stores unbounded, large strings suitable for arbitrary long-form content.

### Q9: Why shouldn't you use `FLOAT` for storing monetary currency?

**Answer:** `FLOAT` uses binary floating-point representation, which cannot represent base-10 decimals exactly. This introduces silent cumulative rounding errors. Monetary systems must use exact precision numeric types like `DECIMAL` or `NUMERIC`.

### Q10: What is a Composite Key? Give a practical example.

**Answer:** A composite key is a primary key that consists of two or more combined columns to uniquely identify a row. A classic example is inside a `Students_Courses` junction table, where the combination of `(student_id, course_id)` serves as the composite key to prevent duplicate enrollments.

### Q11: What is the difference between a Strong Entity and a Weak Entity?

**Answer:** A Strong Entity can exist independently in the database (e.g., `Customer`). A Weak Entity depends on a strong entity for its existence and identification (e.g., `OrderItem`, which cannot exist without an `Order`).

### Q12: What does "Cascading Deletes" mean?

**Answer:** Cascading Delete (`ON DELETE CASCADE`) is an automated foreign key rule. When a record in a parent table is deleted, the database automatically deletes all associated child records in dependent tables, preventing orphaned records.

### Q13: What is the difference between Simple and Composite attributes?

**Answer:** A simple attribute is atomic and cannot be split further (e.g., `age`). A composite attribute consists of multiple logical sub-components that should be split into individual columns (e.g., `address` into `street`, `city`, `zip_code`).

### Q14: What is a Derived Attribute? Should it be stored in a table?

**Answer:** A derived attribute is a value calculated from other existing data (e.g., `age` from `date_of_birth`). Generally, derived attributes should **not** be stored in tables to prevent data staleness and synchronization bugs; they should be computed dynamically via queries or application logic.

### Q15: What is Denormalization and when is it appropriate?

**Answer:** Denormalization is the deliberate re-introduction of redundancy or combining normalized tables to optimize read performance and reduce complex JOIN operations in high-volume, read-heavy applications.

### Q16: What is a Transitive Dependency?

**Answer:** A transitive dependency occurs when an attribute depends on a non-key attribute, which in turn depends on the primary key ($A \rightarrow B \rightarrow C$). Eliminating transitive dependencies is the primary goal of 3NF.

### Q17: What is the purpose of a `CHECK` constraint?

**Answer:** A `CHECK` constraint evaluates a logical expression before allowing a row insertion or update. If the expression evaluates to false (e.g., `price > 0`), the database rejects the transaction.

### Q18: What is a UUID and what are its advantages over auto-incrementing integers?

**Answer:** A UUID (Universally Unique Identifier) is a 128-bit globally unique identifier. Advantages include: safe key generation on client/distributed nodes without central database checks, and obfuscation of total row counts (preventing competitors from enumerating resource IDs like `/orders/1`, `/orders/2`).

### Q19: How do you choose between a 1:1 relationship and merging attributes into a single table?

**Answer:** Attributes should be kept in a single table if they belong to the same core entity domain and are frequently queried together. Move attributes into a separate 1:1 table if: the data is optional/infrequently accessed (e.g., large blob metadata), has distinct security/permission constraints, or belongs to a separate logical entity.

### Q20: What is an insertion anomaly?

**Answer:** An insertion anomaly occurs in poorly normalized tables when you cannot record details about one entity without forcibly entering incomplete or unrelated details about another entity (e.g., being unable to create a `Course` record because no `Student` has enrolled in it yet).

## 19. Knowledge Check

Use these conceptual questions to evaluate your understanding.

1. If you are designing a blog database where posts can have multiple tags, why is creating columns `tag1`, `tag2`, and `tag3` inside the `Posts` table a flaw?
    
2. How does a Foreign Key constraint prevent "orphaned" records?
    
3. Identify the primary entity violation when storing a customer's current age directly in a user profile table.
    
4. Explain why a table with a single-column Primary Key is automatically compliant with 2NF if it is already in 1NF.
    
5. In an e-commerce platform, why must the price of an item in the `OrderItems` table be independent of the `price` column in the `Products` catalog table?
    
6. Describe a real-world scenario where a 1:1 relationship is preferred over combining fields into one table.
    
7. What database anomaly occurs if a customer updates their address, but only 1 of their 5 historical order rows gets updated?
    
8. Why is an SSN (Social Security Number) generally considered a risky choice for a Primary Key, despite being unique per person?
    
9. Explain how a junction table converts a Many-to-Many relationship into two One-to-Many relationships.
    
10. Identify which normal form is violated if a table contains non-key columns: `author_id`, `author_name`, and `author_bio`.
    
11. What rule dictates whether an attribute should be marked `NOT NULL` versus `NULL`?
    
12. Why does choosing `BIGINT` over `INTEGER` matter when designing primary keys for high-frequency event tracking tables?
    
13. Describe the difference between cardinalities represented by Crow's foot notation `||` and `}o`.
    
14. Under what condition can a Candidate Key become an Alternate Key?
    
15. What structural problem arises if you store raw unformatted JSON strings containing core business data inside a relational text column?
    
16. How do database constraints complement Pydantic input validation in a FastAPI backend service?
    
17. In a social media application, how would you design a relationship where users can follow other users (Self-Referential Many-to-Many)?
    
18. Why are surrogate integer primary keys faster for JOIN operations than long string natural keys?
    
19. What is the fundamental risk associated with excessive denormalization?
    
20. Why should table names generally be plural (e.g., `users`, `products`) while Python backend classes are singular (`User`, `Product`)?
    

## 20. Practical Exercises

### Exercise 1: Identify Entities and Attributes

**Scenario:** "We are building a Digital Library application. Readers can register and borrow Books. Books are written by Authors. Each book can have multiple physical Copies available in different branches."

- **Task:** List the core entities, assign 3 key attributes to each, and mark potential primary keys.
    

> [!tip]
> 
> **Expected Solution:**
> 
> - `Readers`: `id` (PK), `name`, `email`
>     
> - `Authors`: `id` (PK), `name`, `bio`
>     
> - `Books`: `id` (PK), `author_id` (FK), `title`, `isbn`
>     
> - `Branches`: `id` (PK), `name`, `address`
>     
> - `BookCopies`: `id` (PK), `book_id` (FK), `branch_id` (FK), `status`
>     
> - `Loans`: `id` (PK), `reader_id` (FK), `copy_id` (FK), `borrowed_at`, `returned_at`
>     

### Exercise 2: Fix a Flawed Schema (Normalization Practice)

Given the following flawed table structure:

**`StudentGrades_Flawed`**

|**student_id**|**student_name**|**student_email**|**course_code**|**course_title**|**instructor_name**|**grade**|
|---|---|---|---|---|---|---|
|`1`|Alice|alice@univ.edu|CS101|Intro to CS|Dr. Smith|A|
|`1`|Alice|alice@univ.edu|MATH201|Calculus|Dr. Jones|B|

- **Task:** Identify 1NF, 2NF, and 3NF violations and refactor into clean, fully normalized tables.
    

> [!tip]
> 
> **Expected Solution:**
> 
> Split into 4 entities:
> 
> 1. **`Students`**: `id` (PK), `name`, `email`
>     
> 2. **`Instructors`**: `id` (PK), `name`
>     
> 3. **`Courses`**: `id` (PK), `code`, `title`, `instructor_id` (FK)
>     
> 4. **`Enrollments`**: `student_id` (FK), `course_id` (FK), `grade` -> Primary Key: (`student_id`, `course_id`)
>     

### Exercise 3: Draw an ERD for a Social Media System

**Scenario:**

- `Users` can post multiple `Posts`.
    
- `Posts` can have multiple `Comments`.
    
- `Users` can like multiple `Posts` (and posts can be liked by multiple users).
    

- **Task:** Write a Mermaid ERD illustrating tables, relationships, and cardinalities.
    

> [!tip]
> 
> **Expected Solution:**

Фрагмент кода

```
erDiagram
    USERS ||--o{ POSTS : "creates"
    POSTS ||--o{ COMMENTS : "has"
    USERS ||--o{ COMMENTS : "writes"
    USERS ||--o{ POST_LIKES : "likes"
    POSTS ||--o{ POST_LIKES : "liked_by"

    USERS {
        int id PK
        string username UNIQUE
    }

    POSTS {
        int id PK
        int user_id FK
        text content
    }

    COMMENTS {
        int id PK
        int post_id FK
        int user_id FK
        text content
    }

    POST_LIKES {
        int user_id PK, FK
        int post_id PK, FK
        timestamp liked_at
    }
```

## 21. Summary

### Core Database Design Checklist

> [!important]
> 
> Before implementing any table in PostgreSQL or SQLAlchemy, run through this final checklist:
> 
> - [ ] Every table has a distinct Surrogate Primary Key (`id` or `uuid`).
>     
> - [ ] All foreign keys are formally linked with `FOREIGN KEY` constraints.
>     
> - [ ] No table contains repeating groups or comma-separated list strings.
>     
> - [ ] Every non-key attribute depends entirely on the primary key (3NF).
>     
> - [ ] Monetary values use exact precision `DECIMAL`/`NUMERIC`, never `FLOAT`.
>     
> - [ ] Prices inside historical transactional tables (`OrderItems`) are copied snapshots.
>     
> - [ ] Mandatory attributes are marked `NOT NULL`.
>     
> - [ ] Unique fields (emails, codes, usernames) have `UNIQUE` constraints.
>     

### Key Concept Recap

|**Topic**|**Key Principle**|
|---|---|
|**Entities**|Nouns representing real-world business domains mapped directly to database tables.|
|**Attributes**|Columns defining characteristics; composite and multi-valued fields must be split.|
|**Data Types**|Choose precise types to enforce validity and save storage (`DECIMAL` for money).|
|**Constraints**|Fail-safe engine rules enforcing valid data states (`NOT NULL`, `UNIQUE`, `CHECK`).|
|**Keys**|PKs identify rows; FKs maintain referential integrity across linked tables.|
|**Relationships**|1:1, 1:N, and N:M (N:M always requires a dedicated Junction Table).|
|**Normalization**|Systematic reduction of data redundancy and elimination of modification anomalies.|

[[SQL_Fundamentals]]
[[PostgreSQL_Fundamentals]]