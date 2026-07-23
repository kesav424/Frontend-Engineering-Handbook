---
title: Operating System Part 2
sidebar_position: 2
description: Learn what an Operating System is, why it exists, and how it manages hardware, memory, processes, storage, and applications.
---

# Operating System Part 2

> "The Operating System is the manager of the computer."

---

# Learning Objectives

After this chapter, you should understand:

- Why an Operating System exists
- How it manages hardware
- What system calls are
- Why applications cannot directly access hardware
- The difference between a program and a process
- Why every process has its own memory

---

# Why Do We Need an Operating System?

A computer has hardware:

- CPU
- RAM
- SSD
- Keyboard
- Mouse
- Display
- Network Card

These devices cannot organize themselves.

If multiple applications tried to use the CPU or memory simultaneously, the computer would become unstable.

The Operating System exists to manage all hardware resources.

---

# Think of a Hotel

Imagine a hotel.

Resources:

- Rooms
- Electricity
- Water
- Staff

Guests:

- Chrome
- VS Code
- Spotify
- Node.js

The hotel manager decides:

- Who gets a room
- Who checks in
- Who checks out
- How resources are shared

The Operating System plays the same role for computer hardware.

---

# Responsibilities of the Operating System

## CPU Management

The OS decides:

- Which process runs next
- How long it runs
- When it should stop

This is called **CPU Scheduling**.

---

## Memory Management

The OS:

- Allocates RAM
- Releases RAM
- Protects process memory
- Prevents applications from reading each other's memory

---

## Storage Management

Applications never write directly to an SSD.

Flow:

VS Code

↓

Operating System

↓

File System

↓

SSD

---

## Device Management

Applications use:

- Keyboard
- Mouse
- Display
- Camera
- Printer

through the Operating System.

---

# System Calls

Applications cannot directly access hardware.

Instead they request services from the Operating System.

Example:

```javascript
fs.readFile("notes.txt")
```

Node.js asks the Operating System to read the file.

Flow:

Node.js

↓

System Call

↓

Operating System

↓

File System

↓

SSD

↓

RAM

↓

Node.js

↓

JavaScript Callback

---

# Program vs Process

This is one of the most important concepts in Computer Science.

## Program

A program is simply a file stored on disk.

Example:

Chrome.app

Calculator.exe

node

Programs do nothing until executed.

---

## Process

A process is a running instance of a program.

When you start Chrome:

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

---

# Every Process Has

- Process ID (PID)
- Own Memory
- Own Stack
- Own Heap
- CPU Scheduling Information
- Open Files
- Security Information

---

# One Program Can Have Multiple Processes

One executable file:

calculator.exe

can create:

Process A (PID 101)

Process B (PID 205)

Process C (PID 317)

Each process is independent.

---

# Process Isolation

Every process owns its own memory.

Example:

Terminal 1

```javascript
let count = 0;
count++;
console.log(count);
```

Output:

1

Terminal 2

Running the same program again also prints:

1

because the Operating System creates a completely new process with its own memory.

The variables are not shared.

---

# Under the Hood

Imagine you type:

```bash
npm start
```

Flow:

Terminal

↓

Operating System

↓

Create Process

↓

Allocate RAM

↓

Start Node.js

↓

Read package.json

↓

Execute Docusaurus

↓

V8 Parses JavaScript

↓

JavaScript Executes

↓

System Call (if needed)

↓

Operating System

↓

SSD

↓

RAM

↓

CPU Executes

↓

Terminal Output

---

# Discussion Highlights

## Why can't applications access hardware directly?

Because one application could:

- Read another application's memory
- Delete files
- Crash the system
- Create security vulnerabilities

The Operating System protects hardware resources.

---

## Why does every process have separate RAM?

If Process A changes:

```javascript
count = 5;
```

Process B should not automatically become:

```javascript
count = 5;
```

Memory isolation prevents applications from interfering with each other.

---

## Real Example

Running:

```bash
node app.js
```

creates:

Process A

Running it again creates:

Process B

Both execute the same program file.

Both have different:

- PID
- RAM
- Heap
- Stack

---

# Common Misconceptions

❌ Chrome itself is a process.

✅ Chrome.app is a program.

Opening Chrome creates one or more processes.

---

❌ Multiple processes automatically share variables.

✅ Every process owns its own memory.

---

❌ Applications talk directly to hardware.

✅ Applications communicate through the Operating System using system calls.

---

# Engineering Rules

## Rule #023

Applications do not own hardware.

---

## Rule #024

The Operating System controls hardware access.

---

## Rule #025

Applications request hardware services through system calls.

---

## Rule #026

A process owns its own memory.

Running the same program twice creates separate processes with separate memory.

---

# Interview Questions

1. What is an Operating System?

2. What is a process?

3. Difference between a program and a process?

4. What is a PID?

5. Why can't applications access hardware directly?

6. What is a system call?

7. Why does every process have its own memory?

8. What happens when you execute:

```bash
node app.js
```

9. Why does running the same program twice not share variables?

---

# Summary

The Operating System manages hardware resources.

Applications communicate with hardware using system calls.

Programs are files stored on disk.

Processes are running instances of those programs.

Every process has its own memory, PID, and execution state, allowing multiple applications—or multiple instances of the same application—to run safely without interfering with one another.