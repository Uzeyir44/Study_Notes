
## 1. What is an API?

### Definition

An **API** stands for **Application Programming Interface**.

- **Application:** Any software program designed to fulfill a specific task or function.
    
- **Programming:** Code written by software engineers to control computer systems.
    
- **Interface:** A boundary or point of interaction where two distinct, independent systems meet and exchange data or instructions.
    

In backend development, an API is a software intermediary that allows two separate applications to talk to each other. It defines a formal contract of acceptable requests, data structures, and expected responses.

### Why APIs Exist & The Problem They Solve

Before APIs became standard, integrating different software components required engineers to have a deep, intimate knowledge of each other's codebases, data structures, and programming languages.

If a travel aggregation website wanted to display flight schedules from fifty different airlines, it would have to build fifty custom integrations. It would need to read directly from fifty different database types, understand fifty custom internal code implementations, and adapt whenever any airline modified its internal table columns. This was unsustainable, brittle, and insecure.

APIs solve this by introducing an abstraction layer. An API acts as a universal adapter. It hides the underlying complexity of the server's codebase, business logic, and database engines behind a set of clean, predictable access points.

### The Everyday Analogy: The Restaurant Waiter

To visualize an API, consider the classic analogy of dining at a restaurant:

```
+--------------------+        +---------------------+        +--------------------+
|       Customer     |        |       Waiter        |        |      Kitchen       |
|  (Wants to Eat)    | -----> | (Takes Order/Menu)  | -----> | (Prepares Food)    |
|     [Client]       |        |        [API]        |        |     [Server]       |
+--------------------+        +---------------------+        +--------------------+
```

1. **The Customer (The Client):** You sit at a table. You want to order a meal, but you do not have direct physical access to the kitchen, shelves, stoves, or ingredients.
    
2. **The Kitchen (The Server/Database):** This is the backend system where raw ingredients are stored and cooked into complex dishes according to private recipes.
    
3. **The Menu & The Waiter (The API):** The menu lists the _only_ available requests you can make. The waiter is the intermediary interface. You review the menu, give your order to the waiter, the waiter carries that structured request to the kitchen, retrieves the prepared meal (the response) from the chef, and delivers it back to your table.
    

You never need to know _how_ the chef cooked the food, which pan they used, or how the kitchen organized its refrigerator. The waiter abstracted all of that complexity away from you.

### Relationship Between Client, API, Server, and Database

An API sits cleanly in front of your server's core application code, creating an orchestration layer that guards and interacts with your storage systems.

#### Mermaid Architecture Diagram

Фрагмент кода

```
graph LR
    subgraph Client Layer
        Browser[Web Browser]
        MobileApp[Mobile Application]
    end

    subgraph Backend Infrastructure
        API[API Gateway / Routing Layer]
        Server[Application Server Logic]
        DB[(Database Engine)]
    end

    Browser -->|HTTP Request| API
    MobileApp -->|HTTP Request| API
    API -->|Route Mapping| Server
    Server -->|SQL / Queries| DB
    DB -->|Raw Data Rows| Server
    Server -->|Structured Payload| API
    API -->|HTTP Response| Browser
    API -->|HTTP Response| MobileApp
```

## 2. Why Clients Do Not Access Databases Directly

A common question for beginner backend developers is: _"If our web application has a frontend client and a backend database, why don't we let the client query the database directly instead of building an API in the middle?"_ Bypassing the API layer and exposing your raw database engine to client applications introduces severe system risks.

### 1. Security

Exposing a database port to the open internet means anyone can attempt to connect to it. If a client application contains database credentials (such as username and password) inside its source code, malicious actors can extract those credentials through reverse-engineering. Once a hacker has direct database access, they can steal data, drop tables, or install ransomware.

### 2. Validation & Business Logic

Databases enforce rigid technical constraints (like making sure a data column is a number), but they lack context for complex business workflows.

- **Example:** If a user registers an account, the application must verify the email isn't a duplicate, check that the password meets complexity rules, send a welcome email, and log an audit trail event. A database cannot manage these auxiliary operations by itself. The API acts as the central brain that orchestrates these multi-step processes.
    

### 3. Authentication vs. Authorization

- **Authentication:** Verifying _who_ a user is.
    
- **Authorization:** Verifying what that specific user is _allowed_ to do.
    

