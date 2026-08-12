#    Angular · Day 3 — Services, Singletons & Pipes

---

## 1. What is an Angular service?

A plain class that holds logic or data you want to **share across components**. API calls, logged-in user state, a company name shown in multiple places — this belongs in a service, not duplicated in every component.

> **In one line:** components are for the view, services are for the shared work. Keep components light, put reusable logic in services.

A service is a class marked with `@Injectable`. CLI: `ng generate service logo`

```ts
// logo.service.ts
import { Injectable } from "@angular/core";

@Injectable({ providedIn: "root" })
export class LogoService {
  companyName = "Resume Loop";

  getCompanyName() { return this.companyName; }
  setCompanyName(name: string) { this.companyName = name; }
}
```

`@Injectable` marks the class as injectable. `providedIn: "root"` makes it available across the whole app.

---

## 2. Dependency Injection (DI) — how a component gets a service

A component doesn't build the service itself — it asks for it in the constructor, and Angular hands it over.

```ts
// header.component.ts
import { Component } from "@angular/core";
import { LogoService } from "./logo.service";

@Component({
  selector: "app-header",
  template: `
    <h1>{{ logoService.getCompanyName() }}</h1>
    <button (click)="logoService.setCompanyName('Snapied')">Change</button>
  `,
})
export class HeaderComponent {
  constructor(public logoService: LogoService) {}
}
```

`public logoService: LogoService` in the constructor says "I need a `LogoService`." Angular creates it (or reuses the existing one) and passes it in. **You never write `new LogoService()` yourself.**

> **Picture it:** you don't build your own electricity generator at home — you plug into the socket and the supply is provided. DI is the same: the component plugs in and asks, Angular provides.

---

## 3. Singleton — one shared instance

When a service is `providedIn: "root"`, Angular creates **exactly one instance** for the whole app and gives that same one to everybody who asks. This is called a **singleton**.

Why it matters: since everyone shares the same instance, if one component changes a value, every other component sees the change.

### Three everyday analogies

**1. The TV remote in a family**
One remote, shared by everyone. Papa switches to cricket → everyone sees cricket. Sister switches to cartoons → everyone sees cartoons. No secret second remote.

**2. The washing machine in a small family**
One machine. Mother starts a load; son later uses the same machine, in whatever state it was left. No separate machine appears for each person.

**3. Items and the cart in a mall**
One shopping cart. Shirt from one shop, shoes from another, toy from a third — at billing, the cart holds everything from every shop. Same cart the whole time.

### The opposite (for contrast)

If everyone had their **own** remote / machine / cart — nothing would be shared. Change one, others know nothing. That's life *without* a singleton: separate copies, no sharing.

---

## 4. Sharing data through the singleton

Two components that are **not** parent-child can still share data through a service, because both get the same singleton.

```ts
// Header changes it:
this.logoService.setCompanyName("Snapied");

// Footer reads it (in its template):
<p>{{ logoService.getCompanyName() }}</p> // shows "Snapied"
```

> ⚠️ **Common mistake:** copying the value into a component's own property once in `ngOnInit` — it will show the old value forever, since it was read only once. Read it directly in the template with `logoService.getCompanyName()` so it always shows the latest.
>
> *Think: don't photograph the whiteboard once — keep looking at the board.*

---

## 5. Why Angular 13, not 22?

| Reason | Explanation |
|---|---|
| Core ideas are the same | Components, services, DI, pipes, modules work identically across versions |
| Most real jobs run older versions | Companies don't upgrade every year — knowing older versions is an advantage |
| Stable & well documented | Years of tutorials, answers, examples exist |
| Fewer moving parts for learning | Newer features (standalone components, signals) add confusion for a first course |

**Takeaway:** learn strong fundamentals on a stable version. Moving to Angular 22 later is a small jump once basics are solid.

---

## 6. Pipes: format a value for display

A pipe changes how a value **looks** in the template, without changing the real data. Used with `|`.

```html
<p>{{ name | uppercase }}</p>            <!-- RESUME LOOP -->
<p>{{ price | currency:'INR' }}</p>      <!-- Rs 500.00 -->
<p>{{ today | date:'longDate' }}</p>     <!-- August 7, 2026 -->
```

**Common built-in pipes:** `uppercase`, `lowercase`, `titlecase`, `date`, `currency`, `number`, `percent`, `json`, `slice`

**Chaining:**
```html
{{ name | slice:0:5 | uppercase }}
```

---

## 7. Custom pipes: make your own

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
<p>{{ 10 | double }}</p> <!-- shows 20 -->
```

`transform` gets the value on the left of `|`, does its work, returns the result.

> CLI: `ng generate pipe double`. **Remember to add it to your module's `declarations`** (Angular 13).

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

## 8. Key takeaways

- **Service:** a class for shared logic/data. `@Injectable` with `providedIn: "root"`
- **DI:** ask for a service in the constructor, Angular provides it. Never write `new`
- **Singleton:** one shared instance for the whole app — one TV remote, one washing machine, one mall cart. Change it in one place, everyone sees it
- **Why Angular 13:** core ideas are identical across versions, most jobs use older versions, stable base is easier to learn on
- **Pipes:** format a value in the template with `|`. Built-in (`uppercase`, `date`, etc.) or custom via `@Pipe` + `transform`

---
