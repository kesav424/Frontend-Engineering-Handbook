---

title: Execution Context, Scope, Hoisting, and TDZ
sidebar_position: 3
description: Understand how JavaScript creates execution contexts, resolves variables through lexical scope, handles hoisting, and enforces the Temporal Dead Zone.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 08.10 — Execution Context, Scope, Hoisting, and TDZ

> JavaScript does not simply execute source code from top to bottom. Before execution begins, the JavaScript engine establishes environments needed to run the program.

---

## Learning Objectives

By the end of this lesson, you should understand:

* What an Execution Context is
* Why JavaScript creates an Execution Context
* Creation and execution phases
* Function Execution Contexts
* Lexical environments
* Scope chains
* Lexical scope
* `var` hoisting
* Function declaration hoisting
* `let` and `const`
* Temporal Dead Zone (TDZ)
* Why `typeof` behaves differently with undeclared and uninitialized variables
* Why function declarations behave differently from function expressions

---

# 1. What Is an Execution Context?

An **Execution Context** is the environment JavaScript uses to evaluate and execute code.

When JavaScript starts running a program, it needs information such as:

* What variables exist?
* What functions exist?
* What values are associated with those bindings?
* What scope should be searched when a variable is referenced?
* What `this` value applies?
* Where should execution continue?

The execution context provides the environment needed for that work.

---

# 2. The Global Execution Context

Consider:

```javascript
let name = "Kesav";

console.log(name);
```

Before JavaScript executes:

```javascript
console.log(name);
```

it establishes the global execution environment.

Conceptually:

```text
Global Execution Context

┌─────────────────────────────┐
│ name → "Kesav"              │
│ console → ...               │
│ other global bindings       │
└─────────────────────────────┘
```

Then JavaScript executes the statements.

---

# 3. Function Execution Context

Every time a function is called, JavaScript needs a new environment for that function's execution.

Example:

```javascript
let name = "Kesav";

function greet() {
    let message = "Hello";

    console.log(message + " " + name);
}

greet();
```

When `greet()` is called, a new Function Execution Context is created.

Conceptually:

```text
Call Stack

┌──────────────────────────────┐
│ greet() Execution Context    │
│                              │
│ message → "Hello"            │
└──────────────────────────────┘
┌──────────────────────────────┐
│ Global Execution Context     │
│                              │
│ name → "Kesav"               │
└──────────────────────────────┘
```

The function has its own local environment.

---

# 4. Where Does `message` Live?

Inside:

```javascript
function greet() {
    let message = "Hello";
}
```

`message` belongs to the function's local environment.

Conceptually:

```text
greet()

message → "Hello"
```

However, do not oversimplify this into:

> Every JavaScript variable is physically stored directly on the CPU stack.

The stack/heap model is extremely useful for understanding JavaScript, but V8's actual implementation is more sophisticated. Optimizations can change how and where values are physically represented.

A better mental model is:

```text
Execution Context
        ↓
Lexical Environment / Bindings
        ↓
V8 manages the underlying representation
```

---

# 5. How Does `name` Get Found?

Inside:

```javascript
function greet() {
    let message = "Hello";

    console.log(message + " " + name);
}
```

there is no local `name`.

JavaScript therefore searches the function's outer lexical environment.

Conceptually:

```text
greet()
  │
  │ name found?
  │ NO
  ▼
Outer lexical environment
  │
  │ name found?
  │ YES
  ▼
"Kesav"
```

This is the beginning of understanding the **scope chain**.

---

# 6. Scope Chain

When JavaScript encounters:

```javascript
console.log(name);
```

it needs to resolve `name`.

Conceptually:

```text
Current Scope
      ↓
Outer Scope
      ↓
Outer Scope
      ↓
Global Scope
      ↓
Not found
      ↓
ReferenceError
```

The search stops when JavaScript finds the required binding.

---

# 7. Local Scope Wins

Consider:

```javascript
var a = 10;

function test() {
    var a = 20;

    console.log(a);
}

test();
```

The output is:

```text
20
```

Why?

JavaScript starts looking inside `test()`:

```text
test()

a → 20   ← FOUND
```

Because it found `a`, it stops.

It does not continue searching for the global `a`.

---

# 8. Scope Chain Example

Consider:

```javascript
var a = "global";

function outer() {
    var a = "outer";

    function inner() {
        var a = "inner";

        console.log(a);
    }

    inner();
}

outer();
```

Output:

```text
inner
```

Lookup:

