---
title:  Call Stack
sidebar_position: 3
description: Learn about JavaScript Call Stack
---

## 1. What Is the Call Stack?

The **Call Stack** is a mechanism JavaScript uses to keep track of currently executing functions.

Whenever a function is called, JavaScript creates an **execution context** for that function and conceptually places it on the Call Stack.

When the function finishes, its execution context is removed from the stack.

### Basic flow

```text
Function is called
       ↓
Execution Context created
       ↓
Context pushed onto Call Stack
       ↓
Function executes
       ↓
Function returns
       ↓
Context removed from Call Stack
```

---

# 2. Stack = LIFO

The Call Stack follows:

> **LIFO — Last In, First Out**

Think about a stack of plates.

```text
       ┌─────────┐
       │ Plate 3 │ ← last added
       ├─────────┤
       │ Plate 2 │
       ├─────────┤
       │ Plate 1 │ ← first added
       └─────────┘
```

You cannot remove Plate 1 directly while Plate 3 and Plate 2 are still on top.

The same conceptual rule applies to function calls.

---

# 3. Nested Function Calls

Consider:

```js
function A() {
    console.log("A start");
    B();
    console.log("A end");
}

function B() {
    console.log("B start");
    C();
    console.log("B end");
}

function C() {
    console.log("C");
}

A();
```

The output is:

```text
A start
B start
C
B end
A end
```

Why?

### Initially

```text
Call Stack

┌──────────────┐
│    Global    │
└──────────────┘
```

`A()` is called:

```text
┌──────────────┐
│     A()      │
├──────────────┤
│    Global    │
└──────────────┘
```

Then `A()` calls `B()`:

```text
┌──────────────┐
│     B()      │ ← executing
├──────────────┤
│     A()      │ ← waiting
├──────────────┤
│    Global    │
└──────────────┘
```

Then `B()` calls `C()`:

```text
┌──────────────┐
│     C()      │ ← executing
├──────────────┤
│     B()      │
├──────────────┤
│     A()      │
├──────────────┤
│    Global    │
└──────────────┘
```

At this point, `C()` prints:

```text
C
```

Then `C()` finishes and is removed:

```text
┌──────────────┐
│     B()      │ ← resumes
├──────────────┤
│     A()      │
├──────────────┤
│    Global    │
└──────────────┘
```

`B()` continues with:

```js
console.log("B end");
```

Then `B()` finishes.

Finally:

```text
┌──────────────┐
│     A()      │ ← resumes
├──────────────┤
│    Global    │
└──────────────┘
```

`A()` prints `"A end"` and finishes.

---

# 4. Important: A Function Does Not Restart

When `A()` calls `B()`, `A()` does **not** disappear.

It is simply waiting.

```text
A()
 ↓
B()
 ↓
C()
```

When `C()` finishes:

```text
C() removed
 ↓
B() resumes
 ↓
B() removed
 ↓
A() resumes
```

So the better mental model is:

> A currently executing function can pause while a nested function executes, then continue from where it left off.

---

# 5. Execution Context vs Call Stack

These concepts are related but different.

### Execution Context

An execution context represents the environment in which JavaScript code executes.

It contains information such as:

* variables/bindings
* parameters
* scope information
* execution state

### Call Stack

The Call Stack keeps track of the active execution contexts/function calls.

Conceptually:

```text
Execution Context
        ↓
placed on
        ↓
Call Stack
```

---

# 6. Stack Frame

A **stack frame** is the conceptual representation of one active function call on the Call Stack.

For example:

```js
function add(a, b) {
    const result = a + b;
    return result;
}

add(10, 20);
```

Conceptually:

```text
Call Stack

┌──────────────────────┐
│ add(10, 20)          │
│ a = 10               │
│ b = 20               │
│ result = 30          │
│ return information   │
├──────────────────────┤
│ Global               │
└──────────────────────┘
```

Important:

> This is a **conceptual model**.

Modern JavaScript engines such as V8 optimize execution heavily. The actual physical implementation does not necessarily look exactly like this diagram in RAM.

