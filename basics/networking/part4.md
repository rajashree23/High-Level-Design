# REST (Representational State Transfer)

## 1. What is REST?

**REST is an architectural style for designing APIs.**

It is **not a protocol**.

REST APIs commonly use **HTTP** for communication and often use **JSON** for request/response data.

The core idea is:

> Model your system around **resources** and use HTTP methods to perform operations on those resources.

Example:

```http
GET /users/123
```

Here:

* `users` → resource
* `123` → resource ID
* `GET` → operation being performed

---

## 2. REST Resource-Oriented Design

In REST, we think primarily about **resources**, rather than actions/functions.

For example, instead of:

```http
GET /getUser
POST /createUser
POST /updateUser
```

we model the resource:

```http
GET    /users/123
POST   /users
PUT    /users/123
PATCH  /users/123
DELETE /users/123
```

Think:

```text
Resource + HTTP Method = Operation
```

---

## 3. Common HTTP Methods

| Method | Purpose                                            | Example             |
| ------ | -------------------------------------------------- | ------------------- |
| GET    | Retrieve resource                                  | `GET /users/123`    |
| POST   | Create resource / perform non-idempotent operation | `POST /users`       |
| PUT    | Replace/update resource                            | `PUT /users/123`    |
| PATCH  | Partially update resource                          | `PATCH /users/123`  |
| DELETE | Delete resource                                    | `DELETE /users/123` |

### Example

Create a user:

```http
POST /users

{
  "username": "john",
  "email": "john@example.com"
}
```

Get the user:

```http
GET /users/123
```

Update the user:

```http
PATCH /users/123

{
  "email": "new@example.com"
}
```

Delete the user:

```http
DELETE /users/123
```

---

## 4. REST Resources Often Map to Core Entities

In HLD, the **core entities** of the system often become API resources.

For example:

```text
Core Entities
    ↓
User
Order
Product
Review

REST Resources
    ↓
/users
/orders
/products
/reviews
```

This makes API design easier because the API structure reflects the domain model.

---

## 5. Nested Resources

Resources can be nested when there is a clear relationship between them.

Example:

```http
GET /users/123/posts
```

Meaning:

> Get the posts belonging to user `123`.

Another example:

```http
GET /orders/123/items
```

Meaning:

> Get the items belonging to order `123`.

Don't overuse nesting. Very deep URLs can become difficult to manage.

---

## 6. REST vs Action-Based APIs

A common mistake is designing an endpoint for every business operation.

For example:

```http
POST /startGame
POST /cancelOrder
POST /updateUser
```

A REST-oriented approach tries to model the **resource and its state**.

For example:

```http
PATCH /games/123

{
  "status": "started"
}
```

Instead of:

```http
POST /startGame
```

The idea is:

> Think about **resources and their state**, rather than exposing every function as an API endpoint.

However, this is a guideline, not an absolute rule. Real-world APIs sometimes use action-style endpoints when they make the API clearer.

---

## 7. JSON

REST APIs commonly use **JSON** for request and response bodies.

Example response:

```json
{
  "id": 123,
  "username": "john",
  "email": "john@example.com"
}
```

JSON is popular because it is:

* Human-readable
* Easy to work with
* Supported by almost every programming language

JSON is **not required by REST**. REST can use other representations as well.

---

## 8. REST and HTTP

REST and HTTP are different things.

```text
HTTP
 ↓
Communication protocol

REST
 ↓
Architectural style for designing APIs
```

A REST API commonly uses HTTP:

```text
Client
   ↓
HTTP Request
   ↓
REST API
   ↓
Application
   ↓
Database / Cache / Other Services
```

Not every HTTP API is necessarily RESTful.

---

## 9. REST and Idempotency

An operation is **idempotent** if repeating it produces the same final effect.

Typically:

```text
GET     → Idempotent
PUT     → Idempotent
DELETE  → Idempotent
POST    → Usually NOT idempotent
PATCH   → Depends on implementation
```

Why this matters in distributed systems:

If a client sends a request but doesn't receive the response, it may retry.

For example:

```text
Client → POST /payments
           ↓
        Payment succeeds
           ↓
     Response is lost
           ↓
Client doesn't know
           ↓
Client retries
           ↓
Potential duplicate payment
```

For operations such as payments/orders, we often use an **idempotency key** to make retries safe.

Example:

```http
POST /payments
Idempotency-Key: abc123
```

The server remembers the key and avoids processing the same request twice.

---

## 10. REST vs Other API Styles

For HLD interviews, know the basic positioning:

### REST

Good default choice for most APIs.

```text
Simple
Widely understood
HTTP-based
Resource-oriented
```

### GraphQL

Useful when clients need **flexible/custom data requirements**.

Example:

```text
Client asks for exactly the fields it needs.
```

### gRPC

Useful particularly for **service-to-service communication** where performance, efficient serialization, and strongly typed contracts are important.

### WebSockets / SSE

Useful when you need **real-time server → client updates**.

---

## 11. HLD Interview Default

Unless the problem has a specific requirement:

> **Default to REST for APIs.**

Reach for alternatives when the requirements justify them:

```text
REST
 ↓
Default API choice

GraphQL
 ↓
Flexible client-specific data fetching

gRPC
 ↓
High-performance service-to-service communication

WebSockets / SSE
 ↓
Real-time updates
```

---

## 12. Key Takeaways

Remember these for HLD interviews:

1. **REST is an architectural style, not a protocol.**
2. REST commonly uses **HTTP + JSON**.
3. REST is **resource-oriented**.
4. HTTP methods describe operations on resources.
5. Core entities often map naturally to API resources.
6. Nested resources can represent relationships.
7. Think **resource + state**, rather than creating an endpoint for every function.
8. Understand **idempotency**, especially for retries.
9. REST is usually the **default API choice** in HLD interviews.
10. Use GraphQL, gRPC, WebSockets, or SSE when the requirements specifically call for them.