If a client talks directly to a database, it is difficult to restrict row-level privileges safely. For instance, you might want a user to read _only_ their own private medical records, while preventing them from reading records belonging to other patients. An API inspects the user's secure token on every request, evaluates permission rules in code, and appends safe filters to database queries to guarantee isolation.

### 4. Scalability

Database connections are resource-intensive and limited. A database server can handle only a few hundred or thousand concurrent direct connection sockets before running out of memory. An API layer acts as a buffer. It uses connection pooling, handles connection lifecycle optimization, and scales across multiple app servers independently, insulating the database engine from getting overwhelmed by massive client traffic spikes.

### 5. Maintainability

If you build a mobile application that queries database tables directly, the client app's internal code is bound tightly to your specific database schema design. If you need to rename a table column or switch from a relational database to a NoSQL engine, your mobile application breaks immediately. You would have to force every single user to download an update from the app store.

With an API, you can rewrite your entire database layer, change column names, or migrate engines completely behind the scenes. As long as the API output contract remains identical, client applications keep running perfectly.

## 3. Types of APIs

While this note focuses on Web APIs, it is important to know that APIs exist at every layer of computer systems architecture.

### Web APIs

APIs that communicate across networks using internet protocol systems (predominantly HTTP). They allow remote clients to interact with centralized cloud applications and databases. Examples include the Twitter API, Google Maps API, or your custom application backend.

### Library / Programmatic APIs

APIs exposed by a code package, framework, or library within the same runtime environment. When you import a utility module in JavaScript, Python, or C, the public functions and classes available for you to call are that library's programmatic API.

### Operating System APIs

APIs that allow user applications to safely request services from the underlying operating system kernel. For instance, when a programming script attempts to read a file from the hard drive or open a network connection, it calls the operating system's built-in APIs (such as POSIX or Windows API).

### Hardware APIs

APIs used to communicate directly with physical peripheral components. For example, modern web apps use the WebBluetooth or WebUSB browser APIs to let code communicate safely with local physical hardware devices.

## 4. What Happens When an API Request Is Sent?

Let's trace the full lifecycle of an API interaction from first principles, outlining how a client action transitions into a data modification event.

### Step-by-Step Request Lifecycle

1. **User Action:** A user clicks a "Delete Comment" button inside a social media web application.
    
2. **Client Creates HTTP Request:** The JavaScript frontend handles the click event and constructs an HTTP network message targeting the appropriate resource route path.
    
3. **API Receives Request:** The network routes the packets across the internet to the backend application server port. The API router reads the request verb and path, ensures headers are present, and validates the user's authentication token.
    
4. **Business Logic Executes:** The API passes the payload to the application controller logic. The controller checks permissions (ensuring this user actually owns the comment they are trying to delete) and prepares a command.
    
5. **Database Interaction:** The server code establishes a secure connection to the database engine and executes a query to remove the target row from the data table.
    
6. **API Prepares Response:** The database returns a confirmation code to the server application. The server wraps this result inside a structured HTTP response block alongside a matching success status code.
    
7. **Client Receives Response:** The network routes the response back to the browser. The frontend client processes the message and updates the UI layout to remove the comment from the user's screen.
    

### Mermaid Sequence Diagram

Фрагмент кода

```
sequenceDiagram
    autonumber
    actor User
    participant Client as Frontend Client
    participant API as API Layer (Server)
    participant DB as Database Engine

    User->>Client: Click "Delete Comment"
    Client->>API: HTTP DELETE /comments/402 (with Auth Token)
    activate API
    Note over API: Step 3: Authenticate identity<br/>& parse URL endpoint path
    Note over API: Step 4: Run business logic rules.<br/>Does user own comment 402?
    API->>DB: SQL: DELETE FROM comments WHERE id = 402;
    activate DB
    DB-->>API: Query Ok (1 row affected)
    deactivate DB
    Note over API: Step 6: Construct response<br/>(Status: 200 OK)
    API-->>Client: HTTP Response Payload
    deactivate API
    Client-->>User: Visually remove comment row from UI
```

## 5. REST (Representational State Transfer)

### What is REST?

**REST** stands for **Representational State Transfer**. Introduced by Roy Fielding in his 2000 doctoral dissertation, REST is not a piece of software, a code framework, or a rigid protocol specification. REST is an **architectural style**—a set of design principles and constraints used for building distributed hypermedia systems.

An API that adheres to these core architectural constraints is described as **RESTful**.