```text
inner()
  ↓
inner scope
  ↓
a = "inner"
  ↓
STOP
```

If the inner declaration is removed:

```javascript
var a = "global";

function outer() {
    var a = "outer";

    function inner() {
        console.log(a);
    }

    inner();
}

outer();
```

The lookup becomes:

```text
inner()
  ↓
inner scope
  ↓
not found
  ↓
outer scope
  ↓
a = "outer"
  ↓
STOP
```

Output:

```text
outer
```

If both local declarations are removed:

```text
inner()
  ↓
inner scope
  ↓
outer scope
  ↓
global scope
  ↓
a = "global"
```

Output:

```text
global
```

---

# 9. Lexical Scope

JavaScript uses **lexical scope**.

Lexical scope means that the scope relationship is determined by **where code is written**.

Consider:

```javascript
const name = "Global";

function printName() {
    console.log(name);
}

function outer() {
    const name = "Outer";

    printName();
}

outer();
```

What happens?

The result is:

```text
Global
```

It does **not** print:

```text
Outer
```

Why?

`printName()` was created in the global scope.

Its lexical structure is:

```text
Global
├── name = "Global"
├── printName
└── outer
    └── name = "Outer"
```

`printName()` does not suddenly gain access to `outer()`'s local variables just because `outer()` called it.

---

# 10. Lexical Scope vs Dynamic Scope

JavaScript uses lexical scope.

It does not use dynamic scope.

Dynamic scope would conceptually search through the functions that called the current function.

JavaScript instead follows the lexical structure established where the function was created.

Important rule:

```text
Where the function is written
            ↓
determines its lexical scope
```

not:

```text
Where the function is called
```

This concept becomes extremely important when learning closures.

---

# 11. Execution Context Has Setup Before Execution

Consider:

```javascript
console.log(a);

var a = 10;
```

JavaScript does not simply encounter `console.log(a)` with absolutely no knowledge of `a`.

Conceptually, the execution environment is established first.

A useful simplified model is:

```text
Creation / Setup Phase
        ↓
Execution Phase
```

During setup:

```text
a → undefined
```

Then execution begins.

---

# 12. `var` Hoisting

Example:

```javascript
console.log(a);

var a = 10;

console.log(a);
```

Output:

```text
undefined
10
```

Conceptually:

### Setup

```text
a → undefined
```

### Execution

```text
console.log(a)
        ↓
undefined

a = 10
        ↓
a → 10

console.log(a)
        ↓
10
```

---

# 13. Important: `undefined` Is Not `0`

A common misconception is:

> `var` is initialized to 0.

That is incorrect.

For:

```javascript
var a;
```

the value is:

```text
undefined
```

not:

```text
0
```

JavaScript's `undefined` is an actual JavaScript value.

---

# 14. Function Declaration Hoisting

Consider:

```javascript
hello();

function hello() {
    console.log("Hello");
}
```

Output:

```text
Hello
```

Conceptually, during setup:

```text
hello → function
```

Therefore, when execution reaches:

```javascript
hello();
```

the function is available.

---

# 15. Function Expression With `var`

Now compare:

```javascript
hello();

var hello = function () {
    console.log("Hello");
};
```

This does **not** work.

Conceptually, during setup:

```text
hello → undefined
```

Then execution begins:

```javascript
hello();
```

The runtime effectively attempts to call:

```text
undefined()
```

That results in a:

```text
TypeError
```

This is different from a function declaration.

---

# 16. `let` and `const`

Now consider:

```javascript
console.log(b);

let b = 10;
```

This produces:

```text
ReferenceError
```

But it is important to understand **why**.

It is not because `let` was not parsed.

The JavaScript parser understands the declaration.

The binding exists in the lexical environment, but it has not yet been initialized.

Conceptually:

```text
b → <uninitialized>
```

---

# 17. Temporal Dead Zone

The period between entering the scope and reaching the `let` or `const` declaration is called the **Temporal Dead Zone (TDZ)**.

Example:

```javascript
console.log(b); // TDZ

let b = 10;     // TDZ ends here
```

Conceptually:

```text
Scope begins
     │
     ▼
┌────────────────────────────┐
│ Temporal Dead Zone         │
│                            │
│ console.log(b) ❌          │
│                            │
├────────────────────────────┤
│ let b = 10                 │
└────────────────────────────┘
                 ▲
                 │
             TDZ ends
```

Trying to access `b` during the TDZ causes:

```text
ReferenceError
```

---

# 18. `var` vs `let`

Compare:

```javascript
console.log(a);

var a = 10;
```