---

# 7. Return From a Function

When a function returns:

```js
function greet() {
    return "Hello";
}

const message = greet();
```

The flow is conceptually:

```text
greet()
   ↓
Execution Context created
   ↓
Stack frame pushed
   ↓
"Hello" produced
   ↓
return
   ↓
greet() frame removed
   ↓
Global execution continues
```

The caller then receives the result.

---

# 8. Recursion and the Call Stack

Consider:

```js
function count(n) {
    if (n === 0) {
        return;
    }

    count(n - 1);
}

count(3);
```

The calls build up:

```text
count(0)
count(1)
count(2)
count(3)
Global
```

Then they unwind:

```text
count(0) finishes
      ↓
count(1) finishes
      ↓
count(2) finishes
      ↓
count(3) finishes
```

This is another example of LIFO.

---

# 9. Stack Overflow

If recursion never stops:

```js
function forever() {
    forever();
}

forever();
```

The Call Stack keeps growing:

```text
forever()
forever()
forever()
forever()
forever()
...
...
```

Eventually the engine reaches its call-stack limit and throws an error such as:

```text
RangeError: Maximum call stack size exceeded
```

This is commonly called a **stack overflow**.

---

# 10. Process vs Thread vs Function

This distinction is extremely important.

### Process

A process is an operating-system-managed running program instance.

```text
Operating System
│
├── Process A
│
└── Process B
```

Processes have their own resources and memory isolation managed by the OS.

### Thread

A thread is an execution path within a process.

Conceptually:

```text
Process
└── Thread
```

### Function

A JavaScript function is application-level code.

When it executes, it creates an execution context/stack frame that is tracked by the JavaScript execution mechanism.

```text
Process
   ↓
Thread
   ↓
JavaScript execution
   ↓
Call Stack
   ↓
Function stack frames
```

Therefore:

```text
❌ Function = Process
❌ Call Stack = Process
❌ Stack Frame = Process

✅ Process contains threads
✅ JavaScript execution uses a Call Stack
✅ Function calls create execution contexts/stack frames
```

---

# 11. Long-Running Synchronous JavaScript

Consider:

```js
function longTask() {
    for (let i = 0; i < 5000000000; i++) {
        // heavy work
    }
}

console.log("Start");

longTask();

console.log("End");
```

The output begins with:

```text
Start
```

Then `longTask()` enters the Call Stack.

```text
┌──────────────────┐
│   longTask()     │ ← executing
├──────────────────┤
│     Global       │
└──────────────────┘
```

While `longTask()` is running, the next statement:

```js
console.log("End");
```

cannot execute.

`longTask()` must finish first.

Then:

```text
longTask()
    ↓
returns
    ↓
removed from Call Stack
    ↓
Global continues
    ↓
console.log("End")
```

So:

```text
Start
[heavy work...]
End
```

---

# 12. What Does "Blocking the Main Thread" Mean?

A long-running synchronous JavaScript operation can **block further JavaScript work from running on that execution path** because the Call Stack remains occupied.

Conceptually:

```text
Call Stack
    │
    ▼
longTask()
    │
    │
    │  heavy synchronous work
    │
    ▼
return
    │
    ▼
next JavaScript work
```

The important idea is:

> The Call Stack is occupied by the current synchronous work, so another JavaScript callback cannot simply execute at the same time on that same execution path.

---

# 13. Call Stack Is Not RAM

This distinction came up during our discussion.

These are different concepts:

```text
RAM
│
└── Memory where data/program state can reside

CPU
│
└── Physically executes machine instructions

Call Stack
│
└── JavaScript execution mechanism for tracking active calls
```

Therefore:

> **Call Stack blocking is not the same thing as RAM being full.**

A program can have plenty of available RAM and still have JavaScript work blocked by a long-running synchronous function.

---

# 14. The Plate Analogy

Your analogy is useful:

> "We can't take the first plate without taking the third plate."

That represents the LIFO nature of the Call Stack.

For example:

```text
       ┌──────────────┐
       │    C()       │ ← must finish first
       ├──────────────┤
       │    B()       │
       ├──────────────┤
       │    A()       │
       ├──────────────┤
       │   Global     │
       └──────────────┘
```

`A()` cannot resume until `B()` finishes.

`B()` cannot resume until `C()` finishes.

So execution unwinds from the top:

```text
C()
 ↓
B()
 ↓
A()
 ↓
Global
```

---

# 15. Call Stack and Asynchronous JavaScript

This is where the Event Loop becomes important.

Consider:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

The output is:

```text
A
C
B
```

Why?

`A` executes immediately.

`setTimeout()` registers a timer/callback with the surrounding runtime environment.

JavaScript does not wait for the callback to execute immediately.

Then `C` executes.

Later, when the callback is eligible and the Call Stack is available, the callback can execute and print `B`.

Conceptually:

```text
JavaScript
     │
     ▼
 Call Stack
     │
     ▼
setTimeout()
     │
     ▼
Timer / Runtime
     │
     ▼
Callback becomes ready
     │
     ▼
Queue
     │
     ▼
Event Loop
     │
     ▼
Call Stack
     │
     ▼
console.log("B")
```

---

# 16. Important Terminology: No "Event Loop Stack"

During our discussion, the idea was expressed as:

> "setTimeout function will execute in event loop stack."

The better terminology is:

```text
❌ Event Loop Stack

✅ Call Stack
✅ Callback/Task Queue
✅ Microtask Queue
✅ Event Loop
```

The Event Loop is not another stack containing functions.

It coordinates when queued work can be moved toward execution when the Call Stack is available.

---

# 17. The Big Picture

Everything we've learned so far connects together:

```text
JavaScript code
      │
      ▼
Function call
      │
      ▼
Execution Context
      │
      ▼
Stack Frame
      │
      ▼
Call Stack
      │
      ▼
Function executes
      │
      ▼
Function returns
      │
      ▼
Stack Frame removed
      │
      ▼
Previous function resumes
```

For nested calls:

```text
A()
 ↓
B()
 ↓
C()

Call Stack:

┌─────────┐
│   C()   │ ← execute first
├─────────┤
│   B()   │
├─────────┤
│   A()   │
├─────────┤
│ Global  │
└─────────┘

Then unwind:

C() → B() → A() → Global
```

---

# 18. Foundation for the Event Loop

The Call Stack is the foundation for understanding asynchronous JavaScript.

The next major concept is:

```text
                 JavaScript
                     │
                     ▼
                Call Stack
                     │
             ┌───────┴────────┐
             │                │
        Synchronous       Async operation
             │                │
             │                ▼
             │        Runtime / Web APIs
             │                │
             │                ▼
             │             Queues
             │                │
             └───────────────►│
                              ▼
                         Event Loop
                              │
                              ▼
                         Call Stack
```

This leads directly into:

* Event Loop
* Callback/Task Queue
* Microtask Queue
* Promises
* `setTimeout`
* `queueMicrotask`
* `async/await`
* Why Promise callbacks execute before timers

---

# Key Takeaways

1. The **Call Stack tracks active JavaScript function calls**.
2. A function call creates an execution context and is conceptually represented by a stack frame.
3. The Call Stack follows **LIFO — Last In, First Out**.
4. A function that calls another function waits for that nested function to return.
5. Functions resume after nested calls finish; they are not restarted.
6. Recursion repeatedly pushes stack frames.
7. Excessive recursion can cause **stack overflow**.
8. **Process, thread, execution context, and stack frame are different concepts.**
9. A long-running synchronous function can block further JavaScript work because the Call Stack remains occupied.
10. **Call Stack blocking is not the same as RAM being full.**
11. `setTimeout(..., 0)` does not mean its callback executes immediately.
12. The **Event Loop is not an Event Loop Stack**; it coordinates queued work and the Call Stack.
13. Understanding the Call Stack is essential before learning the **Event Loop and Microtask Queue**.