### Why REST Became Popular

In the early days of the web, web services relied heavily on protocols like SOAP (Simple Object Access Protocol), which were highly complex, heavily reliant on XML, and difficult to parse in simple web browsers.

REST gained dominance because it leveraged the pre-existing features of standard HTTP. Instead of creating new protocols, REST treats standard HTTP verbs, status codes, and headers as a highly expressive, standardized interface vocabulary. This made it lightweight, human-readable, and easy to implement across different client and server tech stacks.

### Core REST Principles for Beginners

#### 1. Client-Server Separation

The client and server must remain completely independent. The client should not care about database storage or backend operations; the server should not care about user interface layouts or browser state. They interact exclusively through the interface contract.

#### 2. Statelessness

Every REST request from a client must contain all the information necessary for the server to understand and process it. The server never stores session state information about past requests in its local memory.

#### 3. Cacheability

Responses from the API must explicitly define themselves as cacheable or non-cacheable. This allows clients or intermediate proxy networks to cache responses locally, reducing server load and improving system speed.

#### 4. Uniform Interface

This is the most critical constraint for api design. It requires that communication across the entire API follows a highly predictable, standardized format. No matter which resource you access, the naming structures, method verbs, and response patterns must look uniform.

## 6. Resources

### What is a Resource?

In the world of REST, everything is a **Resource**. A resource is any data entity, object, or service that your backend application manages and exposes to the internet. It can be a user profile, a photograph, a product order, a chat comment, or an individual tracking metric.

### Resource Representation

Clients never interact directly with the raw database rows or physical files on a server. Instead, they interact with a **representation** of that resource. The server pulls raw data out of its database tables, packages it into a standard text structure (like JSON), and transmits that representation across the wire.

### Resource Naming Conventions

REST mandates that resources are mapped to **URIs (Uniform Resource Identifiers)** using strict hierarchical path trees.

To model resources correctly, paths should use nouns that reflect collections and individual instances.

```
+-------------------------------------------------------------+
| /posts                <-- Collection of all post resources  |
+-------------------------------------------------------------+
| /posts/45             <-- Individual post instance with ID45|
+-------------------------------------------------------------+
| /posts/45/comments    <-- Collection of comments for post 45|
+-------------------------------------------------------------+
```

- **Collection Resources:** A path pointing to an entire group or array of entities. Always use plural nouns for collections (e.g., `/users`, `/products`, `/orders`).
    
- **Individual Resources:** A path pointing to a single entity within a collection, specified by appending a unique identifier (ID) variable to the collection path (e.g., `/users/12`, `/products/99`).
    

## 7. Endpoints

### Definition

An **Endpoint** is the specific URL or path location where an API can be reached to access or manipulate resources. It represents the entry point through which a client connects to your backend code routes.

In REST, an endpoint is the combination of a resource **URL** and an **HTTP Method Verb**. For example, `GET /users` and `POST /users` point to the exact same URL path, but they are two completely distinct API endpoints because they perform different actions.

### URI vs. URL

It is helpful to clarify these two frequently interchanged network engineering acronyms:

[Image comparing URI and URL concepts, showing URL as a subset of URI]

- **URI (Uniform Resource Identifier):** A broad label for any string identifier used to name a resource on the internet.
    
- **URL (Uniform Resource Locator):** A specific type of URI that provides the explicit _address_ or mechanism to locate that resource on the web.
    

```
                  https://api.example.com/v1/users/42
                  \_____________________/\__________/
                             |                 |
                         URL Host         URI Path Route
```

### Route Parameters

Route parameters (or path parameters) are dynamic variables embedded directly inside an endpoint path string, denoted by a colon placeholder (`:id`). They tell the backend server exactly which resource instance to target.

- **Example Endpoint Pattern:** `/users/:userId/books/:bookId`
    
- **Realized Client Request Path:** `/users/88/books/1429`
    
- **Backend Processing:** The routing system parses this path, extracts `userId = 88` and `bookId = 1429`, and passes these variables into the database query logic.
    

## 8. CRUD Operations

**CRUD** is a database architecture acronym that maps directly to the life cycle of data records. A major design strength of REST is how cleanly CRUD database goals map onto native HTTP methods.

### CRUD to HTTP Method Mapping Table