with:

```javascript
console.log(b);

let b = 10;
```

Conceptually:

```text
                 Setup

var a       → undefined

let b       → <uninitialized>
```

Then:

```text
console.log(a)
       ↓
undefined
```

while:

```text
console.log(b)
       ↓
ReferenceError
```

---

# 19. `typeof` and the TDZ

Consider:

```javascript
console.log(typeof a);

var a = 10;
```

Output:

```text
undefined
```

At that point:

```text
a → undefined
```

Therefore:

```javascript
typeof a
```

returns:

```text
"undefined"
```

Now consider:

```javascript
console.log(typeof b);

let b = 10;
```

This produces:

```text
ReferenceError
```

because `b` is in the TDZ.

---

# 20. Undeclared Variable vs Uninitialized Variable

These are different situations.

### Variable doesn't exist

```javascript
console.log(typeof somethingThatDoesNotExist);
```

Result:

```text
"undefined"
```

### `let` exists but is uninitialized

```javascript
console.log(typeof b);

let b = 10;
```

Result:

```text
ReferenceError
```

Why?

```text
Undeclared
    ↓
No binding exists

let before initialization
    ↓
Binding exists
    ↓
But it is uninitialized
    ↓
TDZ
    ↓
ReferenceError
```

This distinction is important.

---

# 21. A Complete Example

Consider:

```javascript
console.log(a);

var a = 10;

function test() {
    console.log(a);

    var a = 20;

    console.log(a);
}

test();

console.log(a);
```

Output:

```text
undefined
undefined
20
10
```

Let's understand why.

---

## Global Environment

Conceptually:

```text
Global

a → undefined

test → function
```

Execution:

```text
console.log(a)
→ undefined
```

Then:

```text
a → 10
```

---

## Calling `test()`

A new Function Execution Context is created.

The local declaration:

```javascript
var a = 20;
```

causes the local binding to exist during setup.

Conceptually:

```text
test() Environment

a → undefined
```

Therefore:

```javascript
console.log(a);
```

finds the local `a` first.

Output:

```text
undefined
```

Then:

```javascript
a = 20;
```

The local binding becomes:

```text
a → 20
```

Next:

```javascript
console.log(a);
```

outputs:

```text
20
```

When `test()` finishes, its execution context is no longer active.

The global `a` remains:

```text
a → 10
```

Therefore the final output is:

```text
10
```

---

# 22. Why Doesn't `test()` Use the Global `a`?

Because variable lookup starts in the current lexical environment.

```text
test()
  │
  ├── local a → undefined
  │
  └── global a → 10
```

The local `a` is found first.

Therefore JavaScript stops searching.

This is sometimes described as:

> Inner scope shadows outer scope.

---

# 23. Function Lookup Example

Consider:

```javascript
function printName() {
    console.log(name);
}

function outer() {
    const name = "Outer";

    printName();
}

outer();
```

This produces:

```text
ReferenceError
```

Why?

`printName()` was created in the global lexical environment.

Its lookup path is:

```text
printName()
      ↓
printName local scope
      ↓
Global lexical environment
      ↓
name found?
      ↓
NO
      ↓
ReferenceError
```

It does **not** search the caller:

```text
printName()
      ↓
outer()
      ↓
name = "Outer"
```

That would be dynamic scoping.

JavaScript uses lexical scoping instead.

---

# 24. Move the Function Inside

Now:

```javascript
function outer() {
    const name = "Outer";

    function printName() {
        console.log(name);
    }

    printName();
}

outer();
```

Output:

```text
Outer
```

Now the lexical structure is:

```text
Global
└── outer
    ├── name → "Outer"
    │
    └── printName
```

Therefore `printName()` can access the `name` from its outer lexical environment.

This is the foundation of **closures**.

---

# 25. Important Mental Model

Do not memorize:

> JavaScript moves declarations to the top.

That is an oversimplification.

Instead, use this model:

```text
JavaScript starts
       ↓
Execution Context created
       ↓
Environment established
       ↓
Bindings created
       ↓
Execution begins
       ↓
Statements execute
       ↓
Bindings receive values
```

For a simplified model:

```text
var
    ↓
undefined

let / const
    ↓
uninitialized

function declaration
    ↓
function available
```

---

# 26. Three Different States

This distinction is worth remembering:

```text
┌─────────────────────────┬──────────────────────┐
│ Situation               │ Result               │
├─────────────────────────┼──────────────────────┤
│ Binding doesn't exist   │ ReferenceError       │
│ var → undefined         │ undefined            │
│ let → uninitialized     │ ReferenceError       │
└─────────────────────────┴──────────────────────┘
```

