# JavaScript APIs & Fetch — Notes

A concise reference on how APIs work, REST conventions, and the Fetch API — written while learning backend fundamentals as part of my SDE prep.

## Table of Contents

- [What is an API?](#what-is-an-api)
- [Client & Server](#client--server)
- [Request & Response](#request--response)
- [JSON](#json)
- [HTTP Methods](#http-methods)
- [GET vs POST](#get-vs-post)
- [HTTP Status Codes](#http-status-codes)
- [REST API Design](#rest-api-design)
- [Using the Fetch API](#using-the-fetch-api)
- [Sending Data with Fetch](#sending-data-with-fetch)
- [Error Handling](#error-handling)
- [Authentication Basics](#authentication-basics)
- [Express Basics](#express-basics)
- [Live Demo — PokeAPI](#live-demo--pokeapi)
- [Key Takeaways](#key-takeaways)

---

## What is an API?

An **API** (Application Programming Interface) is the bridge between a client and a server.

A simple restaurant analogy:

| Real World | API World |
|---|---|
| Customer | Client |
| Waiter | API |
| Kitchen | Server |

The client sends a request, the API delivers it to the server, and the server sends the response back through the API.

---

## Client & Server

A **client** can be:

- A browser
- A mobile app
- A desktop app

The **server** stores and manages data.

Key points:

- The client always sends the first request.
- One request receives exactly one response.
- Communication happens over HTTP.

---

## Request & Response

### Request

A request contains:

- HTTP method
- URL
- Headers
- Body *(optional)*

```http
GET /users
```

### Response

A response contains:

- Status code
- Headers
- Body

> Always check the **status code** first, before trusting the response body.

---

## JSON

**JSON** (JavaScript Object Notation) is the standard format for exchanging data between client and server.

Convert JSON → JavaScript object:

```javascript
const data = await response.json();
```

Convert JavaScript object → JSON:

```javascript
JSON.stringify(user);
```

---

## HTTP Methods

| Method | Purpose | Example |
|---|---|---|
| `GET` | Retrieve data | `GET /users` |
| `POST` | Create new data | `POST /users` |
| `PUT` | Replace an entire resource | `PUT /users/1` |
| `PATCH` | Update specific fields | `PATCH /users/1` |
| `DELETE` | Remove data | `DELETE /users/1` |

---

## GET vs POST

**GET**
- Retrieves data
- Data travels via URL parameters (`/users?id=10`)
- Can be bookmarked
- Safe to repeat

**POST**
- Creates new data
- Data travels in the request body
- Cannot be bookmarked
- Not safe to repeat — resubmitting can create duplicate records

---

## HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

**Status code groups:**

- `1xx` → Informational
- `2xx` → Success
- `3xx` → Redirection
- `4xx` → Client error
- `5xx` → Server error

---

## REST API Design

REST follows one simple convention: **URLs are nouns, HTTP methods are verbs.**

```text
GET    /users
POST   /users
PUT    /users/1
PATCH  /users/1
DELETE /users/1
```

---

## Using the Fetch API

### With `.then()`

```javascript
fetch(url)
  .then(res => res.json())
  .then(data => console.log(data));
```

### With `async/await`

```javascript
const response = await fetch(url);
const data = await response.json();
```

Why two `await`s?

1. The first waits for the server's response.
2. The second parses that response body into a JavaScript object.

---

## Sending Data with Fetch

```javascript
fetch(url, {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(user)
});
```

Key parts of a request: **method**, **headers**, **body**.

---

## Error Handling

`fetch()` only throws for **network failures** — it does *not* throw on 404 or 500 responses. Those must be checked manually:

```javascript
if (!response.ok) {
  throw new Error("Request Failed");
}
```

> Never assume success just because `fetch()` resolved.

---

## Authentication Basics

Most real-world APIs require authentication.

Common methods:

- API keys
- Bearer tokens

**Best practices:**

- Store secrets in a `.env` file
- Never commit secrets to GitHub
- Never expose secret keys in frontend code

---

## Express Basics

```javascript
app.get();
app.post();
res.status().json();
```

Express is the most common framework for building backend APIs in Node.js.

---

## Live Demo — PokeAPI

I explored the [PokeAPI](https://pokeapi.co/) directly in the browser console to practice:

- Making live API requests
- Reading and parsing JSON responses
- Working with real-world API data

---

## Key Takeaways

- APIs let clients and servers communicate.
- Every request gets exactly one response.
- Always check the status code before trusting the response body.
- JSON is the standard data-exchange format.
- `GET` retrieves data; `POST` creates it.
- REST URLs are nouns; HTTP methods are verbs.
- `fetch()` supports both `.then()` and `async/await`.
- Always check `response.ok` — `fetch()` won't throw on 404/500.
- Store keys and tokens in `.env`; never commit secrets to GitHub.

**Author:** Ayush Joshi 