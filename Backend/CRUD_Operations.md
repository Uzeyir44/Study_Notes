
## 1. What is CRUD?

At its core, almost every web application, mobile app, or enterprise system is built around managing resources. Whether you are scrolling through a social media feed, adding items to a shopping cart, or updating your profile, you are interacting with a backend system that performs **CRUD** operations.

**CRUD** is an acronym for four fundamental operations:

- **C**reate: Adding new data to the system.
    
- **R**ead: Retrieving existing data from the system.
    
- **U**pdate: Modifying existing data within the system.
    
- **D**elete: Removing data from the system.
    

### Why Backend Applications Revolve Around CRUD

Backend systems exist to manage state. A backend receives incoming requests, applies rules (**business logic**), and reads from or modifies the underlying storage. Because data lifecycle management boils down to creation, retrieval, modification, and removal, CRUD forms the foundation of RESTful web services.

### Mapping CRUD to REST APIs and HTTP Methods

In REST (Representational State Transfer) architecture, resources (such as `books`, `users`, or `products`) are identified by URIs (Uniform Resource Identifiers). We use standard **HTTP Methods** (verbs) to perform CRUD actions on these resources.

|**CRUD Operation**|**HTTP Method**|**REST API Endpoint Example**|**Purpose**|
|---|---|---|---|
|**Create**|`POST`|`POST /books`|Create a new resource|
|**Read**|`GET`|`GET /books` or `GET /books/{id}`|Retrieve resource(s)|
|**Update**|`PUT` / `PATCH`|`PUT /books/{id}` or `PATCH /books/{id}`|Modify an existing resource|
|**Delete**|`DELETE`|`DELETE /books/{id}`|Remove a resource|

### Concrete Examples across Domains

```
1. E-Commerce (Products)
   - Create:  POST   /products      -> Add a new laptop to the catalog
   - Read:    GET    /products/101  -> View details of product #101
   - Update:  PATCH  /products/101  -> Change the price of product #101
   - Delete:  DELETE /products/101  -> Remove product #101 from inventory

2. User Management (Users)
   - Create:  POST   /users         -> Register a new user profile
   - Read:    GET    /users         -> Fetch all registered users
   - Update:  PUT    /users/42      -> Replace user #42's entire profile
   - Delete:  DELETE /users/42      -> Deactivate/remove user #42

3. Library System (Books)
   - Create:  POST   /books         -> Add a new book record
   - Read:    GET    /books/7       -> Retrieve details of book #7
   - Update:  PATCH  /books/7       -> Mark book #7 as "borrowed"
   - Delete:  DELETE /books/7       -> Scrape book #7 from system
```

## 2. Why Use In-Memory Storage First?

When starting out with backend development, jumping straight into databases (like PostgreSQL, MySQL, or MongoDB) introduces unnecessary complexity. You have to manage database drivers, ORMs, connections, migrations, SQL syntax, and asynchronous drivers alongside API logic.

**In-Memory Storage** means storing your application data inside the server's primary RAM using native programming language data structures—specifically Python `list` and `dict` objects.

Python

```
# Simple in-memory storage for our application state
books_db: list[dict] = []
```

### Advantages of Starting In-Memory

1. **Simplicity:** Zero setup required. No external services to install, configure, or run.
    
2. **Focus on API Logic:** You can spend 100% of your energy understanding HTTP methods, routing, status codes, Pydantic parsing, and business rules.
    
3. **Instant Debugging:** Inspecting state is as simple as printing a Python list or setting a breakpoint.
    

### Limitations of In-Memory Storage

> [!warning] In-Memory Data is Transient!
> 
> - **Volatile Memory:** The moment your FastAPI application stops, reloads, or crashes, all data stored in memory is lost forever.
>     
> - **No Concurrent Persistence:** If you run multiple instances (workers) of your application, they will not share memory state.
>     
> - **Not Production Ready:** RAM size limits scale, and there is no query optimization or structural transaction guarantees.
>     

