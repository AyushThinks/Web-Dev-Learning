# Angular Day 2 - Components 📦

---

# What is a Component?

A **Component** is the **building block** of every Angular application.

A component is a **reusable** and **self-contained** part of the User Interface (UI).

Each component contains:

- HTML (Template)
- CSS (Styles)
- TypeScript (Logic)

All these are grouped together into a single unit. :contentReference[oaicite:0]{index=0}

---

# Real World Examples

A website is made of many reusable components.

Examples:

- Header
- Navigation Bar
- Login Form
- Product Card
- Footer

Instead of writing the same code repeatedly, we create one component and reuse it wherever needed. :contentReference[oaicite:1]{index=1}

---

# Problem Without Components

Without Components:

```html
<div class="card">
    <h2>iPhone 16</h2>
    <button>Buy</button>
</div>

<div class="card">
    <h2>Samsung S25</h2>
    <button>Buy</button>
</div>
```

Problems:

- Duplicate Code
- Difficult Maintenance
- More Bugs
- Time Consuming

---

# Solution Using Components

```html
<product-card name="iPhone 16"></product-card>

<product-card name="Samsung S25"></product-card>
```

Advantages:

- One Component
- Multiple Uses
- Different Data
- Easy Maintenance

:contentReference[oaicite:2]{index=2}

---

# Advantages of Components

## 1. Code Reusability

Write once and use multiple times.

## 2. Easy Maintenance

Fix a bug once.

## 3. Better Organization

Application is divided into smaller parts.

## 4. Improved Readability

Small files are easier to understand.

## 5. Faster Development

Reuse existing components.

## 6. Consistency

Same design throughout the application.

## 7. Independent Development

Multiple developers can work simultaneously.

## 8. Easy Testing

Each component can be tested individually.

## 9. Scalability

Large applications become easier to manage.

## 10. Encapsulation

Each component manages its own:

- HTML
- CSS
- Logic

## 11. Reduced Duplication

Removes repeated code.

## 12. Better Performance

Only changed components are updated.

## 13. Easier Debugging

Problems can be isolated quickly.

## 14. Flexible Composition

Large pages are built using smaller components.

## 15. Better Collaboration

Developers work independently with fewer conflicts.

:contentReference[oaicite:3]{index=3}

---

# Real World Analogy

Think of a Car.

A Car contains:

- Engine
- Wheels
- Doors
- Steering
- Seats

If one wheel breaks, we replace only that wheel.

Similarly,

A Website contains reusable components such as:

- Header
- Sidebar
- Product List
- Shopping Cart
- Footer

:contentReference[oaicite:4]{index=4}

---

# Angular Component

An Angular Component consists of **3 Parts**

```text
Angular Component

├── Class
├── Template
└── Decorator
```

These three together create one Angular Component. :contentReference[oaicite:5]{index=5}

---

# 1. Class

The class stores:

- Properties
- Methods
- Data
- Logic

Example

```ts
export class AppComponent {

    name: string = "Angular";

}
```

Here

- `name` → Property
- `string` → Data Type
- `"Angular"` → Value
- `export` → Makes the class available outside the file

:contentReference[oaicite:6]{index=6}

---

# 2. Decorator

Decorator converts a normal TypeScript class into an Angular Component.

Angular uses

```ts
@Component
```

Import

```ts
import { Component } from "@angular/core";
```

:contentReference[oaicite:7]{index=7}

---

# 3. Template

Template defines what the user sees.

Example

```ts
template: `<h1>Hello {{ name }}</h1>`
```

Template contains

- HTML
- Angular Syntax
- Data Binding

:contentReference[oaicite:8]{index=8}

---

# Complete Angular Component

```ts
import { Component } from "@angular/core";

@Component({
    selector: "app-hello",
    template: `<h1>Hello {{ name }}</h1>`
})

export class AppComponent{

    name:string="Angular";

}
```

A complete Angular Component contains

- Class
- Decorator
- Template

:contentReference[oaicite:9]{index=9}

---

# Interpolation (Data Binding)

Interpolation uses

```html
{{ }}
```

Example

```html
<h1>Hello {{ name }}</h1>
```

Class

```ts
name = "Angular";
```

Output

```text
Hello Angular
```

If

```ts
name = "Ayush";
```

Output

```text
Hello Ayush
```

:contentReference[oaicite:10]{index=10}

---

# Selector

Selector is a custom HTML tag used to display a component.

Example

```ts
selector:"app-hello"
```

Usage

```html
<app-hello></app-hello>
```

Angular replaces

```html
<app-hello></app-hello>
```

with

```html
<h1>Hello Angular</h1>
```

during runtime.

:contentReference[oaicite:11]{index=11}

---

# Creating a Component

Command

```bash
ng generate component hello
```

Shortcut

```bash
ng g c hello
```

Generated Files

```text
hello/

├── hello.component.ts
├── hello.component.html
├── hello.component.scss
└── hello.component.spec.ts
```

Main logic resides in

```text
hello.component.ts
```

:contentReference[oaicite:12]{index=12}

---

# Running the Application

```bash
ng serve
```

Open

```text
http://localhost:4200
```

Output

```text
Hello Angular
```

:contentReference[oaicite:13]{index=13}

---

# Common Mistakes

## 1. Never use both together

❌ Wrong

```ts
template: `...`,
templateUrl: "./hello.component.html"
```

✅ Correct

Use either

```ts
template
```

OR

```ts
templateUrl
```

---

## 2. Never call the same component inside itself

❌ Wrong

```html
<app-hello></app-hello>
```

inside

```text
hello.component.html
```

This creates recursive rendering.

---

## 3. Root Component

Root Component selector is normally

```ts
selector:"app-root"
```

Custom selectors like

```ts
app-hello
```

are generally used for child components.

---

## 4. Route Path

❌ Wrong

```ts
path:"/hello"
```

✅ Correct

```ts
path:"hello"
```

Do not use `/` inside Angular route definitions.

---