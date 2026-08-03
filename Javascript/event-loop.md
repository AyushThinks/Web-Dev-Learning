# ⚡ JavaScript Event Loop

These are my personal notes on how the **JavaScript Event Loop** works, including the **Call Stack**, **Web APIs**, **Callback Queue**, **Microtask Queue**, and the execution order of synchronous and asynchronous code.

---

# 📚 Table of Contents

1. What is the Event Loop?
2. Why Do We Need the Event Loop?
3. JavaScript Runtime Architecture
4. Call Stack
5. Web APIs
6. Callback Queue (Task Queue)
7. Microtask Queue
8. Event Loop Working
9. Execution Flow
10. Example
11. Interview Questions
12. Key Takeaways

---

# 1. What is the Event Loop?

JavaScript is a **single-threaded language**, meaning it can execute **only one task at a time**.

Even though JavaScript is single-threaded, it can still perform asynchronous operations such as:

- `setTimeout()`
- `setInterval()`
- `fetch()`
- DOM Events
- File Operations (Node.js)

This is possible because of the **Event Loop**.

The Event Loop continuously manages the execution of asynchronous tasks without blocking the main thread.

---

# 2. Why Do We Need the Event Loop?

Consider this code:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Hello");
}, 5000);

console.log("End");
```

Without the Event Loop, JavaScript would wait for **5 seconds** before executing the next line.

Output would become:

```
Start
(wait 5 seconds)
Hello
End
```

This would freeze the application.

Instead, JavaScript sends asynchronous tasks to the browser (or Node.js), allowing the remaining synchronous code to execute immediately.

Actual Output:

```
Start
End
(wait 5 seconds)
Hello
```

This makes JavaScript fast and non-blocking.

---

# 3. JavaScript Runtime Architecture

A JavaScript runtime consists of the following components:

```
                JavaScript Runtime

            ┌─────────────────────┐
            │     Call Stack      │
            └─────────┬───────────┘
                      │
        Async Task    │
                      ▼
            ┌─────────────────────┐
            │      Web APIs       │
            └─────────┬───────────┘
                      │
          Task Ready  ▼
     ┌───────────────────────────┐
     │     Callback Queue        │
     └───────────────────────────┘

     ┌───────────────────────────┐
     │    Microtask Queue        │
     └───────────────────────────┘
                      ▲
                      │
               Event Loop
```

---

# 4. Call Stack

The **Call Stack** is where JavaScript executes code.

It follows the **LIFO (Last In, First Out)** principle.

Every function call is pushed onto the stack.

After execution, it is removed (popped) from the stack.

Example:

```javascript
function one() {
  two();
}

function two() {
  console.log("Hello");
}

one();
```

Execution:

```
Push one()
Push two()
Print Hello
Pop two()
Pop one()
```

The Call Stack always executes **synchronous code first**.

---

# 5. Web APIs

Web APIs are provided by the browser (or Node.js runtime).

They handle asynchronous operations outside the Call Stack.

Examples:

- `setTimeout()`
- `setInterval()`
- `fetch()`
- DOM Events
- Geolocation
- File System (Node.js)

Example:

```javascript
setTimeout(() => {
  console.log("Done");
}, 3000);
```

Execution:

```
Call Stack
     │
     ▼
Web API
(wait 3 seconds)
     │
     ▼
Callback Queue
```

The Call Stack remains free to execute other code.

---

# 6. Callback Queue (Task Queue)

The Callback Queue stores callbacks from completed asynchronous tasks.

Examples:

- `setTimeout`
- `setInterval`
- DOM Events
- Message Events

Example:

```javascript
setTimeout(() => {
  console.log("Timer Finished");
}, 1000);
```

After one second:

```
Callback Queue

┌─────────────────────┐
│ console.log(...)    │
└─────────────────────┘
```

The callback waits until the Call Stack becomes empty.

---

# 7. Microtask Queue

The Microtask Queue has **higher priority** than the Callback Queue.

It stores:

- Promise `.then()`
- Promise `.catch()`
- Promise `.finally()`
- `queueMicrotask()`
- `MutationObserver`

Example:

```javascript
Promise.resolve().then(() => {
  console.log("Promise");
});
```

The callback is placed inside the **Microtask Queue**.

---

## Priority

```
Highest Priority

Call Stack

↓

Microtask Queue

↓

Callback Queue
```

The Event Loop **always executes every microtask before processing callback queue tasks**.

---

# 8. How the Event Loop Works

The Event Loop continuously monitors the runtime.

Its job is very simple:

1. Check if the Call Stack is empty.
2. If empty, execute all pending Microtasks.
3. After the Microtask Queue is completely empty, execute one task from the Callback Queue.
4. Repeat forever.

---

# Event Loop Algorithm

```
Loop Forever

↓

Is Call Stack Empty?

↓

YES

↓

Is Microtask Queue Empty?

↓

NO

↓

Execute ALL Microtasks

↓

Check Again

↓

If Empty

↓

Execute ONE Callback Queue Task

↓

Repeat
```

---

# 9. Execution Flow

Example:

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

Execution:

### Step 1

```
Call Stack

console.log("A")

Output:

A
```

---

### Step 2

```
setTimeout()

↓

Web API
```

---

### Step 3

```
Promise

↓

Microtask Queue
```

---

### Step 4

```
console.log("D")

Output:

A
D
```

---

### Step 5

Call Stack becomes empty.

Event Loop checks:

```
Microtask Queue

↓

Promise

↓

Output:

A
D
C
```

---

### Step 6

Microtask Queue becomes empty.

Now Event Loop executes Callback Queue.

Output:

```
A
D
C
B
```

---

# 10. Complete Example

```javascript
console.log("1. Start");

setTimeout(() => {
  console.log("2. Timer (Callback Queue)");
}, 0);

Promise.resolve().then(() => {
  console.log("3. Promise (Microtask Queue)");
});

console.log("4. End");
```

### Output

```text
1. Start
4. End
3. Promise (Microtask Queue)
2. Timer (Callback Queue)
```

### Explanation

1. `"Start"` is synchronous, so it executes immediately.
2. `setTimeout()` is sent to the Web API.
3. Promise callback is added to the Microtask Queue.
4. `"End"` executes synchronously.
5. The Call Stack becomes empty.
6. Event Loop executes all Microtasks first.
7. Finally, it executes the Callback Queue task.

---

# 11. Interview Questions

### Why is JavaScript called single-threaded?

Because it has only **one Call Stack**, allowing it to execute one task at a time.

---

### Does `setTimeout(fn, 0)` execute immediately?

No.

It is first handled by the Web API, then placed in the Callback Queue.

It executes only when:

- The timer has completed.
- The Call Stack is empty.
- The Microtask Queue is empty.

---

### Which has higher priority?

```
Microtask Queue > Callback Queue
```

Promises always execute before `setTimeout()` callbacks.

---

### What does the Event Loop do?

The Event Loop continuously checks:

- Is the Call Stack empty?
- Are there pending Microtasks?
- Are there pending Callback Queue tasks?

It moves eligible tasks to the Call Stack for execution.

---

# 12. Key Takeaways

- JavaScript is **single-threaded**.
- Synchronous code executes inside the **Call Stack**.
- Asynchronous operations are handled by **Web APIs**.
- Completed async callbacks move to the **Callback Queue**.
- Promise callbacks are stored in the **Microtask Queue**.
- The Event Loop executes **all Microtasks before Callback Queue tasks**.
- `setTimeout(..., 0)` never executes immediately.
- The Event Loop keeps JavaScript non-blocking and responsive.

---
