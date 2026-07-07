---
title: 01 - computer fundamentals
sidebar_position: 1
description: Learn what a computer really is, how it executes instructions, and why understanding computers makes you a better frontend engineer.
---

# What Is a Computer?

> *"Every line of code you write eventually becomes electrical signals flowing through billions of tiny transistors."*

---

# Reading Time

25–30 minutes

# Difficulty

⭐☆☆☆☆

# Interview Importance

⭐⭐⭐⭐⭐

---

# Learning Objectives

By the end of this chapter you will understand:

- What a computer actually is.
- Why computers exist.
- How computers execute programs.
- Why software cannot exist without hardware.
- The relationship between applications, operating systems, CPUs, and memory.
- Why frontend engineers should understand computer fundamentals.

---

# The Problem

You open VS Code.

You write:

```javascript
console.log("Hello");
```

You press Save.

Then you type:

```bash
node app.js
```

A second later...

```
Hello
```

appears on your screen.

Seems simple.

But think about what actually happened.

Questions:

- Who read your file?
- Who understood JavaScript?
- Who displayed the text?
- Who used the CPU?
- Where was your program stored?
- How did electricity become the word "Hello"?

To answer these questions, we first need to understand what a computer really is.

---

# First Principle

> A computer does **not** understand JavaScript.

It does not understand Python.

It does not understand C++.

It only understands **machine instructions**.

Everything else is translated before the CPU can execute it.

---

# What Is a Computer?

A computer is a programmable machine that:

- receives input
- processes data
- stores information
- produces output

That's all.

Everything else—games, websites, operating systems, browsers—is built on top of these four abilities.

---

# Mental Model

```
Input

↓

Processing

↓

Storage

↓

Output
```

Every application follows this cycle.

Examples:

Instagram

```
Touch Screen

↓

CPU

↓

Database

↓

Phone Screen
```

Calculator

```
Keyboard

↓

CPU

↓

Memory

↓

Display
```

Browser

```
Mouse

↓

CPU

↓

RAM

↓

Monitor
```

---

# The Four Major Components

Every computer contains four fundamental parts.

```
Computer

├── CPU
├── Memory (RAM)
├── Storage (SSD/HDD)
└── Input / Output Devices
```

Everything else supports these components.

---

# CPU

The CPU is the brain of the computer.

Its job is surprisingly simple.

It repeatedly performs this cycle:

```
Fetch

↓

Decode

↓

Execute

↓

Repeat
```

Millions or billions of times every second.

The CPU never gets tired.

It simply follows instructions.

---

# Memory (RAM)

RAM is the computer's short-term memory.

Programs that are currently running live here.

When you close an application, most of its RAM is released.

Think of RAM as your desk while working.

Large.

Fast.

Temporary.

---

# Storage

Storage keeps data permanently.

Examples:

- SSD
- Hard Drive

Files like:

```
app.js

photo.png

resume.pdf
```

live in storage.

They are **not running**.

They're simply stored.

---

# Input Devices

Input allows humans to communicate with computers.

Examples:

- Keyboard
- Mouse
- Microphone
- Camera
- Touchscreen

---

# Output Devices

Output allows computers to communicate with humans.

Examples:

- Monitor
- Speakers
- Printer

---

# The Journey of a JavaScript File

Imagine this file.

```javascript
console.log("Hello");
```

Initially...

```
Storage

↓

app.js
```

Nothing happens.

The file is just data.

When you run:

```bash
node app.js
```

Something changes.

```
Storage

↓

RAM

↓

Node.js

↓

V8

↓

CPU

↓

Output
```

Notice something important.

Your file must first leave storage.

Only then can it execute.

---

# Hardware vs Software

Hardware is physical.

Software is instructions.

Hardware examples:

- CPU
- RAM
- SSD
- Keyboard

Software examples:

- Chrome
- VS Code
- Node.js
- Windows
- macOS

Software cannot exist without hardware.

Hardware is useless without software.

They depend on each other.

---

# Internal Working

Imagine opening Chrome.

The computer roughly performs these steps.

```
Find chrome.exe

↓

Read file from SSD

↓

Load into RAM

↓

Operating System starts process

↓

CPU begins execution

↓

Chrome appears
```

This happens in fractions of a second.

---

# Common Misconceptions

## ❌ The CPU stores programs.

No.

Programs are stored on the SSD.

They move into RAM before execution.

---

## ❌ RAM permanently stores files.

No.

RAM is temporary.

---

## ❌ Software runs itself.

No.

The CPU executes machine instructions.

---

## ❌ JavaScript directly reaches the CPU.

No.

Several layers exist between them.

---

# Hands-on Lab

Open Activity Monitor (macOS) or Task Manager (Windows).

Observe:

- Chrome
- VS Code
- Terminal

Questions:

1. Which applications consume the most memory?
2. What happens when you close Chrome?
3. Does memory usage decrease?
4. Why?

Write your observations.

---

# Interview Questions

## Junior

What is a computer?

---

## Junior

What is RAM?

---

## Junior

What is the CPU?

---

## Mid-Level

Why can't programs execute directly from storage?

---

## Mid-Level

Explain the difference between RAM and SSD.

---

## Senior

Describe what happens after double-clicking an application.

---

# Engineering Notebook

## Rule #011

> Files are stored.

Programs execute.

Never confuse storage with execution.

---

## Rule #012

> Every running application consumes memory.

No RAM.

No running program.

---

# Summary

In this chapter you learned:

- What a computer is.
- The four major components.
- Hardware vs Software.
- CPU.
- RAM.
- Storage.
- Input and Output.
- The journey from file to execution.

---

# Revision Checklist

- [ ] I can explain what a computer is.
- [ ] I understand the role of the CPU.
- [ ] I understand RAM.
- [ ] I understand storage.
- [ ] I know the difference between hardware and software.
- [ ] I completed the Activity Monitor lab.

---

# Related Chapters

Previous

- Module 0

Next

- CPU Architecture

---

> ## Key Takeaway

> **Every application you use is ultimately just instructions executed by a CPU.**