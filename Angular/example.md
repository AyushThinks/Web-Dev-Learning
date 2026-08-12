# Angular · Day 4 (Worked Example) — Real Example: Login & Profile

---

## 1. The whole flow, in one look

1. User types email and password in ResumeForge, clicks Login
2. App sends them to the server → server replies with a **token**, which we save
3. We move to the profile page
4. Profile page asks the server for user data → **interceptor** adds the saved token to that request
5. Server checks the token and returns the user's profile, which we show

---

## 2. The auth service — login and save the token

Login sends email + password with `POST`. When the token comes back, we save it.

```ts
// auth.service.ts
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";

@Injectable({ providedIn: "root" })
export class AuthService {
  private api = "https://api.resumeforge.com";

  constructor(private http: HttpClient) {}

  login(email: string, password: string): Observable<any> {
    return this.http.post(this.api + "/login", { email, password });
  }

  saveToken(token: string) {
    localStorage.setItem("token", token);
  }

  getToken() {
    return localStorage.getItem("token");
  }

  logout() {
    localStorage.removeItem("token");
  }
}
```

The service just makes the call and returns it. Saving is a separate small method — each method does one clear job.

---

## 3. The login component

On submit → call `login`, subscribe. Inside the callback: save the token, go to the profile page. On error: show a message.

```ts
// login.component.ts
export class LoginComponent {
  email = "";
  password = "";
  error = "";

  constructor(private auth: AuthService, private router: Router) {}

  onLogin() {
    this.auth.login(this.email, this.password).subscribe(
      (res: any) => {
        this.auth.saveToken(res.token);       // save the token
        this.router.navigate(["/profile"]);   // go to profile
      },
      (err) => {
        this.error = "Invalid email or password";
      }
    );
  }
}
```

```html
<!-- login.component.html -->
<input [(ngModel)]="email" placeholder="Email" />
<input [(ngModel)]="password" type="password" placeholder="Password" />
<button (click)="onLogin()">Login</button>
<p *ngIf="error">{{ error }}</p>
```

**Why save the token here?** Because it only arrives when the server replies — inside the `subscribe` callback. That's the moment we have it.

**Order matters, and it's safe here:** inside the callback, `saveToken` runs first and finishes immediately (plain `localStorage.setItem`, not an API call), *then* `navigate` runs. By the time the profile page loads and asks for data, the token is already saved. These two lines run one after the other, not at the same time.

---

## 4. The profile service — fetch the user

Just asks for the profile. Notice: it does **not** add the token by hand — the interceptor does that.

```ts
// profile.service.ts
@Injectable({ providedIn: "root" })
export class ProfileService {
  private api = "https://api.resumeforge.com";

  constructor(private http: HttpClient) {}

  getProfile(): Observable<any> {
    return this.http.get(this.api + "/me");
  }
}
```

---

## 5. The interceptor — add the token to every request

Every request passes through here. If there's a token, add it as a header.

```ts
// auth.interceptor.ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private auth: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const token = this.auth.getToken();
    if (token) {
      const cloned = req.clone({
        setHeaders: { Authorization: "Bearer " + token },
      });
      return next.handle(cloned);
    }
    return next.handle(req);
  }
}
```

Register in the module:
```ts
{ provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
```

Now the profile request — and every request — carries the token automatically.

---

## 6. The profile component — show the user

```ts
// profile.component.ts
export class ProfileComponent implements OnInit {
  user: any = null;

  constructor(private profile: ProfileService) {}

  ngOnInit() {
    this.profile.getProfile().subscribe((data) => {
      this.user = data;
    });
  }
}
```

```html
<!-- profile.component.html -->
<div *ngIf="user">
  <h2>{{ user.name }}</h2>
  <p>{{ user.email }}</p>
</div>
```

---

## 7. The routes — login and profile pages

Map `/login` and `/profile` to their components in the routing module.