| **CRUD Operation** | **Database Intent**             | **REST HTTP Method** | **Is Safe?** | **Is Idempotent?**             |
| ------------------ | ------------------------------- | -------------------- | ------------ | ------------------------------ |
| **C**reate         | Insert a brand new record       | **POST**             | No           | No                             |
| **R**ead           | Fetch/Retrieve existing records | **GET**              | **Yes**      | **Yes**                        |
| **U**pdate         | Modify an existing record       | **PUT** / **PATCH**  | No           | **PUT** (Yes) / **PATCH** (No) |
| **D**elete         | Permanently remove a record     | **DELETE**           | No           | **Yes**                        |

### Practical Mapping Examples

#### Create Operation

- **Goal:** Register a new user in the system.
    
- **Endpoint:** `POST /users`
    
- **Request Payload:**
    

HTTP

```
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "username": "alice",
  "email": "alice@example.com"
}
```

- **Expected Response Status:** `201 Created`
    

#### Read Operation

- **Goal:** Fetch details for a specific user.
    
- **Endpoint:** `GET /users/42`
    
- **Request Payload:** _(Empty body)_
    

HTTP

```
GET /users/42 HTTP/1.1
Host: api.example.com
```

- **Expected Response Status:** `200 OK`
    

#### Update Operation (Partial Modification)

- **Goal:** Update only a user's email address.
    
- **Endpoint:** `PATCH /users/42`
    
- **Request Payload:**
    

HTTP

```
PATCH /users/42 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "alice-new@example.com"
}
```

- **Expected Response Status:** `200 OK`
    

#### Delete Operation

- **Goal:** Wipe a user out of the tracking database.
    
- **Endpoint:** `DELETE /users/42`
    
- **Request Payload:** _(Empty body)_
    

HTTP

```
DELETE /users/42 HTTP/1.1
Host: api.example.com
```

- **Expected Response Status:** `204 No Content`
    

## 9. Designing Good REST APIs

When building REST APIs, following clean design conventions is essential. It ensures your API remains intuitive and easy for frontend developers to integrate.

### Core REST Design Rules

#### 1. Use Nouns, Never Verbs

Endpoints must represent _resources_, not actions. The action is already defined by the HTTP method verb (`GET`, `POST`, etc.). Do not include verbs inside the path string.

#### 2. Always Use Plural Nouns

Keep resource paths uniform by defaulting to plural names across all endpoints.

#### 3. Establish Clear Resource Hierarchies

When a resource depends on or belongs to another resource, reflect that relationship directly in the path tree structure.

### Endpoint Comparison Table

|**Intent**|**❌ Bad REST Design (Verb/Malformed)**|**Good REST Design (Noun/Hierarchical)**|
|---|---|---|
|Fetch list of all books|`GET /getAllBooks`|`GET /books`|
|Create a new book entry|`POST /createNewBook`|`POST /books`|
|Fetch individual book|`GET /book?id=105`|`GET /books/105`|
|Delete a book record|`POST /books/105/delete`|`DELETE /books/105`|
|View reviews for a book|`GET /getViewsForBook105`|`GET /books/105/reviews`|
|Update a single review|`PUT /updateReview/3`|`PUT /books/105/reviews/3`|

## 10. Statelessness in REST

### What Does Stateless Mean?

Statelessness means that the server treats every incoming HTTP request as a completely isolated transaction. The server does not store past request details, session tokens, or client history in its local memory (RAM).

> [!important] The Stateless Rule
> 
> Every single request must arrive carrying **100% of the context, authorization credentials, and parameters** needed to process it. If a request lacks data, the server will not attempt to fill in the blanks from a previous request.

### Why REST Uses Stateless Communication

1. **Horizontal Scalability:** If your web traffic spikes, you can deploy five identical server instances behind a load balancer. Because no individual server stores user session details in local memory, the load balancer can route a user's requests to _any_ available server instance. Each server can process any request instantly, as long as the request contains the necessary authentication token.
    

```
                     +---------------+
                     | Load Balancer |
                     +---------------+
                       /           \
                      /             \
                     v               v
             +----------+        +----------+
             | Server A |        | Server B |  <-- Any server can handle any request
             +----------+        +----------+
```

2. **Reliability & Simplicity:** If Server A crashes, you can safely shut it down. Since there are no local user sessions saved on that machine, no user data or active login states are lost.
    

### Practical Example: Stateful vs. Stateless Interaction

#### Stateful System (Anti-Pattern)

