# 🚀 Angular - Day 1

---

# 📚 Table of Contents

- Introduction to Angular
- Why Angular?
- Performance
- Mobile Support
- Language Choices
- What is ECMAScript?
- What is TypeScript?
- Installing Angular
- Angular CLI
- Common CLI Commands
- Quick Revision
- Interview Questions
- Homework

---

# 📖 Introduction to Angular

Angular is one of the most popular JavaScript frameworks developed and maintained by **Google**.

It is used to build:

- Dynamic Web Applications
- Single Page Applications (SPA)
- Enterprise-level Applications

Angular focuses on:

- Performance
- Code Organization
- Reusability
- Scalability
- Easy Maintenance

---

# ❓ Why Should We Use Angular?

Angular provides many improvements over plain JavaScript.

## 🚀 1. Better Performance

Angular provides:

- Faster initial loading
- Efficient Change Detection
- Faster Rendering
- Better User Experience

It also includes:

- Modularity
- Dependency Injection
- Easy Testing

Modern Angular versions are optimized to build smaller and faster applications.

---

## 📱 2. Mobile Support

Angular is built with mobile development in mind.

Advantages:

- Responsive Applications
- Single Codebase
- Works on Desktop and Mobile
- No extra third-party tools required

---

## 💻 3. Language Choices

Angular supports multiple languages:

- JavaScript
- Modern ECMAScript
- TypeScript ✅ (Recommended)

Angular itself is written using **TypeScript**.

---

# 📘 What is ECMAScript?

ECMAScript is the official specification (standard) of JavaScript.

Every new version of JavaScript follows ECMAScript standards.

Example:

- ES6 = ES2015

ES2015 introduced:

- Classes
- Modules
- Arrow Functions

Today, browsers directly support these modern features.

### ✅ One-Line Definition

> ECMAScript is the official standard specification of JavaScript.

---

# 📗 What is TypeScript?

TypeScript is:

- Free
- Open Source
- Developed by Microsoft

It is a **superset of JavaScript**.

This means:

Every JavaScript program is valid TypeScript.

TypeScript adds extra features such as:

- Static Types
- Classes
- Interfaces
- Inheritance

TypeScript code is compiled into JavaScript before running in browsers.

### Benefits

- Better Error Detection
- Better Code Maintenance
- Easier Refactoring
- Object-Oriented Programming Support

### ✅ One-Line Definition

> TypeScript is JavaScript with Static Typing and OOP features.

---

# ⚙️ Installing Angular

Angular installation consists of **3 Steps**.

---

## Step 1 — Install Node.js

Download the **LTS Version**.

Angular requires:

- Node.js v20.19 or later

Verify Installation:

```bash
node -v
npm -v
```

npm is automatically installed with Node.js.

---

## Step 2 — Install Angular CLI

Install Angular CLI globally.

```bash
npm install -g @angular/cli
```

Verify Installation

```bash
ng version
```

If Angular version appears, installation is successful.

---

## Step 3 — Create First Angular Project

Create Project

```bash
ng new my-first-app
```

Move Inside Project

```bash
cd my-first-app
```

Run Project

```bash
ng serve
```

Open Browser

```
http://localhost:4200
```

If everything is installed correctly, your Angular application will start successfully.

---

# 🛠 What is Angular CLI?

CLI stands for:

> Command Line Interface

Angular CLI is Angular's official command-line tool.

It automates:

- Project Creation
- Component Generation
- Running Applications
- Building Projects
- Testing

Instead of manually creating folders and configuration files, Angular CLI generates everything automatically.

---

# 📌 Common Angular CLI Commands

## Create New Project

```bash
ng new project-name
```

---

## Run Development Server

```bash
ng serve
```

---

## Generate Component

```bash
ng generate component component-name
```

Shortcut

```bash
ng g c component-name
```

---

## Build Application

```bash
ng build
```

---

## Run Tests

```bash
ng test
```

---

## Check Version

```bash
ng version
```

---

# ⭐ Why Angular CLI is Important

Angular CLI:

- Saves Time
- Creates Standard Folder Structure
- Generates Boilerplate Code
- Builds Production Applications
- Increases Developer Productivity

---

# 📊 Angular vs JavaScript

| JavaScript | Angular |
|------------|----------|
| Programming Language | Frontend Framework |
| Manual Project Setup | Automatic Project Setup |
| No Built-in Structure | Organized Structure |
| Less Scalable | Highly Scalable |
| More Manual Coding | Many Built-in Features |

---


---

# ✅ Key Takeaways

- Angular is a frontend framework developed by Google.
- Angular applications are written primarily in TypeScript.
- ECMAScript is the official JavaScript standard.
- Angular CLI automates project creation and development.
- Node.js and npm must be installed before Angular.
- Angular supports fast, scalable, and maintainable web application development.