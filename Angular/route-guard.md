# Angular · Day 5 — Route Guards

---

## 1. What is a route guard?

A small piece of logic that runs **before a route opens** and decides whether to allow it. If the check passes, the page loads. If not, the guard blocks it and usually redirects (e.g. to login).

> **In one line:** a guard is a security gate in front of a route. It answers "is this user allowed in?" before the page shows.

**The ResumeForge problem it solves:** the profile page should only open for a logged-in user. Without a guard, someone can skip login by typing the URL directly. The guard checks for a token first and turns them away if it's missing.

---

## 2. Making a guard (Angular 13 style)

A guard is a service that implements `CanActivate`. CLI: `ng generate guard auth` (choose `CanActivate` when asked).

```ts
// auth.guard.ts
import { Injectable } from "@angular/core";
import { CanActivate, Router } from "@angular/router";
import { AuthService } from "./auth.service";

@Injectable({ providedIn: "root" })
export class AuthGuard implements CanActivate {
  constructor(private auth: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.auth.getToken()) {
      return true; // token exists, allow the page
    }
    // no token, send them to login and block the page
    this.router.navigate(["/login"]);
    return false;
  }
}
```

`canActivate` returns `true` to allow the route, `false` to block it. Here: token present → allow; missing → redirect to login and return `false`.

---

## 3. Putting the guard on a route

Add `canActivate` to the route you want to protect, in the routing module.

```ts
// app-routing.module.ts
import { AuthGuard } from "./auth.guard";

const routes: Routes = [
  { path: "login", component: LoginComponent },
  {
    path: "profile",
    component: ProfileComponent,
    canActivate: [AuthGuard], // protected
  },
  { path: "", redirectTo: "login", pathMatch: "full" },
];
```

Before `/profile` opens, Angular runs `AuthGuard`. Logged in → page shows. Not logged in → straight to login. Same guard can be put on any number of routes.

---

## 4. The reverse guard: `NoAuthGuard`

Opposite need: if a user is already logged in, they shouldn't see login/signup again. Typing `/login` while already logged in should send them to profile, not show the form.

`NoAuthGuard` allows the page **only when there is no token**.

```ts
// no-auth.guard.ts
import { Injectable } from "@angular/core";
import { CanActivate, Router } from "@angular/router";
import { AuthService } from "./auth.service";

@Injectable({ providedIn: "root" })
export class NoAuthGuard implements CanActivate {
  constructor(private auth: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (!this.auth.getToken()) {
      return true; // no token, allow login/signup page
    }
    // already logged in, send them to profile
    this.router.navigate(["/profile"]);
    return false;
  }
}
```

Mirror image of `AuthGuard`: `AuthGuard` allows the page when a token **exists**. `NoAuthGuard` allows the page when a token **does not exist**. One check, flipped.

Put it on the login and signup routes:

```ts
// app-routing.module.ts
const routes: Routes = [
  { path: "login", component: LoginComponent, canActivate: [NoAuthGuard] },
  { path: "signup", component: SignupComponent, canActivate: [NoAuthGuard] },
  { path: "profile", component: ProfileComponent, canActivate: [AuthGuard] },
  { path: "", redirectTo: "login", pathMatch: "full" },
];
```

### The two guards side by side

| Guard | Protects | Rule |
|---|---|---|
| `AuthGuard` | Private pages (e.g. profile) | No token? → go to login |
| `NoAuthGuard` | Login and signup pages | Already have a token? → go to profile |

Together: logged-out users can't reach private pages, logged-in users aren't shown the login form again.

---

## 5. The full auth story, together

1. **Login** checks the user and gives a token, which we save
2. **The interceptor** attaches that token to every request, so the server knows who is asking
3. **The guard** protects the pages, so only a logged-in user can open them

Three pieces, one system. Login + interceptor were built earlier — the guard completes it.

---

## 6. The main types of guard

`CanActivate` is the one you'll use most, but here's the family:

| Guard | Runs when | Used for |
|---|---|---|
| `CanActivate` | Before entering a route | Block a page if not logged in |
| `CanDeactivate` | Before leaving a route | Warn about unsaved changes |
| `CanActivateChild` | Before entering child routes | Protect a group of pages at once |
| `Resolve` | Before a route loads | Fetch data so the page opens ready |

> **Nice one to know:** `CanDeactivate` can ask "You have unsaved changes, leave anyway?" when a user tries to navigate away from a half-filled form. Useful in a resume builder like ResumeForge.

---

## 7. Key takeaways

- A route guard is a gate that runs before a route and decides if the user may enter
- `CanActivate` is the common one: return `true` to allow, `false` to block
- The auth guard checks for a token; if missing, redirects to login and blocks the page
- Attach with `canActivate: [AuthGuard]` on the route
- `NoAuthGuard` is the reverse: keeps logged-in users away from login/signup, sending them to profile instead
- **The whole story:** login gets the token, the interceptor sends it, the guard protects the pages

---