- **Client:** `POST /login` (Credentials fine. Server remembers this connection as logged-in User 12).
    
- **Client:** `POST /posts` (Payload: `{"title": "My Post"}`).
    
- **Server:** _Saves data successfully because it remembers who the user is from the previous request._
    

#### Stateless REST System (Standard Pattern)

- **Client:** `POST /login` (Credentials fine. Server returns a secure cryptographic token back to client, then forgets the transaction).
    
- **Client:** `POST /posts` (Payload: `{"title": "My Post"}`).
    
- **Server:** `401 Unauthorized`. _Reason: I do not know who you are. This is a new request, and you did not include an identity token._
    
- **Corrected Request:** `POST /posts` (Payload + Header: `Authorization: Bearer token_xyz`).
    
- **Server:** `201 Created`. _Reason: I parsed the token inside this request and verified your identity._
    

## 11. Example REST API: Library Management System

Let's apply these design guidelines to map out a REST API for a Library Management System.

### Endpoints Architecture Design Contract

```
+------------------------------------------------------------------------------------+
| VIEW ALL BOOKS                                                                     |
| Endpoint:  GET /books                                                              |
| Purpose:   Fetch an array of all book objects in the library catalog database.     |
+------------------------------------------------------------------------------------+
| VIEW A SINGLE BOOK                                                                 |
| Endpoint:  GET /books/:id                                                          |
| Purpose:   Fetch data for one specific book instance using its ID.                 |
+------------------------------------------------------------------------------------+
| ADD A NEW BOOK                                                                     |
| Endpoint:  POST /books                                                             |
| Purpose:   Insert a new book record. Payload contains structural book parameters.  |
+------------------------------------------------------------------------------------+
| UPDATE A BOOK                                                                      |
| Endpoint:  PUT /books/:id  or  PATCH /books/:id                                    |
| Purpose:   Modify details for an existing book instance.                           |
+------------------------------------------------------------------------------------+
| DELETE A BOOK                                                                      |
| Endpoint:  DELETE /books/:id                                                       |
| Purpose:   Permanently delete a book record from the backend tables.               |
+------------------------------------------------------------------------------------+
```

### Typical Request/Response Payloads

#### Fetch Single Book

- **Request String:**
    

HTTP

```
GET /books/978 HTTP/1.1
Host: library-api.com
Accept: application/json
```

- **Response String:**
    

HTTP

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 978,
  "title": "The Picture of Dorian Gray",
  "author": "Oscar Wilde",
  "available": true
}
```

#### Add New Book

- **Request String:**
    

HTTP

```
POST /books HTTP/1.1
Host: library-api.com
Content-Type: application/json

