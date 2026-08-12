# Angular · Day 2 — Modules and Component Communication

---

## 1. What is an Angular Module (`NgModule`)?

A module is Angular's original way to **group related components, directives, and pipes** together and declare what they need to work.

Think of it as a labeled folder: *"these pieces belong together, here's what they share, here's what they expose to the rest of the app."*

Example: a shopping app might have `UserModule`, `ProductModule`, `CartModule`.

### What a module holds

| Property | Meaning |
|---|---|
| `declarations` | Components, directives, pipes that belong to this module |
| `imports` | Other modules this one needs to borrow features from |
| `exports` | Pieces this module makes available to other modules |
| `providers` | Services this module shares |

---

## 2. Why modules exist

Angular needed a way to know which components exist, what they depend on, and how to load them efficiently.

- **Organisation** – group related features, avoid one giant pile of code
- **Reusability** – a well-made module can be reused across projects
- **Separation of concerns** – each feature lives in its own module
- **Lazy loading** – load a feature module only when the user visits it → faster app start
- **Shared setup** – common services/components declared in one place

---

## 3. How a module looks

Marked with the `@NgModule` decorator (same idea as `@Component`):

```ts
// app.module.ts — the older module style
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { AppComponent } from "./app.component";

@NgModule({
  declarations: [AppComponent], // components in this module
  imports: [BrowserModule],     // modules it needs
  bootstrap: [AppComponent],    // root component to start with
})
export class AppModule {}
```

In older Angular projects, `app.module.ts` is the file that runs first — it tells Angular which components exist and which one to bootstrap.

---

## 4. Important: Modules are now optional

Modern Angular has moved to **standalone components** — each component declares what it needs on its own. No module file required.

- Since **Angular 19**, `ng new` projects are standalone by default and often have **no module file**.

| | Status |
|---|---|
| Still supported | ✅ Yes — older apps run fine, you'll meet modules in real jobs |
| Default for new code | ❌ No — standalone components are the modern default |
| Still useful | ✅ Mainly for older third-party libraries built as modules |

**Takeaway:** Learn modules to read/maintain existing Angular codebases. Use standalone for new work.

---

## 5. Component Communication: Parent ↔ Child

When one component's tag sits inside another's template → outer = **parent**, inner = **child**.

| Direction | Tool |
|---|---|
| Parent → Child | `@Input` |
| Child → Parent | `@Output` |

---

## 6. Sending data down — `@Input`

**Child** marks a property with `@Input()` — "a parent may set this":

```ts
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-child",
  template: `<h3>Hello {{ name }}</h3>`,
})
export class ChildComponent {
  @Input() name: string = "";
}
```

**Parent** passes a value using square brackets:

```ts
template: `<app-child [name]="userName"></app-child>`

// in the class:
userName: string = "Priya";
```

Result: child renders `Hello Priya`. `[name]` = *bind to a value*, not literal text.

---

## 7. Sending news up — `@Output`

**Child** raises an event via `@Output` + `EventEmitter`:

```ts
import { Component, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "app-child",
  template: `<button (click)="sendHello()">Say hello</button>`,
})
export class ChildComponent {
  @Output() helloEvent = new EventEmitter<string>();

  sendHello() {
    this.helloEvent.emit("Hello from the child!");
  }
}
```

**Parent** listens:

```ts
template: `<app-child (helloEvent)="onHello($event)"></app-child>`

onHello(text: string) {
  this.message = text; // "Hello from the child!"
}
```

`$event` carries whatever the child emitted.

---

## 8. Brackets cheat sheet

| Syntax | Direction | Meaning |
|---|---|---|
| `[name]="value"` | Parent → Child | Pass data down into an `@Input` |
| `(event)="handler()"` | Child → Parent | Listen for an `@Output` event coming up |

**Memory trick:** `[ ]` = box you put data **in** · `( )` = ear **listening** for an event out.

---

## 9. Key takeaways

- `NgModule` groups related components + declares dependencies. Purpose: organisation, reusability, separation, lazy loading.
- Modules are now **optional** — modern Angular defaults to standalone components.
- `@Input` → data flows **down** (parent to child) → `[prop]="value"`
- `@Output` → event flows **up** (child to parent) → `(event)="handler($event)"` with `EventEmitter`
- `[ ]` = data in · `( )` = event out

---