> [!tip] Why It's Perfect for Learning
> 
> Using a simple list like `books = []` simulates a database. It forces you to write search logic, handle missing records, and construct responses—building the exact mental model needed before introducing persistent databases.

## 3. Understanding the Request Lifecycle

When a client (like Postman or a Web Browser) sends an HTTP request to your FastAPI server, the request traverses multiple processing layers before returning a response.

### Step-by-Step Lifecycle Flow

1. **Client Action:** Client constructs an HTTP request (e.g., `POST /books` with a JSON payload) and transmits it over TCP/IP.
    
2. **FastAPI Routing:** FastAPI matches the incoming HTTP method and path to a specific Python function (route handler).
    
3. **Pydantic Validation:** FastAPI converts the raw JSON string into a structured Pydantic model instance, validating types and required fields.
    
4. **Business Logic Execution:** The route function runs custom validation, checks for business constraints (e.g., duplicate IDs), and prepares the operation.
    
5. **In-Memory Storage Operation:** The Python `list` or `dict` is mutated (e.g., `.append()` or conditional replacement).
    
6. **Response Serialization:** The route function returns a Python dictionary or model. FastAPI automatically converts it into a JSON string and attaches appropriate HTTP response headers and status codes.
    
7. **Client Reception:** The client receives and renders the HTTP JSON response.
    

### Request Lifecycle Sequence Diagram

Фрагмент кода

```
sequenceDiagram
    autonumber
    actor Client as Client (Postman)
    participant FA as FastAPI Router
    participant PY as Pydantic Validation
    participant BL as Business Logic
    participant DB as In-Memory DB (List)

    Client->>FA: HTTP POST /books (JSON Payload)
    FA->>PY: Parse & Validate JSON
    alt Validation Fails
        PY-->>Client: 422 Unprocessable Entity (JSON Error)
    else Validation Succeeds
        PY->>BL: Pass Validated Pydantic Object
        BL->>DB: Check for duplicate ID
        alt Duplicate ID Exists
            BL-->>Client: 404/400 HTTP Exception
        else Unique ID
            BL->>DB: Append item to list (books.append)
            DB-->>BL: Success
            BL-->>FA: Return Created Book Object
            FA-->>Client: HTTP 201 Created (JSON Response)
        end
    end
```

## 4. Building a CRUD API

Let's build a complete, runnable FastAPI application for managing a collection of **Books**.

Create a file named `main.py` and follow along as we build out each operation.

### Base Setup & Data Structure

Python

```
from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel, Field
from typing import Optional

app = FastAPI(title="Book Management API")

# Define the Pydantic schema for data incoming/outgoing
class Book(BaseModel):
    id: int
    title: str = Field(..., min_length=1, max_length=100)
    author: str = Field(..., min_length=1, max_length=50)
    pages: int = Field(..., gt=0)
    is_borrowed: bool = False

# Partial schema for update operations
class BookUpdate(BaseModel):
    title: Optional[str] = Field(None, min_length=1, max_length=100)
    author: Optional[str] = Field(None, min_length=1, max_length=50)
    pages: Optional[int] = Field(None, gt=0)
    is_borrowed: Optional[bool] = None

# In-Memory Database (List of dictionaries)
books_db: list[dict] = []
```

### READ: GET `/books` (Retrieve All)

Python

```
@app.get("/books", response_model=list[Book], status_code=status.HTTP_200_OK)
def get_all_books():
    return books_db
```

#### Code Line-by-Line Explanation:

- `@app.get("/books", ...)`: Decorator registering this function to handle HTTP `GET` requests sent to `/books`.
    
- `response_model=list[Book]`: Instructs FastAPI to validate and format the output data structure as a list of `Book` items.
    
- `status_code=status.HTTP_200_OK`: Explicitly sets the success HTTP status code to `200 OK`.
    
- `def get_all_books():`: Function declaration for the endpoint handler.
    
- `return books_db`: Returns the `books_db` Python list. If empty, it returns `[]`, which is valid behavior for an empty collection.
    

