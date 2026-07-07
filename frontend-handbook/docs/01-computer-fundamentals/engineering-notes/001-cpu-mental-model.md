---
title: Engineering Notebook 001 - CPU Mental Model
sidebar_position: 1
---

# Engineering Notebook 001 — CPU Mental Model

> These are my personal engineering notes while learning Computer Fundamentals.

---

# Key Ideas

## A computer only understands machine instructions.

Programming languages like JavaScript, Python, or Java are designed for humans.

Before the CPU can execute them, they must be translated into machine instructions.

---

## JavaScript execution path

```text
JavaScript Source Code

↓

Node.js / Browser Runtime

↓

JavaScript Engine (V8)

↓

Machine Instructions

↓

CPU
```

The CPU never executes JavaScript directly.

---

## The CPU has one responsibility

Execute instructions.

It continuously repeats:

```text
Fetch

↓

Decode

↓

Execute

↓

Repeat
```

Billions of times every second.

---

## A program is not a process

Program

```text
app.js
```

A file stored on disk.

Process

```text
Running Node.js
```

A program that is currently executing.

---

## The Operating System creates processes

When running:

```bash
node app.js
```

The Operating System:

- Creates a process
- Allocates memory
- Loads Node.js into RAM
- Gives the process CPU time

Node.js then starts executing my JavaScript.

---

## One CPU Core executes one thread at a time

Even though many applications appear to run simultaneously:

- Chrome
- VS Code
- Spotify
- Terminal

A single CPU core executes only one thread at any instant.

---

## CPU Scheduling

The Operating System rapidly switches CPU time between runnable threads.

Conceptually:

```text
Chrome

↓

VS Code

↓

Spotify

↓

Node.js

↓

Chrome

↓

...
```

This happens extremely quickly, creating the illusion that everything runs simultaneously.

---

## Why my computer doesn't freeze

Even if my Node.js application contains:

```javascript
while (true) {}
```

The Operating System continues scheduling other processes.

The Node.js process becomes unresponsive, but the entire computer usually remains usable because other processes continue receiving CPU time.

---

## Context Switching

When switching from one running task to another, the Operating System saves the CPU state of the current thread and restores the saved state of another.

This allows applications to continue exactly where they previously stopped.

---

# My Mental Model

```text
You

↓

Terminal

↓

Operating System

↓

Create Process

↓

Load Node.js

↓

Read app.js

↓

V8 parses JavaScript

↓

V8 compiles JavaScript

↓

Machine Instructions

↓

CPU

↓

Fetch

↓

Decode

↓

Execute

↓

Node.js

↓

Operating System

↓

Terminal

↓

Output
```

---

# Important Distinctions

| Concept | Meaning |
|----------|---------|
| Language | JavaScript syntax and rules |
| Runtime | Executes JavaScript (Node.js, Browser) |
| Engine | Converts JavaScript into machine instructions (V8) |
| Program | A file stored on disk |
| Process | A running instance of a program |
| CPU | Executes machine instructions |
| Operating System | Manages processes, memory, and CPU scheduling |

---

# Engineering Rules

## Rule #013

The CPU executes machine instructions, not programming languages.

---

## Rule #014

A program becomes useful only when it becomes a running process.

---

## Rule #015

The Operating System decides which process or thread gets CPU time.

---

## Rule #016

Concurrency is often an illusion created by extremely fast CPU scheduling.

---

# Interview Takeaways

I should be able to explain:

- Why the CPU cannot execute JavaScript directly.
- What a runtime is.
- The difference between a program and a process.
- What the CPU actually does.
- Why one CPU core can appear to run multiple applications.
- What CPU scheduling is.
- Why an infinite loop doesn't usually freeze the entire operating system.

---

# Questions I Should Always Ask

Instead of asking:

> "What is Node.js?"

Ask:

> "Where does Node.js fit into the execution pipeline?"

Instead of asking:

> "What is a CPU?"

Ask:

> "What is the CPU actually doing while my program runs?"

Understanding systems is more valuable than memorizing definitions.