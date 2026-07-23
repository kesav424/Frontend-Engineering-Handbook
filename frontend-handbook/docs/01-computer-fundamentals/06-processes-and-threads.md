---
title: Processes and Threads
sidebar_position: 2
description: Understand the difference between programs, processes, and threads, and why operating systems use them.
---

# Processes and Threads

> "A process owns resources. A thread executes instructions."

---

# Learning Objectives

After this chapter you should understand:

- What is a program?
- What is a process?
- What is a thread?
- Difference between a program, process, and thread
- Why multiple processes exist
- Why multiple threads exist
- What is a race condition?
- Why JavaScript is called single-threaded

---

# From Program to Process

Imagine you install Chrome.

```
Chrome.app
```

This file is stored on your SSD.

At this point nothing is running.

It is simply a **program**.

When you double-click Chrome:

```
Chrome.app

↓

Operating System

↓

Create Process

↓

Assign PID

↓

Allocate RAM

↓

Schedule CPU Time

↓

Chrome Starts
```

The running version of a program is called a **process**.

---

# Program vs Process

## Program

A program is:

- A file
- Stored on SSD
- Not executing
- Passive

Examples:

- Chrome.app
- node
- calculator.exe

---

## Process

A process is:

- A running instance of a program
- Created by the Operating System
- Has its own PID
- Has its own memory
- Receives CPU time

---

# Every Process Owns

When the OS creates a process, it allocates:

- PID (Process ID)
- RAM
- Heap
- Stack
- CPU scheduling information
- Open file handles
- Security information

Each process is isolated from every other process.

---

# One Program Can Create Multiple Processes

One executable file:

```
calculator.exe
```

can create:

```
Process A

PID = 101

RAM A
```

```
Process B

PID = 205

RAM B
```

```
Process C

PID = 317

RAM C
```

The executable file is shared.

The running state is not.

---

# Memory Isolation

Imagine this program:

```javascript
let count = 0;

count++;

console.log(count);
```

Open two terminals.

Run:

```bash
node app.js
```

in both.

Output:

```
1
```

```
1
```

Not:

```
1
2
```

Why?

Because every process owns its own memory.

The variables inside one process do not automatically appear inside another.

---

# Under the Hood

```
SSD

app.js

↓

Operating System

↓

Create Process

↓

Allocate RAM

↓

Assign PID

↓

Start Node.js

↓

Execute JavaScript
```

The process exists only while the program is running.

When the process exits:

- RAM is released
- PID becomes available for reuse
- The program file still remains on the SSD

---

# Why Processes?

Processes provide:

- Isolation
- Stability
- Security

If one process crashes:

```
Chrome

❌ Crash
```

VS Code can continue running because it is a different process.

---

# What is a Thread?

A process owns resources.

A thread performs work.

Think of a restaurant.

Restaurant:

- Kitchen
- Tables
- Money
- Ingredients

These are resources.

The chefs actually cook.

The chefs are like **threads**.

```
Process

RAM

Heap

Stack

↓

Thread

↓

CPU Executes Instructions
```

The CPU executes **threads**, not processes.

---

# One Process Can Have Multiple Threads

Example:

```
Chrome Process

├── UI Thread
├── Network Thread
├── Audio Thread
├── GPU Thread
└── JavaScript Main Thread
```

Each thread performs a different task while sharing the same process resources.

---

# Shared Memory

Unlike processes:

Threads inside the same process share memory.

```
Process

count = 0

├── Thread A
└── Thread B
```

Both threads can access:

```
count
```

This improves performance but introduces new challenges.

---

# Race Condition

Suppose two threads execute:

```javascript
count++;
```

at the same time.

Internally, `count++` is roughly:

```
Read

↓

Add 1

↓

Write
```

Possible execution:

```
Thread A reads 0

Thread B reads 0

Thread A writes 1

Thread B writes 1
```

Final value:

```
1
```

Expected:

```
2
```

This problem is called a **race condition**.

Multiple threads race to update shared memory.

---

# Why Use Multiple Threads?

Imagine a browser.

Without multiple threads:

- Download file
- Draw UI
- Handle mouse click
- Play audio

Everything would happen one after another.

With multiple threads:

```
Browser Process

├── UI Thread
├── Network Thread
├── Audio Thread
└── GPU Thread
```

Many tasks can happen simultaneously.

Benefits:

- Better CPU utilization
- Better responsiveness
- Less waiting
- Improved user experience

---

# JavaScript and Threads

People often say:

> JavaScript is single-threaded.

This statement is often misunderstood.

JavaScript code runs on one **main JavaScript thread**.

However, the runtime (Browser or Node.js) uses many threads behind the scenes for networking, file I/O, timers, rendering, and other tasks.

So:

Browser ≠ JavaScript

Node.js ≠ JavaScript

The runtime is multithreaded.

JavaScript execution is typically single-threaded.

---

# Discussion Highlights

## Our Restaurant Analogy

A process is like a restaurant.

It owns:

- Kitchen
- Tables
- Ingredients

Threads are the chefs.

The chefs perform the work.

The restaurant owns the resources.

---

## Our Discovery About Processes

Initially, we thought opening the same application twice might share data.

We discovered that:

- The executable file is shared.
- The running memory is not.

Every process owns independent RAM.

---

## Our Discovery About Threads

When asked what happens if two threads execute:

```javascript
count++;
```

at exactly the same time, we reasoned:

Both threads may read:

```
0
```

Both calculate:

```
1
```

Both write:

```
1
```

This naturally led us to discovering the concept of a race condition.

---

# Common Misconceptions

❌ A process executes instructions.

✅ Threads execute instructions.

---

❌ One process means one thread.

✅ A process may contain many threads.

---

❌ Multiple processes share variables.

✅ Every process has its own memory.

---

❌ JavaScript being single-threaded means browsers have only one thread.

✅ Browsers and Node.js use many threads. JavaScript code itself usually runs on one main thread.

---

# Engineering Rules

## Rule #027

A process owns resources.

---

## Rule #028

A thread is the unit of execution inside a process.

---

## Rule #029

Multiple threads inside the same process share memory.

---

## Rule #030

Multiple processes do not automatically share memory.

---

## Rule #031

Race conditions occur when multiple threads modify shared memory without proper synchronization.

---

# Interview Questions

1. What is the difference between a program and a process?
2. What is a PID?
3. What resources does a process own?
4. What is a thread?
5. Why do processes use multiple threads?
6. Why can race conditions happen?
7. Why is JavaScript called single-threaded?
8. Can one program create multiple processes?
9. Why doesn't running `node app.js` twice share variables?
10. What is the difference between a process and a thread?

---

# Summary

A program is a file stored on disk.

A process is a running instance of that program created by the Operating System.

Every process owns its own memory and resources.

Threads are the units of execution inside a process. The CPU executes threads, not processes.

Multiple threads improve responsiveness and CPU utilization, but because they share memory, they can introduce race conditions.

JavaScript typically executes on a single main thread, while browsers and Node.js use multiple threads internally to perform background work.