### READ: GET `/books/{id}` (Retrieve Single)

Python

```
@app.get("/books/{book_id}", response_model=Book, status_code=status.HTTP_200_OK)
def get_single_book(book_id: int):
    for book in books_db:
        if book["id"] == book_id:
            return book
    
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail=f"Book with ID {book_id} was not found."
    )
```

#### Code Line-by-Line Explanation:

- `@app.get("/books/{book_id}", ...)`: Registers a GET route taking a path parameter named `book_id`.
    
- `def get_single_book(book_id: int):`: FastAPI parses the path parameter, casts it to an integer, and passes it to the function.
    
- `for book in books_db:`: Iterates through each dictionary in our in-memory list.
    
- `if book["id"] == book_id:`: Checks if the current book's `id` matches the requested `book_id`.
    
- `return book`: Returns the matching book immediately upon finding it.
    
- `raise HTTPException(...)`: If loop completes without finding a match, halts execution and returns a `404 Not Found` response with a detailed JSON error payload.
    

### CREATE: POST `/books` (Create New)

Python

```
@app.post("/books", response_model=Book, status_code=status.HTTP_201_CREATED)
def create_book(book: Book):
    # Check if ID already exists (Business Logic)
    for existing_book in books_db:
        if existing_book["id"] == book.id:
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail=f"Book with ID {book.id} already exists."
            )
    
    # Convert Pydantic model to dictionary
    new_book = book.model_dump()
    
    # Save into in-memory list
    books_db.append(new_book)
    
    return new_book
```

#### Code Line-by-Line Explanation:

- `@app.post("/books", ..., status_code=status.HTTP_201_CREATED)`: Defines a `POST` handler, defaulting the success status code to `201 Created`.
    
- `def create_book(book: Book):`: Expects a JSON request body matching the `Book` schema. FastAPI validates the payload before reaching this line.
    
- `for existing_book in books_db:`: Iterates over stored items to enforce unique identifier constraints.
    
- `if existing_book["id"] == book.id:`: Triggers if a duplicate ID is found.
    
- `raise HTTPException(status_code=400, ...)`: Aborts creation and sends a `400 Bad Request` error to the client.
    
- `new_book = book.model_dump()`: Converts the validated Pydantic model object into a native Python `dict`.
    
- `books_db.append(new_book)`: Appends the new dictionary to our global in-memory `books_db` list.
    
- `return new_book`: Returns the created object, serialized automatically to JSON.
    

### UPDATE: PATCH `/books/{id}` (Partial Update)

Python

```
@app.patch("/books/{book_id}", response_model=Book, status_code=status.HTTP_200_OK)
def update_book(book_id: int, book_update: BookUpdate):
    for book in books_db:
        if book["id"] == book_id:
            # Filter out fields that were not provided in the request
            update_data = book_update.model_dump(exclude_unset=True)
            
            # Apply updates directly to the dictionary
            book.update(update_data)
            return book
            
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail=f"Cannot update. Book with ID {book_id} was not found."
    )
```

#### Code Line-by-Line Explanation:

- `@app.patch("/books/{book_id}", ...)`: Registers a `PATCH` route for modifying specific fields of a resource.
    
- `def update_book(book_id: int, book_update: BookUpdate):`: Accepts both a path parameter (`book_id`) and a partial body payload (`book_update`).
    
- `for book in books_db:`: Searches for the target resource.
    
- `update_data = book_update.model_dump(exclude_unset=True)`: Extracts _only_ the fields explicitly set by the client in the request body, ignoring default `None` values.
    
- `book.update(update_data)`: Mutates the target dictionary by overwriting key-value pairs present in `update_data`.
    
- `return book`: Returns the updated book dictionary.
    
- `raise HTTPException(...)`: Raises a `404 Not Found` exception if no match is found.
    

