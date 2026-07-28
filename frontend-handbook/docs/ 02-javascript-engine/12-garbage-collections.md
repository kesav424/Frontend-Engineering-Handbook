---
title:  Garbage Collection  How JavaScript Frees Memory
sidebar_position: 3
description: Learn how V8 automatically frees unused memory using the Garbage Collector, Mark-and-Sweep algorithm, and Generational Heap.
---

# Garbage Collection — How JavaScript Frees Memory

> *"Allocating memory is only half the story. The harder problem is knowing when memory is no longer needed."*

---

# Learning Objectives

After completing this chapter, you will understand:

- Why memory must be freed
- What Garbage Collection (GC) is
- What "reachable" and "unreachable" objects are
- The Mark-and-Sweep algorithm
- Young Generation vs Old Generation
- Minor GC vs Major GC
- Why memory leaks still happen in JavaScript

---

# Prerequisites

Before reading this chapter, you should understand:

- RAM
- Heap
- Stack
- Execution Contexts
- Objects and References

---

# Where We Are

Our journey now looks like this:

```text
JavaScript

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

Execution Context

↓

Stack

↓

Heap

↓

???
```

Your application is running.

Objects are continuously being created.

But here's the question:

**Who cleans them up?**

---

# Imagine There Was No Garbage Collector

Consider this code:

```javascript
function createUser() {
    return {
        name: "Kesav",
        age: 26
    };
}

createUser();
createUser();
createUser();
createUser();
```

Every function call creates a new object.

```
Heap

Object

Object

Object

Object

Object

Object

Object
```

If nothing removed them...

Memory would continue growing forever.

Eventually:

```
Heap Full

↓

Application Crash
```

---

# Languages Without Automatic GC

In C or C++, you might write:

```cpp
Person* p = new Person();

// use it

delete p;
```

The programmer is responsible for freeing memory.

If they forget:

```
Memory Leak
```

If they free memory twice:

```
Crash
```

JavaScript avoids this by automatically managing memory.

---

# What Is Garbage Collection?

Garbage Collection is the process of automatically freeing memory that is no longer reachable by your program.

The keyword here is:

> **Reachable**

---

# Reachable vs Unreachable

```javascript
let user = {
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

{
 name:"Kesav"
}
```

The object is reachable because `user` points to it.

---

Now:

```javascript
user = null;
```

Memory becomes:

```text
Stack

user

null

────────────

Heap

{
 name:"Kesav"
}
```

Nothing points to the object anymore.

It has become **unreachable**.

---

# Does Memory Free Immediately?

No.

Many beginners think:

```javascript
user = null;
```

Immediately deletes the object.

It does not.

Instead:

```
Object

↓

Unreachable

↓

Wait for Garbage Collector

↓

Memory Reclaimed
```

The Garbage Collector decides **when** to reclaim memory.

---

# Mark-and-Sweep Algorithm

Modern JavaScript engines use a variation of the **Mark-and-Sweep** algorithm.

It has two main phases.

---

## Phase 1 — Mark

The Garbage Collector starts from root objects.

Examples of roots include:

- Global variables
- Current Execution Context
- Active Call Stack
- Closures

It follows every reference it can reach.

```text
Global

↓

user

↓

address

↓

city
```

Every reachable object is marked.

---

## Phase 2 — Sweep

After marking:

Anything **not marked** is considered garbage.

```
Heap

✓ Reachable

✓ Reachable

❌ Unreachable

✓ Reachable

❌ Unreachable
```

The unreachable objects are removed.

Their memory becomes available for future allocations.

---

# Visual Example

Before GC:

```text
Heap

┌──────────────┐
│ User         │◄──── Stack
├──────────────┤
│ Address      │
├──────────────┤
│ Old Array    │
├──────────────┤
│ Temp Object  │
└──────────────┘
```

After Mark:

```text
✓ User

✓ Address

❌ Old Array

❌ Temp Object
```

After Sweep:

```text
Heap

┌──────────────┐
│ User         │
├──────────────┤
│ Address      │
├──────────────┤
│ Free Space   │
└──────────────┘
```

---

# Why Doesn't GC Run Constantly?

Garbage Collection takes CPU time.

If GC ran after every object creation:

```javascript
{}
{}
{}
{}
{}
```