```ts
// app-routing.module.ts
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { LoginComponent } from "./login/login.component";
import { ProfileComponent } from "./profile/profile.component";

const routes: Routes = [
  { path: "login", component: LoginComponent },
  { path: "profile", component: ProfileComponent },
  { path: "", redirectTo: "login", pathMatch: "full" }, // default page
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

`/login` shows the login page, `/profile` shows the profile page, empty path redirects to login. This is why `this.router.navigate(["/profile"])` in the login worked — the route exists.

Also need a `<router-outlet>` in the main template:

```html
<!-- app.component.html -->
<router-outlet></router-outlet>
```

> **Going further (guard):** right now anyone can open `/profile` by typing the URL, even without logging in. A **route guard** can block that by checking for a token before letting the route open. Kept simple today — that's the next step to make it secure.

---

## 8. Styling it with SCSS

CSS with variables, nesting, and reuse — Angular supports it out of the box. Component style files end in `.scss`.

```scss
// login.component.scss
$primary: #c0272d;
$radius: 6px;

.login-box {
  max-width: 320px;
  margin: 40px auto;
  padding: 24px;
  border-radius: $radius;

  input {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
  }

  button {
    background: $primary;
    color: white;
    padding: 10px 16px;
    border: none;
    border-radius: $radius;

    &:hover {
      background: darken($primary, 10%);
    }
  }

  .error {
    color: $primary;
  }
}
```

Wins over plain CSS: `$primary` stores the colour once, input/button styles nest inside `.login-box` to match the HTML, `&:hover` means `button:hover`. Change `$primary` in one place, whole page follows.

```scss
// profile.component.scss
$primary: #c0272d;

.profile-card {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #eee;
  border-radius: 6px;

  h2 {
    color: $primary;
    margin-bottom: 4px;
  }

  p {
    color: #555;
  }
}
```

> **Tip:** put shared values like `$primary` in one partial file (e.g. `_variables.scss`) and pull it into each component's styles. Then the whole app shares one colour palette.

### Mixins: reusable blocks of styles

A named group of styles written once, dropped into any rule with `@include`. Great for repeated patterns like centering or a card look.

```scss
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.login-box {
  @include flex-center;
  flex-direction: column;
}
```

Mixins can take arguments — one mixin, many variations:

```scss
@mixin button($bg) {
  background: $bg;
  color: white;
  padding: 10px 16px;
  border-radius: 6px;
}

.save-btn { @include button(#c0272d); }
.cancel-btn { @include button(#6b7a8f); }
```

### Inheritance with `@extend`

Lets one selector inherit all the styles of another, avoiding repetition.

```scss
.btn {
  padding: 10px 16px;
  border-radius: 6px;
  border: none;
}

.save-btn {
  @extend .btn; // takes all of .btn
  background: $primary;
}

.cancel-btn {
  @extend .btn;
  background: #6b7a8f;
}
```

> **Mixin vs `@extend`, the simple rule:** use a mixin when you need arguments or fresh styles each time. Use `@extend` when several selectors truly share the same base look. Both cut repetition.

### Operators and functions

SCSS can do maths and transform values — plain CSS can't:

```scss
.col {
  width: 100% / 3; // maths
}

.button:hover {
  background: darken($primary, 10%); // colour function
}
```

Handy built-in colour functions: `darken()`, `lighten()`, `rgba()`.

### Placeholders (`%`): a lighter `@extend`

Starts with `%`, only appears in final CSS if something extends it — nothing wasted.

```scss
%card-base {
  padding: 20px;
  border-radius: 6px;
  border: 1px solid #eee;
}

.profile-card { @extend %card-base; }
.resume-card { @extend %card-base; }
```

---

## 9. Key takeaways

- Login sends email/password, gets a token back, saves it in the `subscribe` callback
- The token proves who the user is — protected requests must carry it
- The interceptor adds the token to every request, so the profile service never has to
- Routes map `/login` and `/profile` to their components, shown in a `<router-outlet>`. A guard can protect `/profile` later
- **The payoff:** log in once to get the token, every protected call just works
- SCSS: `$variables` for colours, nesting to match the HTML, `@mixin`/`@include` for reusable blocks, `@extend` for inheritance. Compiles to normal CSS

---