> [!important] PUT vs. PATCH
> 
> - **`PUT` (Full Replacement):** Requires the client to send the **entire updated resource representation**. Any omitted field is overwritten with `None` or default values.
>     
> - **`PATCH` (Partial Modification):** Modifies **only the provided fields**. Omitted fields remain untouched.
>     

|**Feature**|**PUT Method**|**PATCH Method**|
|---|---|---|
|**Intent**|Replace the whole resource|Modify specific fields|
|**Payload**|Complete object payload|Partial object payload|
|**Missing Fields**|Reset or overwritten to defaults|Preserved as they are|
|**Idempotent?**|Yes|Generally Yes (depending on logic)|

### DELETE: DELETE `/books/{id}` (Remove)

Python

```
@app.delete("/books/{book_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_book(book_id: int):
    for index, book in enumerate(books_db):
        if book["id"] == book_id:
            books_db.pop(index)
            return  # Returns no content body with 204 status code
            
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail=f"Cannot delete. Book with ID {book_id} was not found."
    )
```

#### Code Line-by-Line Explanation:

- `@app.delete("/books/{book_id}", status_code=status.HTTP_204_NO_CONTENT)`: Maps `DELETE` requests to this handler and configures a `204 No Content` status code.
    
- `for index, book in enumerate(books_db):`: Uses `enumerate()` to track both list index and content dictionary during iteration.
    
- `if book["id"] == book_id:`: Checks for an ID match.
    
- `books_db.pop(index)`: Removes the dictionary at index position `index` from `books_db`.
    
- `return`: Ends execution. Because status code is `204`, no HTTP response body is sent back to the client.
    
- `raise HTTPException(...)`: Raises a `404 Not Found` if no match exists.
    

## 5. Working with Python Lists

When using in-memory storage, your backend capability depends directly on how you manipulate core Python primitives.

### Translation of List Operations to CRUD Actions

|**List Method**|**CRUD Translation**|**Example Usage**|
|---|---|---|
|`.append(item)`|**Create**|Appends a new item dictionary to the end of the collection list.|
|`iteration` / `comprehension`|**Read**|Loops through items to search for matching parameters or apply filters.|
|`dict.update()` / assignment|**Update**|Modifies inner key-value pairs of a targeted dictionary item.|
|`.pop(index)` / `.remove()`|**Delete**|Removes an item from the collection by index or value matching.|

### Common List Patterns in Backend Handlers

Python

```
# 1. Searching for an item by ID
target_item = next((item for item in items_db if item["id"] == target_id), None)

# 2. Filtering items (e.g., getting all borrowed books)
borrowed_books = [book for book in books_db if book["is_borrowed"] is True]

# 3. Deleting an item securely using list comprehension
books_db = [book for book in books_db if book["id"] != target_id]
```

## 6. Business Logic

**Business Logic** refers to the custom rules, validations, and workflows specific to your domain application. It lives inside your backend code—not inside FastAPI itself.

FastAPI handles routing and JSON parsing, but **you** are responsible for enforcing domain logic rules:

Python

```
@app.post("/books", status_code=status.HTTP_201_CREATED)
def create_book_with_business_logic(book: Book):
    # Business Rule 1: Check for duplicate IDs
    if any(b["id"] == book.id for b in books_db):
        raise HTTPException(
            status_code=400, 
            detail="Business Rule Violation: ID already in use."
        )

    # Business Rule 2: Enforce unique book titles
    if any(b["title"].lower() == book.title.lower() for b in books_db):
        raise HTTPException(
            status_code=400, 
            detail="Business Rule Violation: A book with this title already exists."
        )
    
    # Business Rule 3: Enforce operational caps (e.g., max 100 books limit)
    if len(books_db) >= 100:
        raise HTTPException(
            status_code=400,
            detail="Capacity Reached: Inventory cannot exceed 100 books."
        )

    new_book = book.model_dump()
    books_db.append(new_book)
    return new_book
```

## 7. Error Handling

Proper error handling prevents your application from crashing unexpectedly and provides clear feedback to the client. In FastAPI, error handling is implemented using `HTTPException`.

