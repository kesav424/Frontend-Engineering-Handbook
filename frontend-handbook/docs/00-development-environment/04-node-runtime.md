---
title: 04 - Node.js Runtime
sidebar_position: 5
description: Understand what Node.js really is, how a runtime differs from a programming language, what V8 does, and how JavaScript reaches the CPU.
---

# Node.js Runtime

> *"JavaScript is a language. Node.js is an environment that allows that language to execute outside the browser."*

---

# Reading Time

25–35 minutes

# Difficulty

⭐⭐☆☆☆

# Interview Importance

⭐⭐⭐⭐⭐

---

# Learning Objectives

After completing this chapter, you should be able to explain:

- What a runtime is
- What Node.js is
- Why JavaScript needs a runtime
- What the V8 Engine does
- The difference between JavaScript, Node.js, and V8
- Runtime APIs
- Program vs Process
- What happens when you execute `node app.js`

---

# The Problem

Imagine writing this file.

```javascript
console.log("Hello World");
```

Question:

**Who executes this code?**

Does JavaScript execute itself?

Does the operating system understand JavaScript?

The answer is **No**.

Something must translate your JavaScript into machine instructions.

That "something" is called a **runtime**.

---

# First Principle

> **A programming language cannot execute itself.**

A language is only a set of rules.

Something else must understand those rules and execute them.

---

# Mental Model

```text
You

↓

JavaScript Code

↓

Runtime (Node.js)

↓

JavaScript Engine (V8)

↓

Machine Code

↓

Operating System

↓

CPU
```

Never confuse these layers.

Each has a different responsibility.

---

# What Is JavaScript?

JavaScript is a programming language.

It defines things like:

- Variables
- Functions
- Loops
- Objects
- Arrays
- Classes
- Promises

JavaScript describes **how to write code**.

It does **not** describe **how that code runs**.

---

# What Is a Runtime?

A runtime is the environment that executes a program.

It provides:

- An execution engine
- Built-in APIs
- Memory management
- Communication with the operating system

Examples of JavaScript runtimes:

- Browser
- Node.js
- Deno
- Bun

Different runtimes provide different capabilities.

---

# Why Does JavaScript Need a Runtime?

Imagine writing instructions on paper.

The paper doesn't perform the instructions.

Someone must read them.

JavaScript is like the instructions.

The runtime is the reader.

Without a runtime:

```javascript
console.log("Hello");
```

is simply text.

---

# What Is Node.js?

Node.js is a JavaScript runtime built on Google's V8 JavaScript engine.

It allows JavaScript to run outside the browser.

Node.js provides access to operating system features such as:

- Reading files
- Writing files
- Creating web servers
- Networking
- Timers
- Environment variables

Without Node.js, JavaScript running in a browser cannot perform these operations directly.

---

# What Is V8?

V8 is Google's JavaScript engine.

Its responsibility is to:

- Parse JavaScript
- Optimize JavaScript
- Compile JavaScript into machine code
- Execute the machine code

Think of V8 as the "brain" that understands JavaScript syntax.

Node.js includes V8, but Node.js is more than just V8.

---

# Node.js ≠ V8

Many beginners think these are the same.

They are not.

```text
Node.js

├── V8 Engine
├── Runtime APIs
├── Event Loop
├── File System APIs
├── Networking APIs
└── Module System
```

V8 is one part of Node.js.

---

# Browser Runtime vs Node.js Runtime

Both execute JavaScript.

But they provide different APIs.

## Browser

Provides:

- DOM
- document
- window
- localStorage
- fetch

Example:

```javascript
document.querySelector("button");
```

Works in the browser.

Fails in Node.js.

---

## Node.js

Provides:

- fs
- path
- http
- process
- os

Example:

```javascript
import fs from "node:fs";

const text = fs.readFileSync("notes.txt", "utf8");
```

Works in Node.js.

Fails in the browser.

---

# Why Are They Different?

Because they solve different problems.

Browser:

Displays web pages safely.

Node.js:

Runs applications on your computer or server.

Their environments have different responsibilities.

---

# Runtime APIs

JavaScript itself does **not** include APIs like:

```javascript
setTimeout()

fetch()

console.log()

process

document
```

These are provided by the runtime.

Different runtimes expose different APIs.

This is why some code works in the browser but not in Node.js.

---

# Program vs Process

This distinction is often overlooked.

## Program

A program is a file containing instructions.

Example:

```
app.js
```

Nothing is happening yet.

The file simply exists.

---

## Process

A process is a running instance of a program.

When you execute:

```bash
node app.js
```

The operating system creates a new process.

That process:

- Receives memory
- Receives CPU time
- Executes your code
- Terminates when finished

---

# Internal Working

What actually happens when you run:

```bash
node app.js
```

```text
You

↓

Terminal

↓

Node.js starts

↓

Node loads app.js

↓

V8 parses JavaScript

↓

V8 compiles JavaScript

↓

Machine Code

↓

Operating System

↓

CPU executes instructions

↓

Program finishes
```

This entire process usually happens in milliseconds.

---

# Common Misconceptions

## ❌ Node.js is a programming language

No.

JavaScript is the language.

Node.js is the runtime.

---

## ❌ V8 is Node.js

No.

V8 is one component inside Node.js.

---

## ❌ JavaScript can access files directly

No.

JavaScript requires runtime APIs provided by Node.js (or another runtime).

---

## ❌ Browsers and Node.js are identical

No.

They provide different APIs because they solve different problems.

---

# Hands-on Lab

Create a file:

```javascript
console.log("Hello Runtime");
```

Run:

```bash
node app.js
```

Observe the output.

Now try:

```javascript
console.log(document);
```

Run again.

Questions:

1. What error do you see?
2. Why does it happen?
3. Which environment provides `document`?
4. Why doesn't Node.js?

Write your answers before searching online.

---

# Interview Questions

## Junior

What is Node.js?

---

## Junior

What is a runtime?

---

## Junior

What is V8?

---

## Mid-Level

Explain the difference between JavaScript, Node.js, and V8.

---

## Mid-Level

Why does `document` exist in browsers but not in Node.js?

---

## Senior

Describe what happens internally when you execute:

```bash
node app.js
```

---

# Engineering Notebook

## Rule #008

> Languages define syntax. Runtimes execute programs.

---

## Rule #009

> Different environments provide different APIs.

Never assume JavaScript code runs the same everywhere.

---

## Rule #010

> A file is not a running program.

Execution begins only when a runtime starts a process.

---

# Summary

You learned:

- JavaScript is a language.
- Node.js is a runtime.
- V8 is a JavaScript engine.
- A runtime provides APIs.
- Browsers and Node.js expose different APIs.
- Programs become processes when executed.
- JavaScript ultimately reaches the CPU through several layers.

---

# Revision Checklist

- [ ] I can explain what a runtime is.
- [ ] I can explain what Node.js is.
- [ ] I know what V8 does.
- [ ] I understand why JavaScript needs a runtime.
- [ ] I know the difference between a program and a process.
- [ ] I completed the hands-on lab.

---

# Related Chapters

Previous

- Package Management

Next

- Module 1 — Computer Fundamentals

---

> ## Key Takeaway

> **JavaScript tells the computer *what* to do. The runtime makes it possible for the computer to actually do it.**