{
  "title": "Crime and Punishment",
  "author": "Fyodor Dostoevsky"
}
```

- **Response String:**
    

HTTP

```
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 1024,
  "title": "Crime and Punishment",
  "author": "Fyodor Dostoevsky",
  "available": true
}
```

## 12. Real-World Example: Navigating Instagram

Let's see how a frontend application maps user interactions to RESTful API endpoints behind the scenes.

### 1. Loading the Main Feed

- **User Action:** You open the Instagram app on your phone.
    
- **Underlying API Call:** `GET /feed?page=1&limit=20`
    
- **Backend Action:** The server identifies your user ID from your secure authentication token, queries the database for recent posts from accounts you follow, and returns an array of post objects.
    

### 2. Viewing a User's Profile

- **User Action:** You tap on a friend's handle name (`@traveler_uzair`).
    
- **Underlying API Call:** `GET /users/traveler_uzair/profile`
    
- **Backend Action:** The database finds the profile metrics for that specific username, returning their bio text, follower counts, and avatar asset links.
    

### 3. Creating a New Post

- **User Action:** You select a photograph, type a caption description, and tap "Share".
    
- **Underlying API Call:** `POST /posts`
    
- **Backend Action:** The client sends the image URL and caption text payload down to the server. The server writes a new record into the database's post table, returning a `201 Created` status code.
    

### 4. Liking a Post

- **User Action:** You double-tap a photo to like it.
    
- **Underlying API Call:** `POST /posts/5532/likes` (or `PUT /posts/5532/likes/my_user_id`)
    
- **Backend Action:** The server updates the post's engagement metrics or creates a record in a likes tracking table to link your account to that post ID.
    

### 5. Deleting a Comment

- **User Action:** You slide a comment left and tap the trash bin icon.
    
- **Underlying API Call:** `DELETE /posts/5532/comments/901`
    
- **Backend Action:** The server verifies that you own either the post or the comment, deletes comment ID 901 from the database table, and returns a `204 No Content` status code.
    

## 13. Common Beginner Mistakes & Misconceptions

> [!warning] Critical Backend Mental Shifts

### 1. "The API is the Database."

- **Correction:** They are completely separate systems. The database is a storage engine that saves data to a disk (e.g., PostgreSQL). The API is an application code layer (e.g., Node.js or Python logic) that sits _in front of_ the database. The API acts as a gatekeeper, validating inputs and managing business logic before talking to the database.
    

### 2. "The API is the Server."

- **Correction:** A server is physical or virtual hardware computing infrastructure (like an AWS cloud instance) running an operating system. An API is the software _application code_ that runs on that hardware, listening for incoming network traffic.
    

### 3. "REST is a Protocol."

- **Correction:** REST is not a protocol. It has no official software code specification or validation engine. It is an **architectural design style**. You can build an API that breaks REST rules, and it will still compile and run, but it won't be RESTful. Protocols (like HTTP) are strict sets of rules you _must_ follow for network communication to work at all.
    

### 4. "Endpoints are Functions."

- **Correction:** While an API endpoint maps to a specific function or controller route in your code, they are not the same thing. An endpoint is a network resource address (`GET /books/5`). It acts as a public entry point that handles web constraints, headers, and authentication tokens before passing data down to internal programming functions.
    

### 5. "CRUD maps 1-to-1 to HTTP Methods across all software systems."

- **Correction:** CRUD is a data persistence concept (Create, Read, Update, Delete). HTTP methods are network protocol communication actions. While REST maps them together closely for clean design, they are distinct concepts. You can design an application that reads data using a `POST` method if necessary, though it wouldn't follow standard REST guidelines.
    

## 14. Interview Questions

#### Q1: What does it mean for an API endpoint to be RESTful?

> **Answer:** An endpoint is RESTful when it conforms to the core design constraints of the REST architectural style. This includes maintaining a clear separation between client and server, operating statelessly, allowing responses to be cached, and using a uniform interface. A uniform interface means managing resources with nouns instead of verbs, using plural names for collections, and leveraging native HTTP methods (`GET`, `POST`, etc.) and status codes.

#### Q2: Why is it bad design to create an endpoint named `POST /deleteUser?id=5`?

> **Answer:** This endpoint violates two major REST design rules:
> 
> 1. It uses a verb (`/deleteUser`) inside the path instead of a noun resource.
>     
> 2. It uses the `POST` method to handle a deletion action.
>     
>     A clean RESTful design would represent this operation as `DELETE /users/5`, using the correct HTTP verb to define the action and keeping the path focused purely on the resource noun.
>     

#### Q3: What is the practical difference between PUT and PATCH methods?

> **Answer:** `PUT` completely replaces the target resource with the uploaded payload, overwriting any omitted fields with default values. `PATCH` applies a partial modification, updating only the specific properties included in the payload while preserving all other existing data fields.

#### Q4: How does statelessness impact horizontal scaling in a backend API infrastructure?

> **Answer:** Statelessness ensures that individual servers don't store user session data in local memory. Because every request contains all the information needed to identify and authorize the user, incoming requests can be safely routed to _any_ server instance behind a load balancer. This allows you to scale out by adding more servers without worrying about syncing user sessions across machines.

#### Q5: What is content negotiation in a Web API?

> **Answer:** Content negotiation is a mechanism where the client and server agree on the data format used for requests and responses. The client uses the `Accept` header to tell the server what format it wants to receive (e.g., `Accept: application/json`), and the server sets the `Content-Type` header in its response to confirm how the payload data is structured.

#### Q6: What response status code should an API return when a resource is successfully created?

> **Answer:** The API should return `201 Created`, along with a payload body containing the newly generated resource representation and its database ID.

#### Q7: What is the structural difference between path parameters and query parameters?

> **Answer:** Path parameters (`/users/:id`) are part of the hierarchical URL structure and identify a specific resource instance. Query parameters (`/users?status=active`) appear after a question mark and are used to sort, filter, or paginate a collection of resources without changing the base resource path.

#### Q8: What does a `405 Method Not Allowed` error code mean?

> **Answer:** It means the resource path URL exists on the server, but the requested HTTP method verb is not supported by that endpoint route. For example, sending a `DELETE` request to an endpoint that only allows `GET` requests will trigger a 405 error.

#### Q9: What problem does an API abstraction layer solve for frontend teams?

> **Answer:** It decouples the frontend team from the backend's internal implementation details. Frontend developers only need to understand the API documentation contract (endpoints, request payloads, and response structures). They can build the user interface independently without knowing how the backend code is written or how the database schemas are structured.

#### Q10: Why are GET requests categorized as "safe" methods?

> **Answer:** `GET` requests are safe because their standard operational intent is purely read-only. Fetching a resource representation should never modify database records, delete files, or alter the server state.

#### Q11: What is an API Gateway?

> **Answer:** An API Gateway is an architectural component that acts as a single point of entry for all incoming client requests. It handles cross-cutting concerns like authentication, request routing, rate limiting, and logging before passing requests down to internal backend services.

#### Q12: What does a `422 Unprocessable Entity` status code indicate?

> **Answer:** It indicates that the request syntax and formatting are correct, but the payload data violates business logic validation rules (e.g., an age field contains a negative number, or an email address is already taken in the system).

#### Q13: Can a REST API return data formats other than JSON?

> **Answer:** Yes. REST is independent of data formats. While JSON is the most common format on the modern web, a RESTful API can return XML, HTML, plain text, or binary media assets as long as the format is accurately described using the `Content-Type` header.

#### Q14: How does a client safely perform authorization checks in a stateless REST API?

> **Answer:** The client must include a secure authorization credential (such as a token) in the headers of every request. The API parses this token, extracts the user's identity, checks their permission scopes in the database, and either fulfills or rejects the request based on those access rules.

#### Q15: What is an idempotent method? Which CRUD methods are idempotent?

> **Answer:** A method is idempotent if executing an identical request multiple times yields the exact same server resource state as executing it a single time. `GET`, `PUT`, and `DELETE` are idempotent methods, while `POST` is non-idempotent because repeating it creates duplicate resource entries.

## 15. Knowledge Check

> [!tip] Practice Session
> 
> Try to answer these conceptual questions yourself to test your understanding of API and REST fundamentals.

1. What is the distinction between an API and an API endpoint?
    
2. Why is using verbs inside resource paths considered a bad design practice in REST?
    
3. Which HTTP status code class indicates a client-side validation error?
    
4. How do client applications maintain a user's logged-in state if the REST API is stateless?
    
5. Why should a frontend client application never talk directly to a SQL database?
    
6. Is REST a protocol or an architectural style? Explain the difference.
    
7. What does the `Content-Type` header tell the receiving application?
    
8. Why is a `POST` request classified as non-idempotent?
    
9. Explain the difference between route path parameters and query filtering parameters.
    
10. What response status code should be returned if a client attempts to access a valid endpoint without providing authentication credentials?
    
11. True or False: An API must always be connected to a database.
    
12. Why do REST design conventions recommend using plural nouns for resource paths?
    
13. What is the significance of a `204 No Content` status code in a `DELETE` request?
    
14. How does separating the client from the server improve system reliability?
    
15. What are the key concepts you must master before moving on to testing APIs with tools like Postman?
    

## 16. Summary

### Key Takeaways

- **Universal Middleware:** An API acts as a clear communication interface between separate software applications, shielding clients from internal code complexity.
    
- **Security & Abstraction:** Keeping clients decoupled from direct database access is essential for protecting system security, processing validation logic, and managing long-term codebase architecture.
    
- **REST as a Blueprint:** REST is an architectural design style that leverages native HTTP features to build scalable, uniform, and predictable web interfaces.
    
- **Resource-Centric Naming:** RESTful design organizes data models into distinct, noun-based resource paths using explicit plural structures (e.g., `/products/:id`).
    

### Important Terminology

- **API:** Application Programming Interface.
    
- **REST:** Representational State Transfer.
    
- **Resource:** Any data entity or object exposed by an API.
    
- **Endpoint:** A specific resource path combined with an HTTP method verb.
    
- **Statelessness:** A design pattern where the server retains no memory of past requests; each request must carry full context.
    
- **Idempotency:** A property where repeating an operation multiple times produces the exact same system state.

[[HTTP_HTTPS_Fundamentals]]
[[JSON]]