Python

```
from fastapi import HTTPException, status
```

### Common HTTP Exception Scenarios

Python

```
# Scenario 1: Resource Not Found (404)
raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="The requested item was not found."
)

# Scenario 2: Bad Request / Business Logic Violation (400)
raise HTTPException(
    status_code=status.HTTP_400_BAD_REQUEST,
    detail="Invalid operation. Cannot delete a borrowed book."
)

# Scenario 3: Validation Error (422)
# Handled automatically by FastAPI when request payload fails Pydantic schema validation.
```

## 8. Response Status Codes

Using accurate HTTP status codes is essential for building well-designed RESTful APIs.

|**Status Code**|**Name**|**Usage in CRUD Operations**|
|---|---|---|
|**`200 OK`**|Standard Success|Successful `GET`, `PATCH`, or `PUT` operations.|
|**`201 Created`**|Resource Created|Successful `POST` operation creating a new entry.|
|**`204 No Content`**|Action Succeeded, No Data Returned|Successful `DELETE` operation where no response body is sent.|
|**`400 Bad Request`**|Client Error|Business logic failure (e.g., duplicate ID, invalid state transition).|
|**`404 Not Found`**|Resource Missing|Requesting an ID that does not exist in the collection.|
|**`422 Unprocessable Entity`**|Schema Validation Failure|Sent automatically by FastAPI when JSON structure or type validation fails.|

## 9. Testing with Postman

Postman allows you to send raw HTTP requests to your FastAPI application to test all endpoint routes.

### Execution Workflow

#### 1. Create a Book (`POST /books`)

- **URL:** `[http://127.0.0.1:8000/books](http://127.0.0.1:8000/books)`
    
- **Method:** `POST`
    
- **Headers:** `Content-Type: application/json`
    
- **Body (raw JSON):**
    
    JSON
    
    ```
    {
      "id": 1,
      "title": "Clean Code",
      "author": "Robert C. Martin",
      "pages": 464,
      "is_borrowed": false
    }
    ```
    
- **Expected Response Status:** `201 Created`
    

#### 2. Get All Books (`GET /books`)

- **URL:** `[http://127.0.0.1:8000/books](http://127.0.0.1:8000/books)`
    
- **Method:** `GET`
    
- **Expected Response Status:** `200 OK`
    
- **Response Payload:**
    
    JSON
    
    ```
    [
      {
        "id": 1,
        "title": "Clean Code",
        "author": "Robert C. Martin",
        "pages": 464,
        "is_borrowed": false
      }
    ]
    ```
    

#### 3. Update a Book (`PATCH /books/1`)

- **URL:** `[http://127.0.0.1:8000/books/1](http://127.0.0.1:8000/books/1)`
    
- **Method:** `PATCH`
    
- **Body (raw JSON):**
    
    JSON
    
    ```
    {
      "is_borrowed": true
    }
    ```
    
- **Expected Response Status:** `200 OK`
    

#### 4. Delete a Book (`DELETE /books/1`)

- **URL:** `[http://127.0.0.1:8000/books/1](http://127.0.0.1:8000/books/1)`
    
- **Method:** `DELETE`
    
- **Expected Response Status:** `204 No Content` (Empty response body)
    

## 10. Common Beginner Mistakes

> [!warning] Common Mistake Checklist

### 1. Forgetting to `.append()` new items

- **Symptom:** API returns `201 Created`, but subsequent `GET /books` calls show an empty list.
    
- **Cause:** Validating or transforming data without storing it back into `books_db`.
    
- **Fix:** Ensure `books_db.append(new_item)` is called explicitly.
    

### 2. Confusing Dictionary Access with Object Attribute Access

- **Symptom:** `TypeError: 'dict' object is not subscriptable` or `AttributeError: 'dict' object has no attribute 'id'`.
    
- **Cause:** Pydantic models use dot notation (`book.id`), while Python dictionaries use bracket notation (`book["id"]`).
    
