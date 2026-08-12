# Angular · Day 3 (2 of 3) — Binding, Directives & Pipes

---

## 1. Data Binding — the four kinds

Binding connects your **class (data)** and your **template (view)**. Brackets tell you the direction.

### 1. Interpolation — `{{ }}`
Shows a class value in the page. **Class → View, read only.**

```html
<h1>Hello {{ name }}</h1>
<!-- class: name = "Angular"; -> shows "Hello Angular" -->
```

### 2. Property binding — `[ ]`
Sets an element's property from a class value. **Class → View.**

```html
<img [src]="imageUrl" />
<button [disabled]="isBusy">Save</button>
```

Difference: `{{ }}` puts text *between* tags, `[ ]` sets a *property on* the tag.

### 3. Event binding — `( )`
Runs a class method on an event. **View → Class.**

```html
<button (click)="save()">Save</button>
```
```ts
save() { console.log("saved!"); }
```

### 4. Two-way binding — `[( )]`
Connects an input to a class property **both ways**. Nicknamed "banana in a box".

```html
<input [(ngModel)]="username" />
<p>You typed: {{ username }}</p>
```

> ⚠️ Needs `FormsModule`. Older module project → add to module's `imports`. Standalone component → add `FormsModule` to the component's `imports`.

### Summary

| Syntax | Direction | Purpose |
|---|---|---|
| `{{ }}` | Class → View | Show a value |
| `[ ]` | Class → View | Set a property |
| `( )` | View → Class | Handle an event |
| `[( )]` | Both ways | Two-way binding (`ngModel`) |

`[ ]` = data in · `( )` = event out · `[( )]` = both together

---

## 2. Directives — `*ngIf`, `*ngFor`, `ngSwitch`

Special instructions on an element that change its behavior.

### `*ngIf` — show or hide
If false, the element isn't on the page at all (not just hidden).

```html
<p *ngIf="isLoggedIn">Welcome back!</p>
<p *ngIf="!isLoggedIn">Please log in.</p>
```

### `*ngFor` — loop over a list
Repeats an element once per array item.

```html
<ul>
  <li *ngFor="let fruit of fruits">{{ fruit }}</li>
</ul>
```
```ts
// class: fruits = ["Apple", "Mango", "Banana"];
```
Three fruits → three `<li>`s. Change the array → list updates.

### `ngSwitch` — pick one of many
Like a switch statement in the template.

```html
<div [ngSwitch]="role">
  <p *ngSwitchCase="'admin'">You are an admin</p>
  <p *ngSwitchCase="'user'">You are a user</p>
  <p *ngSwitchDefault>Unknown role</p>
</div>
```

If `role` is `"admin"` → only the first line shows. No match → default shows.

---

## 3. Pipes — format a value for display

A pipe transforms a value in the template **without changing the original data**. Used with `|`.

```html
<p>{{ name | uppercase }}</p>            <!-- ANGULAR -->
<p>{{ price | currency:'INR' }}</p>      <!-- Rs 500.00 -->
<p>{{ today | date:'longDate' }}</p>     <!-- August 5, 2026 -->
```

**Built-in pipes:** `uppercase`, `lowercase`, `titlecase`, `date`, `currency`, `number`, `percent`, `json`, `slice`

**Chaining:**
```html
{{ name | slice:0:5 | uppercase }}
```

---

## 4. Custom Pipes — make your own

A pipe is a class with `@Pipe` decorator + a `transform` method.

```ts
// double.pipe.ts
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({ name: "double" })
export class DoublePipe implements PipeTransform {
  transform(value: number): number {
    return value * 2;
  }
}
```

```html
<!-- using it -->
<p>{{ 10 | double }}</p> <!-- shows 20 -->
```

`transform` receives the value on the left of `|`, does its work, returns the result.

> CLI shortcut: `ng generate pipe double`

### Pipes with arguments

```ts
@Pipe({ name: "multiply" })
export class MultiplyPipe implements PipeTransform {
  transform(value: number, times: number): number {
    return value * times;
  }
}
```

```html
<p>{{ 10 | multiply:3 }}</p> <!-- shows 30 -->
```

---

## 5. Key takeaways

- Binding, four kinds: `{{ }}` show a value · `[ ]` set a property · `( )` handle an event · `[( )]` two-way with `ngModel`
- `*ngIf` shows/hides · `*ngFor` loops a list · `ngSwitch` picks one block of many
- Pipes format a value for display with `|`, without changing the data
- Custom pipe = class with `@Pipe` + `transform` method returning the changed value

---

