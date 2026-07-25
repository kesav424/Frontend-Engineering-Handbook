---
title: JavaScript Memory Stack, Heap, and Execution Context
sidebar_position: 3
description: Learn how JavaScript stores variables, objects, functions, and execution contexts in memory using the Stack and Heap.
---

# JavaScript Memory — Stack, Heap, and Execution Context

> *"JavaScript doesn't magically remember your variables. Every value has a place in memory, and understanding where it lives is one of the biggest steps toward becoming an engineer."*

---

# Learning Objectives

After completing this chapter, you will understand:

- How JavaScript uses memory
- What the Call Stack is
- What the Heap is
- What an Execution Context is
- Where primitive values are stored
- Where objects and arrays are stored
- Why objects behave differently from primitive values
- What causes a Stack Overflow

---

# Prerequisites

Before reading this chapter, you should understand:

- CPU Fundamentals
- RAM
- Processes
- Virtual Memory
- V8
- Ignition
- TurboFan

---

# Where We Are

Our journey:

```text
JavaScript

↓

Lexer

↓

Parser

↓

AST

↓

Bytecode

↓

Ignition

↓

TurboFan

↓

Machine Code

↓

Program Running

↓

???
```

Your JavaScript is finally executing.

Now the question becomes:

> **Where is everything stored?**

---

# A Simple Program

```javascript
let age = 25;

const user = {
    name: "Kesav",
    city: "Chennai"
};

function greet() {
    console.log(user.name);
}

greet();
```

Where are:

- age?
- user?
- greet()?
- name?
- city?

The answer is:

Not all in the same place.

---

# JavaScript Memory

V8 mainly uses two areas:

```text
JavaScript Memory

├── Call Stack
└── Heap
```

Each has a completely different purpose.

---

# Big Picture

```text
                   JavaScript Memory

        ┌────────────────────────────────┐
        │           Call Stack           │
        └───────────────┬────────────────┘
                        │
                        │ references
                        ▼
        ┌────────────────────────────────┐
        │              Heap              │
        └────────────────────────────────┘
```

---

# The Call Stack

Think of the Call Stack as a pile of books.

```
Top

Book 3

Book 2

Book 1

Bottom
```

The last book placed on top is the first one removed.

This is called:

```text
LIFO

Last In

First Out
```

---

# Why Is It Called a Stack?

Imagine stacking plates.

```
🍽

🍽

🍽

🍽
```

You always remove the top plate first.

The Call Stack works exactly like this.

---

# What Lives on the Stack?

The stack stores:

- Function calls
- Local variables
- Primitive values
- Return addresses
- Execution Contexts

---

# Primitive Values

Example:

```javascript
let age = 25;
```

Conceptually:

```
Call Stack

──────────────

age

25
```

Primitive values are small.

They fit nicely on the stack.

---

# What Is an Execution Context?

Every time JavaScript executes code, V8 creates an **Execution Context**.

Think of it as a workspace.

It contains:

- Variables
- Function parameters
- this
- Scope information

Without an Execution Context, JavaScript wouldn't know where variables belong.

---

# Global Execution Context

When your program starts:

```javascript
console.log("Hello");
```

V8 creates:

```
Global Execution Context
```

This is the very first frame on the Call Stack.

---

# Function Execution Context

Now:

```javascript
function greet() {
    console.log("Hi");
}

greet();
```

Stack:

```
Top

greet()

────────────

Global

Bottom
```

When `greet()` finishes:

```
Top

Global

Bottom
```

The function frame disappears.

---

# Visual Example

```text
Step 1

Stack

┌──────────────┐
│ Global       │
└──────────────┘
```

Call function:

```text
Step 2

┌──────────────┐
│ greet()      │
├──────────────┤
│ Global       │
└──────────────┘
```

Return:

```text
Step 3

┌──────────────┐
│ Global       │
└──────────────┘
```

---

# Nested Functions

```javascript
function one() {
    two();
}

function two() {
    three();
}

function three() {}
```

Stack:

```
Top

three()

two()

one()

Global

Bottom
```

Functions keep stacking until they return.

---

# Enter the Heap

Objects are different.

Example:

```javascript
const user = {
    name: "Kesav"
};
```

Does the whole object live on the stack?

No.

Objects live in the **Heap**.

---

# Why?

Objects can be:

- Large
- Dynamic
- Grow at runtime

Imagine:

```javascript
const users = [];
```

Later:

```javascript
users.push(...100000 objects...)
```

The object keeps growing.

The stack is too small for that.

---

# Heap Memory

Conceptually:

```
Heap

──────────────────

Object A

Object B

Array

Function

Map

Set

Date

Promise

...
```

The heap is much larger than the stack.

---

# Reference

Suppose:

```javascript
const user = {
    name: "Kesav"
};
```

Memory:

```text
Stack

user

↓

0x1000

────────────

Heap

0x1000

{name:"Kesav"}
```

Notice:

The stack stores the **reference**.

The heap stores the **actual object**.

---

# Arrays

```javascript
const numbers = [1,2,3];
```

Memory:

```
Stack

numbers

↓

0x2500

────────────

Heap

[1,2,3]
```

Again:

Only the address is on the stack.

---

# Why Objects Behave Differently

Example:

```javascript
const a = {
    age: 25
};

const b = a;
```

Memory:

```
Stack

a

↓

0x1000

b

↓

0x1000

────────────

Heap

{
 age:25
}
```

Both variables point to the same object.

---

# Primitive Copy

```javascript
let a = 10;

let b = a;
```

Memory:

```
Stack

a

10

b

10
```

Changing `b` doesn't affect `a`.

---

# Object Reference

```javascript
const a = {
    age:25
};

const b = a;

b.age = 50;
```

Result:

```javascript
console.log(a.age);
```

Output:

```
50
```

Why?

Because both variables reference the same heap object.

---

# Function Memory

Functions are objects too.

```javascript
function hello() {}
```

Memory:

```
Stack

hello

↓

0x5000

────────────

Heap

Function Object
```

---

# Stack Overflow

Consider:

```javascript
function run() {
    run();
}

run();
```

What happens?

Every call creates another Execution Context.

```
run()

run()

run()

run()

run()

run()

...
```

Eventually:

```
Maximum call stack size exceeded
```

The stack runs out of space.

---

# Stack vs Heap

| Stack | Heap |
|--------|------|
| Small | Large |
| Fast | Slower |
| Automatic | Dynamic |
| Stores execution contexts | Stores objects |
| LIFO | No fixed order |

---

# Visual Memory Layout

```text
                JavaScript Memory

┌────────────────────────────────────────────┐
│                Call Stack                  │
├────────────────────────────────────────────┤
│ greet()                                   │
├────────────────────────────────────────────┤
│ Global Execution Context                  │
└────────────────────────────────────────────┘


┌────────────────────────────────────────────┐
│                  Heap                      │
├────────────────────────────────────────────┤
│ Object                                    │
│ Array                                     │
│ Function                                  │
│ Promise                                   │
│ Map                                       │
│ Set                                       │
└────────────────────────────────────────────┘
```

---

# Under the Hood

When JavaScript executes:

```javascript
const user = {
    name: "Kesav"
};
```

V8 roughly performs:

1. Allocate memory in Heap
2. Store object
3. Return heap address
4. Store reference in Stack

This entire process happens in microseconds.

---

# Engineering Thinking

Many developers think:

> "Variables are stored in memory."

An engineer thinks:

> "Primitive values typically live within execution contexts on the Call Stack, while objects, arrays, and functions are allocated in the Heap. Stack variables often store references that point to heap objects."

This mental model explains why copying primitives and copying objects behave differently.

---

# Try It Yourself

Predict the output:

```javascript
const user = {
    age: 25
};

const admin = user;

admin.age = 50;

console.log(user.age);
```

Then run it.

Can you explain why the answer is **50**?

---

# Common Misconceptions

❌ Objects are copied when assigned.

✅ Only their references are copied.

---

❌ Arrays are stored on the stack.

✅ Arrays are allocated in the heap.

---

❌ Functions only exist while running.

✅ Function objects exist in the heap; function calls create execution contexts on the stack.

---

❌ The stack stores every JavaScript value.

✅ The stack mainly stores execution contexts, local values, and references.

---

# Performance Notes

Accessing stack memory is generally very fast because it follows a predictable LIFO structure.

Heap allocation is more flexible but requires memory management and garbage collection.

This is one reason creating millions of short-lived objects can affect performance.

---

# Security Notes

Every JavaScript process has its own heap and stack.

One Node.js process cannot directly access the heap of another process.

Memory isolation is provided by the operating system and the JavaScript engine.

---

# Interview Corner

### Beginner

1. What is the Call Stack?
2. What is the Heap?
3. What is an Execution Context?

### Intermediate

4. Why are objects stored in the Heap?
5. Why are primitive values copied by value?
6. Why are objects copied by reference?

### Advanced

7. What causes a Stack Overflow?
8. What is stored inside an Execution Context?
9. Why is stack allocation generally faster than heap allocation?

---

# Summary

JavaScript manages memory using two primary areas:

- The **Call Stack**, which stores execution contexts, local variables, and references.
- The **Heap**, which stores dynamically allocated objects, arrays, functions, and other complex values.

Every function call creates a new execution context on the stack. Objects live in the heap, and variables usually store references to them.

Understanding this model explains some of JavaScript's most important behaviors, including object mutation, reference sharing, and stack overflow errors.

---

# What Actually Happened So Far

```text
✓ JavaScript source parsed
✓ AST created
✓ Bytecode generated
✓ Ignition started execution
✓ TurboFan optimized hot functions
✓ Global Execution Context created
✓ Call Stack initialized
✓ Objects allocated in Heap
✓ References stored in Stack
✓ Functions executing normally

NEXT →

How does JavaScript free memory that is no longer needed? We'll discover Garbage Collection.
```

---

# Next Chapter

➡ **Garbage Collection: How JavaScript Frees Memory**

In the next chapter, you'll learn:

- Why memory leaks happen
- What "reachable" means
- Mark-and-Sweep Garbage Collection
- The Generational Heap
- Minor GC vs Major GC
- Why setting variables to `null` doesn't instantly free memory
- How V8 automatically manages memory without manual `free()` calls

By the end of the chapter, you'll understand why JavaScript developers rarely think about memory allocation—but still need to understand memory management.