- **Fix:** If stored as `dict` via `.model_dump()`, access properties using `book["id"]`.
    

### 3. Missing Exclude Unset in PATCH Operations

- **Symptom:** Running `PATCH` sets all unspecified fields to `null` or default values.
    
- **Cause:** Converting update models using `.model_dump()` without `exclude_unset=True`.
    
- **Fix:** Use `update_data = update_model.model_dump(exclude_unset=True)`.
    

### 4. Forgetting `raise` before `HTTPException`

- **Symptom:** Code continues executing after an error condition instead of halting.
    
- **Cause:** Writing `HTTPException(...)` instead of `raise HTTPException(...)`.
    
- **Fix:** Always prepend error instances with the `raise` keyword.
    

## 11. Practical Exercises

Complete these exercises to consolidate your understanding of in-memory CRUD development.

### Exercise 1: Student Management API (Difficulty: Easy)

Build a complete FastAPI application managing `students`.

- **Schema:** `id` (int), `name` (str), `grade` (float), `is_enrolled` (bool).
    
- **Endpoints:** Implement standard `GET`, `POST`, `PATCH`, and `DELETE` routes.
    
- **Requirement:** Add validation ensuring `grade` remains between `0.0` and `100.0`.
    

### Exercise 2: Task Manager API with Status Filtering (Difficulty: Medium)

Build a CRUD application for task management.

- **Schema:** `id` (int), `title` (str), `description` (str), `status` (enum or str: `"pending"`, `"in_progress"`, `"completed"`).
    
- **Endpoints:** Add filtering capability to `GET /tasks` so that passing a query parameter like `GET /tasks?status=completed` returns matching tasks only.
    

### Exercise 3: Inventory System with Stock Logic (Difficulty: Hard)

Create a Product API enforcing inventory constraints.

- **Schema:** `id` (int), `name` (str), `price` (float), `quantity` (int).
    
- **Custom Route:** Implement a `POST /products/{id}/sell` endpoint that decreases product quantity by a specified amount in the request body. Raise an `HTTPException` (400 Bad Request) if stock drops below 0.
    

## 12. Interview Questions & Answers

### Q1: What does CRUD stand for, and how does it map to standard HTTP methods?

**Answer:** CRUD stands for Create, Read, Update, and Delete. In REST APIs, it maps to HTTP methods as follows: Create -> `POST`, Read -> `GET`, Update -> `PUT` or `PATCH`, and Delete -> `DELETE`.

### Q2: What is the primary difference between HTTP `PUT` and `PATCH` methods?

**Answer:** `PUT` replaces the entire target resource with the request payload. `PATCH` applies partial modifications to a resource, updating only the fields provided in the request body.

### Q3: Why is storing application state in Python lists problematic for production deployment?

**Answer:** In-memory storage is volatile and clears whenever the process restarts or crashes. It does not support shared state across multiple server instances or processes, leading to data inconsistencies and loss.

### Q4: What HTTP status code should be returned when a resource is successfully created?

**Answer:** `201 Created`.

### Q5: What status code should a `DELETE` endpoint return if it does not send any content in the response body?

**Answer:** `204 No Content`.

### Q6: How does FastAPI use Pydantic models in CRUD operations?

**Answer:** FastAPI uses Pydantic models to validate incoming JSON request structures, enforce data type safety, sanitize payloads, and automatically document endpoints using OpenAPI schemas.

### Q7: What is the purpose of `exclude_unset=True` when calling `model_dump()` in a `PATCH` route?

**Answer:** `exclude_unset=True` ensures that only fields explicitly provided in the request payload are included in the output dictionary, preventing omitted model fields from overwriting existing data with `None`.

### Q8: What exception should be raised when a user requests an ID that does not exist?

**Answer:** `raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="...")`.

### Q9: How can business logic prevent duplicate primary identifiers in an in-memory CRUD setup?

**Answer:** By scanning the list before inserting new records and checking if the requested ID already exists. If a match is found, raise an `HTTPException` with a `400 Bad Request` status code.

