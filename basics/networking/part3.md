# HTTP / HTTPS

## 1. HTTP

**HTTP (HyperText Transfer Protocol)** is an application-layer, request-response protocol used for communication between clients and servers.

```text
Client → HTTP Request → Server
Client ← HTTP Response ← Server
```

HTTP is **stateless**: each request is independent. HTTP itself does not remember previous requests.

> Stateless HTTP does not mean the entire application must be stateless. Applications can maintain state using sessions, cookies, tokens, databases, etc.

---

## 2. HTTP Request

An HTTP request typically contains:

* **Method** — what operation is being requested
* **URL / Path** — which resource
* **Headers** — metadata
* **Body** — data sent to the server, when needed

Example:

```http
POST /users HTTP/1.1
Content-Type: application/json

{
  "name": "Rahul"
}
```

### Common HTTP Methods

| Method | Purpose                   | Idempotent?                                                  |
| ------ | ------------------------- | ------------------------------------------------------------ |
| GET    | Retrieve data             | Yes                                                          |
| POST   | Create/process data       | Usually no                                                   |
| PUT    | Replace/update resource   | Yes                                                          |
| PATCH  | Partially update resource | Usually treated as idempotent, but depends on implementation |
| DELETE | Delete resource           | Yes                                                          |

### Idempotency

An operation is **idempotent** if performing it multiple times has the same final effect as performing it once.

Example:

```text
PUT /users/10
name = Rahul
```

Sending it 5 times still leaves the user with `name = Rahul`.

This matters because network failures can cause clients to retry requests.

---

## 3. HTTP Response

A response typically contains:

* **Status code**
* **Headers**
* **Body**

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 10,
  "name": "Rahul"
}
```

### Common Status Codes

#### 2xx — Success

* `200 OK` — request successful
* `201 Created` — resource created

#### 3xx — Redirection

* `301 Moved Permanently`
* `302 Found`

#### 4xx — Client/request error

* `400 Bad Request`
* `401 Unauthorized` — authentication is missing/invalid
* `403 Forbidden` — authenticated but not allowed
* `404 Not Found`
* `429 Too Many Requests` — rate limited

#### 5xx — Server/upstream error

* `500 Internal Server Error`
* `502 Bad Gateway`
* `503 Service Unavailable`

---

# 4. HTTPS

**HTTPS = HTTP + TLS**

TLS provides:

* Encryption
* Integrity protection
* Server authentication

```text
HTTP:
Client ───── readable HTTP ─────> Server

HTTPS:
Client ───── encrypted data ────> Server
```

HTTPS protects data **in transit**, but it does not make the request automatically trustworthy.

The server still needs to perform:

* Input validation
* Authentication
* Authorization
* Business-rule validation
* Rate limiting

---

# 5. Authentication vs Authorization

### Authentication

**Who are you?**

Examples:

* Password
* OTP
* API key
* Access token
* JWT

### Authorization

**What are you allowed to do?**

Example:

```text
User is authenticated
        ↓
Is this user allowed to delete this order?
        ↓
Yes / No
```

Simple mental model:

```text
Authentication → Identity
Authorization  → Permissions
```

---

# 6. Persistent Connections

Opening a new connection for every HTTP request adds overhead.

With a persistent connection, multiple requests can reuse the same connection.

```text
Connection
   ├── Request 1 → Response 1
   ├── Request 2 → Response 2
   └── Request 3 → Response 3
```

Benefits:

* Lower latency
* Less connection setup overhead
* Better performance

HTTP/1.1 supports persistent connections.

---

# 7. HTTP Caching

HTTP caching allows browsers, CDNs, and other HTTP-aware caches to store responses and reuse them.

The server controls caching behavior using HTTP headers.

Example:

```http
Cache-Control: public, max-age=300
```

Meaning:

> This response can be cached for 300 seconds.

Architecture:

```text
Client → CDN → API → DB
          │
          └── cached response
```

On a cache hit:

```text
Client → CDN → Cached Response
```

The API may not receive the request at all.

### Important point

`Cache-Control` **does not create a cache**.

It provides instructions to an existing HTTP-aware cache such as:

* Browser
* CDN
* Reverse proxy

---

# 8. Redis Cache vs HTTP Cache

These are different layers.

### HTTP cache

```text
Client → CDN/Browser → API
```

Caches an **HTTP response**.

### Application cache

```text
Client → API → Redis → DB
```

Caches **application data/query results**.

Redis caching is explicitly implemented by the application.

---

# 9. Idempotency Keys

Useful when a request can be retried but should not be processed twice.

Example:

```http
POST /payments
Idempotency-Key: abc123
```

First request:

```text
abc123 → Process payment → Store result
```

Retry:

```text
abc123 → Already processed → Return previous result
```

Useful for:

* Payments
* Orders
* Bookings
* Other operations where duplicate processing is dangerous

---

# 10. HTTP Versions — High Level

| Version  | Key idea                                         |
| -------- | ------------------------------------------------ |
| HTTP/1.1 | Persistent connections                           |
| HTTP/2   | Multiplexing, binary framing, header compression |
| HTTP/3   | Uses QUIC over UDP                               |

You don't need protocol-level details for HLD initially.

---

# 11. HTTP vs HTTPS

```text
HTTP
 ↓
Application-layer communication

HTTPS
 ↓
HTTP
 +
TLS encryption/security
```

HTTPS should be the default for modern web/API communication.

---

# HLD Mental Model

```text
Client
  │
  │ HTTPS
  ▼
Browser / CDN Cache
  │
  │ Cache miss
  ▼
Load Balancer
  │
  ▼
API Server
  │
  ├── Redis Cache
  │
  └── Database
```

Key things to remember for HLD:

* HTTP = request/response protocol
* HTTP is stateless
* HTTPS = HTTP + TLS
* Methods define the requested operation
* Status codes communicate the result
* Persistent connections reduce connection overhead
* HTTP caching reduces repeated requests to the backend
* Redis caching reduces repeated database work
* Idempotency matters when requests can be retried
* Idempotency keys prevent duplicate processing
* Authentication identifies the caller
* Authorization determines what the caller can do
