# 🔐 Authentication & JWT Learning Notes

These are my personal notes on **Authentication**, **Password Hashing**, **JWT (JSON Web Tokens)**, and **npm Versioning** in a Node.js/Express application.

---

# 📚 Table of Contents

1. What is Authentication?
2. Why Hash Passwords?
3. What is JWT?
4. JWT Structure
5. How JWT Security Works
6. JWT Authentication Flow
7. npm Versioning (`~` vs `^`)
8. Key Takeaways

---

# 1. What is Authentication?

Authentication is the process of verifying **who a user is**.

When a user enters their credentials:

- Email
- Password

The server checks whether these credentials belong to a valid user.

If everything is correct, the server authenticates the user and allows access.

### Authentication Flow

```
User
   │
   │ Email + Password
   ▼
Server
   │
   │ Check Database
   ▼
Valid?
 │
 ├── No → Login Failed
 │
 └── Yes
       │
       ▼
 Generate JWT Token
       │
       ▼
Return Token to User
```

---

# 2. Why Hash Passwords?

Passwords should **never be stored as plain text** inside the database.

Instead, they are converted into a scrambled string using a hashing algorithm such as **bcrypt**.

Example:

```
Password:
hello123

Stored in Database:
$2b$10$KR0tMiO/l6JH51Bipc3...
```

This scrambled string is called a **Hash**.

## Important Properties of Hashing

- One-way process
- Cannot convert hash back to original password
- Same password always produces a secure hash (with bcrypt, salt is added to make hashes unique)

During Login:

```
User Password
      │
      ▼
Hash Password Again
      │
      ▼
Compare with Stored Hash
      │
      ▼
Match?
```

If both hashes match:

✅ Login Successful

Otherwise:

❌ Invalid Password

---

## Why is this secure?

Imagine someone steals your database.

Without hashing:

```
Email                  Password
-------------------------------
abc@gmail.com          hello123
xyz@gmail.com          password
```

With hashing:

```
Email                  Password
---------------------------------------------
abc@gmail.com          $2b$10$KR0tMi...
xyz@gmail.com          $2b$10$Xh83Lo...
```

The attacker only sees meaningless hashes.

They **cannot recover the original password**.

---

# 3. What is JWT?

JWT stands for:

> **JSON Web Token**

A JWT is a secure token used to identify users after they log in.

Instead of asking the user for their password on every request, the server gives them a JWT.

The client sends this JWT with every future request.

The server verifies the JWT and knows who the user is.

---

# 4. JWT Structure

A JWT looks like this:

```
xxxxx.yyyyy.zzzzz
```

It contains **three parts** separated by dots.

```
Header.Payload.Signature
```

---

## 1. Header

The header tells the server:

- Which algorithm was used
- What type of token it is

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

---

## 2. Payload

The payload contains user information.

Example:

```json
{
  "id": 15,
  "name": "Ayush",
  "email": "abc@gmail.com"
}
```

This information is called **Claims**.

### Common Claims

- user id
- email
- role
- username

The middleware reads the payload to determine **who is making the request**.

---

## ⚠️ Very Important Rule

The payload is **NOT encrypted**.

It is only **Base64 encoded**.

That means anyone holding the token can decode it and read the payload.

Therefore:

❌ Never store:

- Password
- OTP
- Bank Details
- API Keys
- Secret Information

Only store safe user identification data.

---

## 3. Signature

The signature is the most important part of JWT.

It is created using:

```
Header
+
Payload
+
JWT_SECRET
```

The JWT_SECRET is stored securely in the `.env` file.

Example:

```
JWT_SECRET=mySuperSecretKey
```

The client never sees this secret.

---

# 5. How JWT Security Works

A common question:

"If the payload is readable, why can't someone simply change it?"

Example:

Original Payload

```json
{
  "id": 15
}
```

A hacker changes it to:

```json
{
  "id": 1
}
```

Now the payload has changed.

However...

The signature was created using the original payload.

Changing even **one character** makes the signature invalid.

When the server executes:

```javascript
jwt.verify(token, JWT_SECRET);
```

The server recalculates the signature.

If both signatures don't match:

```
Invalid Signature
```

The request is rejected immediately.

---

## JWT is:

✔ Readable

❌ Not Editable

✔ Tamper Proof

---

# 6. JWT Authentication Flow

```
        User Login
             │
             ▼
Enter Email & Password
             │
             ▼
Server Checks Database
             │
             ▼
Password Correct?
      │
      ├── No
      │     ▼
      │  Login Failed
      │
      └── Yes
             │
             ▼
      Generate JWT
             │
             ▼
 Send JWT to Client
             │
             ▼
Client Stores Token
(LocalStorage/Cookies)
             │
             ▼
Future Requests
Authorization:
Bearer <token>
             │
             ▼
Server verifies JWT
             │
             ▼
Valid?
      │
      ├── No
      │     ▼
      │ Unauthorized
      │
      └── Yes
             │
             ▼
Access Granted
```

---

# 7. Understanding npm Versioning (`~` vs `^`)

Node.js packages follow **Semantic Versioning (SemVer)**.

Version format:

```
MAJOR.MINOR.PATCH
```

Example:

```
4.17.21
```

Where:

- **MAJOR** → Breaking changes
- **MINOR** → New features
- **PATCH** → Bug fixes

---

## Exact Version

```json
"express": "4.17.21"
```

Only this version is installed.

No updates.

---

## Tilde (`~`)

```json
"express": "~4.17.21"
```

Allows only **Patch** updates.

Example:

```
✅ 4.17.22
✅ 4.17.25

❌ 4.18.0
❌ 5.0.0
```

Used when you only want bug fixes.

---

## Caret (`^`)

```json
"express": "^4.17.21"
```

Allows:

- Patch updates
- Minor updates

Example:

```
✅ 4.17.22
✅ 4.18.0
✅ 4.20.3

❌ 5.0.0
```

This is the default used by npm.

It allows improvements without introducing breaking changes.

---

# Version Update Summary

| Symbol | Patch | Minor | Major |
|---------|:-----:|:-----:|:-----:|
| None | ❌ | ❌ | ❌ |
| `~` | ✅ | ❌ | ❌ |
| `^` | ✅ | ✅ | ❌ |

---

# 8. Key Takeaways

## Authentication

- Verifies user identity.
- Successful login returns a JWT.

---

## Password Hashing

- Passwords are never stored directly.
- bcrypt creates one-way hashes.
- Hashes cannot be reversed.

---

## JWT

- JWT stands for JSON Web Token.
- Used for authentication after login.
- Contains Header, Payload, and Signature.

---

## Payload

- Readable by anyone with the token.
- Store only non-sensitive information.
- Never store passwords or secrets.

---

## Signature

- Protects the token from modification.
- Generated using `JWT_SECRET`.
- Any change to the payload invalidates the signature.

---

## npm Versioning

- **Exact Version** → No updates.
- **`~`** → Patch updates only.
- **`^`** → Minor + Patch updates.
- Major updates require manual installation.

---