### Q10: What does an HTTP `422 Unprocessable Entity` status code indicate in FastAPI?

**Answer:** It indicates that the client request body failed FastAPI's automatic Pydantic schema validation due to invalid types, missing required fields, or failed constraint rules.

### Q11: Does a `GET` request return `404 Not Found` if a search collection contains no records (`[]`)?

**Answer:** No. Returning an empty list (`[]`) with a `200 OK` status code is the correct RESTful behavior when a collection contains no items.

### Q12: How do you extract raw dictionary data from a Pydantic model instance in Pydantic V2?

**Answer:** By calling `.model_dump()` on the model instance.

### Q13: What is the difference between path parameters and body parameters in FastAPI handlers?

**Answer:** Path parameters are embedded directly into the URL path (e.g., `/books/{id}`) and are typically used to identify specific resources. Body parameters are passed in the HTTP request payload as JSON objects and are used to send structured resource data.

### Q14: Where does business logic belong in a web backend?

**Answer:** Business logic belongs inside the backend application's service or handler layer. It is written by the developer to enforce business rules and domain constraints, distinct from the framework's routing logic.

### Q15: Why is it helpful to learn in-memory CRUD operations before learning databases?

**Answer:** Storing data in memory removes external complexities like database configurations, SQL syntax, and ORM setups. This lets you focus entirely on core API concepts like routing, HTTP methods, Pydantic parsing, status codes, and error handling.

## 13. Knowledge Check

Use these questions to self-assess your understanding without looking at previous sections.

1. Which HTTP method is considered non-idempotent by default in standard REST designs?
    
2. What Python standard dictionary method allows you to update multiple key-value pairs at once?
    
3. What happens to in-memory data when Uvicorn reloads your FastAPI application during development?
    
4. What HTTP status code does FastAPI automatically return when payload validation fails?
    
5. How do path parameters differ syntactically from query parameters in endpoint signatures?
    
6. Why is returning `HTTPException` better than returning a custom error dictionary in FastAPI?
    
7. What is the execution sequence when a request hits a FastAPI application route handler?
    
8. When should you use `PUT` instead of `PATCH`?
    
9. How do you convert a Pydantic model object to a standard Python dictionary in Pydantic V2?
    
10. What Python builtin function tracks both item index and item value during list iteration?
    
11. Which status code should be returned if a client attempts to create an item with an ID that already exists?
    
12. Why should you avoid storing global persistent data inside request handler function bodies?
    
13. What is the role of `response_model` in FastAPI route decorators?
    
14. What are three major operational limitations of in-memory lists for data management?
    
15. What steps should a backend application take when processing a resource deletion request?
    

## 14. Summary

### Key Takeaways

- **CRUD Mechanics:** CRUD mapping forms the backbone of web APIs (`POST` -> Create, `GET` -> Read, `PATCH`/`PUT` -> Update, `DELETE` -> Delete).
    
- **Request Lifecycle:** Requests flow from Client -> Routing -> Pydantic Validation -> Business Logic Enforcement -> In-Memory Mutative Action -> JSON Response.
    
- **In-Memory Simplicity:** Using standard Python `list` structures isolates API routing concepts from database management overhead.
    
- **Defensive Design:** Raise proper HTTP status codes (`201`, `204`, `400`, `404`, `422`) to build reliable, predictable interfaces.
    

```
       [Client Request]
              │
              ▼
   ┌──────────────────────┐
   │  FastAPI Router      │
   └──────────┬───────────┘
              │
              ▼
   ┌──────────────────────┐
   │  Pydantic Validation │
   └──────────┬───────────┘
              │
              ▼
   ┌──────────────────────┐
   │  Business Logic      │
   └──────────┬───────────┘
              │
              ▼
   ┌──────────────────────┐
   │ In-Memory Data Store │ (Python List / Dict)
   └──────────────────────┘
```

[[Request_bodies_and_Pydantic_models]]
[[Project_Structure_&_Software_Development]]