And:

```text
let / const

uninitialized
      ↓
declaration reached
      ↓
initialized
```

---

# 27. Connection to the Call Stack

Execution contexts and the call stack work together.

Consider:

```javascript
function one() {
    two();
}

function two() {
    three();
}

function three() {
    console.log("Hello");
}

one();
```

Conceptually:

```text
one()
  ↓
two()
  ↓
three()
```

Call Stack:

```text
┌─────────────────────┐
│ three()             │
├─────────────────────┤
│ two()               │
├─────────────────────┤
│ one()               │
├─────────────────────┤
│ Global              │
└─────────────────────┘
```

Each function execution requires an execution context.

The next lesson will connect these two concepts more deeply.

---

# 28. Common Misconceptions

### ❌ "JavaScript simply executes every line from top to bottom."

Not quite.

The engine establishes execution environments before executing the statements.

---

### ❌ "`let` is not hoisted."

This is an oversimplification.

The lexical binding is created, but it remains uninitialized until execution reaches its declaration.

---

### ❌ "`var` becomes 0."

No.

It becomes:

```text
undefined
```

---

### ❌ "The parser knows that `a` is permanently a number."

JavaScript is dynamically typed.

The runtime value can change:

```javascript
let a = 10;

a = "Hello";

a = true;
```

---

### ❌ "A function can access variables from whoever called it."

Not in JavaScript.

Variable resolution follows lexical scope.

---

### ❌ "Setting a variable to null immediately destroys the object."

Not necessarily.

For example:

```javascript
let user = {
    name: "Kesav"
};

user = null;
```

The reference is removed, but garbage collection determines when unreachable memory is reclaimed.

---

# 29. Practical Experiment

Run this in Node.js:

```javascript
console.log(a);

var a = 10;

console.log(a);
```

Expected:

```text
undefined
10
```

Then:

```javascript
console.log(b);

let b = 10;
```

Expected:

```text
ReferenceError
```

Then:

```javascript
hello();

function hello() {
    console.log("Hello");
}
```

Expected:

```text
Hello
```

Finally:

```javascript
hello();

var hello = function () {
    console.log("Hello");
};
```

Expected:

```text
TypeError
```

Try to predict the result **before running the program**.

Prediction is an important part of learning runtime behavior.

---

# 30. Engineering Perspective

A beginner might say:

> "`var` gets hoisted."

A stronger explanation is:

> During execution-context setup, a `var` binding is created and initialized to `undefined`. During the execution phase, the assignment occurs when the declaration is reached.

A beginner might say:

> "`let` isn't hoisted."

A stronger explanation is:

> `let` and `const` bindings are created as part of the lexical environment but remain uninitialized until execution reaches their declaration. Access before initialization occurs inside the Temporal Dead Zone and results in a `ReferenceError`.

This difference in explanation represents deeper understanding.

---

# 31. What We Learned

We started with:

```text
JavaScript Code
```

and now understand:

```text
JavaScript Code
      ↓
Execution Context
      ↓
Lexical Environment
      ↓
Bindings
      ↓
Scope
      ↓
Scope Chain
      ↓
Variable Resolution
```

We also learned:

```text
var
 ↓
undefined during setup

let / const
 ↓
uninitialized
 ↓
TDZ

function declaration
 ↓
function available
```

---

# 32. The Bigger Picture

Our JavaScript execution model is becoming:

```text
JavaScript Source
       ↓
Parser
       ↓
AST
       ↓
Bytecode / Machine Code
       ↓
Execution Context
       ↓
Lexical Environment
       ↓
Call Stack
       ↓
Variable Resolution
       ↓
Scope Chain
       ↓
Program Execution
       ↓
Garbage Collection
```

We're now ready to connect **Execution Context + Call Stack + Lexical Scope**.

That connection leads directly to one of the most important concepts in JavaScript:

# Closures

Before we reach closures, we will first study the **Call Stack** in detail.

---

# Next Lesson

**08.11 — Call Stack: How JavaScript Tracks Function Execution**

We'll answer:

* What exactly is pushed onto the call stack?
* What happens when a function calls another function?
* What happens when a function returns?
* Why does infinite recursion cause `Maximum call stack size exceeded`?
* How does the stack relate to Execution Contexts?
* What is a stack frame?
* Why is JavaScript called single-threaded?
* How does this connect to the Event Loop later?