Performance would be terrible.

Instead, V8 runs GC only when it decides it's worthwhile.

---

# The Generational Heap

V8 divides the heap into two major areas.

```text
Heap

├── Young Generation
└── Old Generation
```

This is based on an observation:

> Most objects die young.

---

# Young Generation

Suppose:

```javascript
for (let i = 0; i < 1000; i++) {
    const temp = {};
}
```

Almost every object disappears quickly.

These objects stay in the **Young Generation**.

---

# Old Generation

Some objects survive for a long time.

Example:

```javascript
const appConfig = {
    api: "...",
    version: "1.0"
};
```

It lives during the entire application.

Eventually V8 promotes it to the **Old Generation**.

---

# Minor GC

Minor Garbage Collection focuses on:

```
Young Generation
```

It's:

- Fast
- Frequent
- Cheap

---

# Major GC

Major Garbage Collection focuses on:

```
Old Generation
```

It's:

- Slower
- Less frequent
- More expensive

---

# Memory Leak

JavaScript has automatic memory management.

But memory leaks are still possible.

Example:

```javascript
const cache = [];

function save(data) {
    cache.push(data);
}
```

Every object remains referenced forever.

```
cache

↓

Object

↓

Object

↓

Object

↓

Object
```

The Garbage Collector cannot remove them because they are still reachable.

---

# Common Sources of Memory Leaks

- Global variables
- Large caches
- Event listeners not removed
- Timers never cleared
- Closures holding unnecessary references
- Detached DOM nodes (in browsers)

---

# Under the Hood

V8's Garbage Collector is highly optimized.

It uses techniques such as:

- Mark-and-Sweep
- Generational Collection
- Incremental Marking
- Concurrent Marking
- Memory Compaction

These reduce pauses and improve performance.

---

# Engineering Thinking

Many developers think:

> "JavaScript automatically deletes unused objects."

An engineer thinks:

> "The Garbage Collector periodically identifies unreachable objects by traversing the object graph from root references, then reclaims their memory using optimized collection algorithms."

That distinction is important.

---

# Try It Yourself

Run:

```javascript
function test() {
    let user = {
        name: "Kesav"
    };

    user = null;
}

test();
```

Question:

When does the object disappear?

Answer:

Not immediately.

It disappears when the Garbage Collector decides to reclaim unreachable memory.

---

# Common Misconceptions

❌ `user = null` instantly deletes the object.

✅ It only removes your reference.

---

❌ JavaScript never leaks memory.

✅ Reachable objects can still cause memory leaks.

---

❌ Garbage Collection is free.

✅ GC requires CPU time and careful optimization.

---

# Performance Notes

Creating millions of temporary objects increases GC work.

Reusing objects where appropriate and avoiding unnecessary allocations can reduce memory pressure in performance-critical code.

---

# Interview Corner

### Beginner

1. What is Garbage Collection?
2. What is a memory leak?
3. What does "reachable" mean?

### Intermediate

4. Explain Mark-and-Sweep.
5. Why doesn't GC run after every allocation?
6. What is the Young Generation?

### Advanced

7. Why is the heap divided into generations?
8. What causes memory leaks even in JavaScript?
9. What is the difference between Minor and Major GC?

---

# Summary

JavaScript automatically manages memory using a Garbage Collector.

Objects remain in memory as long as they are reachable from your running program.

When objects become unreachable, the Garbage Collector eventually identifies and removes them, allowing their memory to be reused.

Understanding Garbage Collection is essential for writing efficient applications and avoiding memory leaks.

---

# What Actually Happened So Far

```text
✓ JavaScript parsed
✓ AST created
✓ Bytecode generated
✓ Ignition executing
✓ TurboFan optimized hot code
✓ Stack created
✓ Heap allocated
✓ Objects created
✓ References tracked
✓ Garbage Collector monitoring memory

NEXT →

We'll explore the Execution Context, the environment JavaScript creates every time code runs.
```

---

# Next Chapter

➡ **Execution Context: The Environment Where JavaScript Runs**

We'll answer:

- What exactly is an Execution Context?
- How does JavaScript know where variables live?
- What happens before the first line of code executes?
- What is the Creation Phase?
- What is the Execution Phase?
- How do `var`, `let`, and `const` behave differently during initialization?