# HTTP & REST APIs

This repository contains my learning notes on **HTTP, HTTPS, REST APIs, HTTP methods, status codes, and client-server communication**. These concepts form the foundation of backend development and web applications.

---

# 📖 What I Learned

## 🌐 HTTP

HTTP (HyperText Transfer Protocol) is the communication protocol used between a client (browser or app) and a server. It defines how requests and responses are exchanged over the internet.

---

## 💻 Client & Server

A client is the application or browser that sends requests, while the server processes those requests and returns a response. The client always starts the communication, and the server only responds.

---

## 🧠 Stateless Protocol

HTTP is a stateless protocol, which means the server does not remember previous requests. Every request must contain all the information needed to process it, often using cookies or JWT tokens.

---

## 🔒 HTTP vs HTTPS

HTTPS is the secure version of HTTP. It encrypts data using TLS encryption, making communication safe from attackers. HTTP uses port **80**, while HTTPS uses port **443**.

---

## 🍽 What is an API?

An API (Application Programming Interface) acts as a bridge between the client and the server. It allows different applications to communicate without directly accessing the database.

---

## 🔄 API Request Flow

Whenever a user performs an action, the request follows this flow:

```
Client → API → Server → Database
                 ↓
           JSON Response
                 ↓
              Client
```

The API receives the request, processes it, communicates with the database, and sends the response back.

---

## 🌍 API Examples

Many applications use APIs every day, such as:

- Google Maps for navigation
- Instagram for posts and likes
- WhatsApp for messaging
- Netflix for recommendations
- Spotify for music search
- Weather applications for forecasts
- Banking apps for UPI transactions

---

# REST APIs

## 📦 Resources

A resource is any object that the application manages, such as students, books, orders, or users. In REST APIs, every resource has its own URL.

Example:

```
/students
/students/99
/students/99/courses
```

REST URLs use nouns instead of verbs.

---

# CRUD Operations

CRUD represents the four basic database operations.

| Operation | Meaning | HTTP Method |
|-----------|---------|-------------|
| Create | Add new data | POST |
| Read | Retrieve data | GET |
| Update | Modify existing data | PUT / PATCH |
| Delete | Remove data | DELETE |

Almost every web application performs these CRUD operations.

---

# HTTP Methods

## GET

GET is used to retrieve data from the server. It does not modify any information and is mainly used for fetching records.

---

## POST

POST creates a new resource. The request contains data inside the request body, and the server generates a new record.

---

## PUT

PUT replaces an entire existing resource with new data. If any field is missing, it may overwrite the previous value.

---

## PATCH

PATCH updates only selected fields of a resource. It modifies specific information without replacing the entire object.

---

## DELETE

DELETE removes a resource from the server. After successful deletion, the server usually returns **204 No Content**.

---

# Safe & Idempotent Methods

Safe methods only read data without making changes.

Idempotent methods produce the same result even if the request is repeated multiple times.

Examples:

- GET → Safe & Idempotent
- PUT → Idempotent
- DELETE → Idempotent
- POST → Not Idempotent

---

# URL Anatomy

A URL contains different components that help identify the requested resource.

Example:

```
https://college.com:443/students/99?course=bca#marks
```

Parts include:

- Protocol
- Host
- Port
- Path
- Resource ID
- Query Parameters
- Fragment

---

# HTTP Request

Every HTTP request contains four main parts:

- Start Line
- Headers
- Blank Line
- Body

Headers provide additional information such as authentication and content type, while the body carries the actual data.

---

# Important Headers

### Content-Type

Specifies the format of the request body, such as JSON.

### Accept

Indicates the format expected in the response.

### Authorization

Contains authentication credentials such as JWT tokens.

### Cookie

Stores small pieces of information to identify users.

### Host

Identifies the destination server.

### User-Agent

Provides information about the browser or application making the request.

---

# HTTP Response

A server response contains:

- Status Line
- Response Headers
- Response Body

Most REST APIs send data in JSON format because it is lightweight and easy to understand.

---

# JSON

JSON (JavaScript Object Notation) is the standard format used for exchanging data between the client and server. It is simple, readable, and supported by almost every programming language.

---

# HTTP Status Codes

## 1xx – Informational

The request has been received and processing continues.

---

## 2xx – Success

The request completed successfully.

Common codes:

- 200 OK
- 201 Created
- 202 Accepted
- 204 No Content

---

## 3xx – Redirection

The requested resource has been moved to another location.

Examples:

- 301 Moved Permanently
- 302 Found
- 304 Not Modified

---

## 4xx – Client Errors

These errors occur because of problems in the client's request.

Examples:

- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 409 Conflict
- 422 Unprocessable Entity
- 429 Too Many Requests

---

## 5xx – Server Errors

These errors indicate problems on the server side.

Examples:

- 500 Internal Server Error
- 501 Not Implemented
- 502 Bad Gateway
- 503 Service Unavailable
- 504 Gateway Timeout

---

# REST API Naming Rules

REST APIs follow standard naming conventions.

Good practices include:

- Use plural nouns
- Keep URLs lowercase
- Avoid verbs in URLs
- Use meaningful resource names

Example:

```
/students
/orders
/products
```

---

# API Versioning

API versioning allows developers to release new features without breaking older applications.

Example:

```
/api/v1/students
/api/v2/students
```

---

# HTTP Versions

I learned about different versions of HTTP.

- HTTP/1.1 introduced persistent connections.
- HTTP/2 improved speed using multiplexing.
- HTTP/3 uses QUIC and provides faster communication over modern networks.

---

# Authentication

Authentication verifies the identity of users.

Common authentication methods include:

- Cookies
- Sessions
- JWT (JSON Web Tokens)

JWT is widely used because it supports stateless authentication.

---

# Performance Optimization

Several techniques improve API performance:

- Caching reduces repeated requests.
- Compression decreases response size.
- CDN delivers content from nearby servers.
- ETag helps avoid unnecessary downloads.

---

# Complete Request Journey

A browser request travels through several components before reaching the database.

```
Browser
   ↓
DNS
   ↓
Internet
   ↓
API Gateway
   ↓
Node.js Server
   ↓
Controller
   ↓
Service
   ↓
Database
   ↓
JSON Response
   ↓
Browser
```

This complete flow explains how modern web applications process user requests.

---

# Key Takeaways

- Learned how HTTP communication works.
- Understood the client-server architecture.
- Studied REST API principles.
- Learned CRUD operations and HTTP methods.
- Explored request and response structures.
- Understood status codes and their meanings.
- Learned REST API design best practices.
- Studied authentication using JWT and cookies.
- Explored performance optimization techniques like caching and CDN.

---

# Technologies Covered

- HTTP
- HTTPS
- REST APIs
- JSON
- Node.js (Concepts)
- Express.js (Basics)

---

# Conclusion

This learning helped me understand how web applications communicate over the internet. I gained practical knowledge of HTTP, REST APIs, request-response flow, authentication, status codes, and API design principles, which are essential for backend development.