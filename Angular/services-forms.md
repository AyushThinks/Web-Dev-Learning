# Angular · Day 3 (3 of 3) — Services, Data Sharing & Forms

---

## 1. What is a service?

A plain class that holds logic or data you want to **reuse across components**. API calls, logged-in user state, a shared calculation — belongs in a service, not repeated in every component.

> **In one line:** components are for the view, services are for the shared work. Keep components light, put reusable logic in services.

---

## 2. Making a service

A class marked with `@Injectable`. CLI: `ng generate service data`

```ts
// data.service.ts
import { Injectable } from "@angular/core";

@Injectable({ providedIn: "root" })
export class DataService {
  private message: string = "Hello from the service";

  getMessage(): string { return this.message; }
  setMessage(newMessage: string): void { this.message = newMessage; }
}
```

`@Injectable` marks the class as injectable. `providedIn: "root"` makes it available across the whole app.

---

## 3. Dependency Injection (DI) — how a component gets a service

A component doesn't create the service itself — it asks for it in the constructor, and Angular hands it over.

```ts
// hello.component.ts
import { Component, OnInit } from "@angular/core";
import { DataService } from "./data.service";

@Component({ selector: "app-hello", template: `<p>{{ text }}</p>` })
export class HelloComponent implements OnInit {
  text: string = "";

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.text = this.dataService.getMessage();
  }
}
```

`private dataService: DataService` in the constructor says "I need a `DataService`." Angular creates it (or reuses the existing one) and passes it in. **Never write `new DataService()` yourself.**

---

## 4. Singleton and data sharing

When a service is `providedIn: "root"`, Angular makes **one instance** for the whole app and gives that same one to everybody. This is a **singleton**.

Since everyone shares the same instance, if one component changes a value, every other component sees it — this is how two unrelated components share data:

```ts
// Component A writes:
this.dataService.setMessage("Updated by A");

// Component B reads:
this.text = this.dataService.getMessage(); // "Updated by A"
```

### When to use what

| Situation | Tool |
|---|---|
| Directly connected parent ↔ child | `@Input` / `@Output` |
| Components anywhere in the app | A shared service |

---

## 5. Forms: two ways to handle input

Almost every app has forms: login, signup, search. Angular gives two approaches:

| | Template-driven | Reactive |
|---|---|---|
| **Where logic lives** | Mostly in the HTML template | Mostly in the TypeScript class |
| **Module needed** | `FormsModule` | `ReactiveFormsModule` |
| **Best for** | Simple, small forms | Bigger forms, complex validation |
| **Feel** | Quick and easy | More control, more code |

Both are valid. Learn template-driven first (simpler), then reactive (what most professional projects use).

---

## 6. Template-driven forms

Built in the HTML using `ngModel` for two-way binding. Class stays almost empty. Needs `FormsModule`.

```html
<!-- the template -->
<form (ngSubmit)="onSubmit()">
  <input name="username" [(ngModel)]="username" placeholder="Name" />
  <input name="email" [(ngModel)]="email" placeholder="Email" />
  <button type="submit">Submit</button>
</form>
```

```ts
// the class
username: string = "";
email: string = "";

onSubmit() {
  console.log(this.username, this.email);
}
```

Each input ties to a class property with `[(ngModel)]`. On submit, `onSubmit()` runs and the values are already in the class. Simple and fast for small forms.

---

## 7. Reactive forms

Built in the class using `FormGroup` and `FormControl`. Template just connects to it. More control — helps for bigger forms and validation. Needs `ReactiveFormsModule`.

```ts
// the class
import { Component } from "@angular/core";
import { FormGroup, FormControl, Validators } from "@angular/forms";

@Component({ /* ... */ })
export class SignupComponent {
  signupForm = new FormGroup({
    username: new FormControl("", Validators.required),
    email: new FormControl("", [Validators.required, Validators.email]),
  });

  onSubmit() {
    console.log(this.signupForm.value);
  }
}
```

```html
<!-- the template -->
<form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
  <input formControlName="username" placeholder="Name" />
  <input formControlName="email" placeholder="Email" />
  <button type="submit" [disabled]="signupForm.invalid">Submit</button>
</form>
```

The form is defined in the class as a `FormGroup` of `FormControl`s. The template links via `[formGroup]` and `formControlName`. Validation is built in — `Validators.required`, `Validators.email` — and the button disables itself while the form is invalid.

---

## 8. Validation, in short

| Validator | Rule |
|---|---|
| `Validators.required` | Field cannot be empty |
| `Validators.email` | Must be a valid email |
| `Validators.minLength(6)` | At least 6 characters |
| `Validators.maxLength(20)` | At most 20 characters |

Check form state anytime:
- `signupForm.valid` / `signupForm.invalid`
- Single field: `signupForm.get('email')?.invalid`

---

## 9. Key takeaways

- **Service:** a class for shared logic/data. `@Injectable` with `providedIn: "root"`
- **DI:** ask for a service in the constructor, Angular provides it. Never write `new`
- **Singleton:** one shared instance for the whole app — this is how components share data through a service
- **Template-driven forms:** built in HTML with `ngModel`, needs `FormsModule`. Best for small forms
- **Reactive forms:** built in the class with `FormGroup`/`FormControl`, needs `ReactiveFormsModule`. Best for bigger forms and validation

---