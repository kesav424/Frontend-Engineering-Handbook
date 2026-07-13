---
title: CPU - The Brain of the Computer
sidebar_label: 02 - CPU - The Brain of the Computer
sidebar_position: 2
description: Understand what a CPU is, how it executes instructions, and why every program ultimately depends on it.
---

# CPU – The Brain of the Computer

> *"The CPU has one job: execute instructions. Everything else in computing exists to provide those instructions."*

---

# 🤔 Think Before Reading

Before reading, answer these questions without searching:

1. Why can't a computer execute a `.js` file directly?
2. How does a calculator perform millions of calculations every second?
3. What is the CPU actually doing while your program is running?
4. If a CPU is so fast, why do programs sometimes feel slow?

Don't worry if you're unsure. We'll answer them together.

---

# Reading Time

30–40 minutes

# Difficulty

⭐☆☆☆☆

# Interview Importance

⭐⭐⭐⭐⭐

---

# Learning Objectives

By the end of this chapter, you will be able to:

- Explain what a CPU is.
- Understand why every program depends on the CPU.
- Describe the Fetch–Decode–Execute cycle.
- Understand why CPUs don't understand JavaScript.
- Explain why faster CPUs don't always make applications faster.

---

# The Real Problem

Imagine this program:

```javascript
console.log("Hello, World!");
```

Who prints "Hello, World!"?

- JavaScript?
- Node.js?
- VS Code?
- The Operating System?

None of them.

Eventually, **the CPU** performs the work.

Every application—from Chrome to Photoshop to VS Code—ultimately becomes instructions executed by the CPU.

---

# First Principle

> **The CPU only understands machine instructions.**

Everything else—JavaScript, Python, Java, C++, Rust—is translated before reaching the CPU.

---

# What Is a CPU?

CPU stands for **Central Processing Unit**.

It is the component responsible for executing instructions.

Think of it as the worker that follows a never-ending list of commands.

It does not think.

It does not guess.

It simply executes instructions one after another.

---

# Mental Model

Imagine a chef in a kitchen.

The recipe says:

1. Crack the egg.
2. Mix it.
3. Heat the pan.
4. Cook.

The chef doesn't ask *why*.

The chef follows the instructions.

The CPU behaves the same way.

---

# The Fetch–Decode–Execute Cycle

Every modern CPU repeats this cycle billions of times per second.

```text
           +---------+
           | Fetch   |
           +---------+
                |
                v
           +---------+
           | Decode  |
           +---------+
                |
                v
           +---------+
           | Execute |
           +---------+
                |
                +-------> Repeat
```

Let's examine each step.

## 1. Fetch

The CPU fetches the next instruction from memory.

Example:

```
Add two numbers
```

The CPU doesn't know what it means yet.

It simply retrieves the instruction.

---

## 2. Decode

The CPU interprets the instruction.

For example:

```
ADD R1, R2
```

Now the CPU understands that it must perform an addition.

---

## 3. Execute

The CPU performs the operation.

Then it immediately fetches the next instruction.

This loop continues until the program finishes.

---

# Why Can't the CPU Execute JavaScript?

Consider this line:

```javascript
const total = 5 + 10;
```

The CPU cannot read JavaScript syntax.

Instead, the execution path looks like this:

```text
JavaScript

↓

Node.js Runtime

↓

V8 Engine

↓

Machine Instructions

↓

CPU
```

Only the final machine instructions reach the CPU.

---

# CPU vs GPU

Many beginners confuse these.

| CPU | GPU |
|------|-----|
| General-purpose | Graphics-focused |
| Excellent at sequential tasks | Excellent at parallel tasks |
| Executes application logic | Renders images and performs massive parallel calculations |

When you click a button in React:

- The CPU processes the JavaScript.
- The GPU helps draw pixels to the screen.

---

# Internal Working

Suppose you run:

```bash
node app.js
```

A simplified sequence is:

```text
Terminal

↓

Operating System

↓

Node.js Process

↓

V8 Engine

↓

Machine Instructions

↓

CPU

↓

RAM Updated

↓

Console Output
```

The CPU never sees JavaScript directly.

---

# Engineering Insight

Many people believe:

> "A faster CPU means a faster application."

Not always.

Applications can also wait for:

- Disk access
- Network requests
- User input
- Database responses

During those waits, the CPU may be mostly idle.

Performance depends on much more than CPU speed.

---

# Hands-on Experiment

Open **Activity Monitor** (macOS) or **Task Manager** (Windows).

1. Open several browser tabs.
2. Start your Docusaurus project.
3. Observe CPU usage.
4. Stop the server.

Questions:

- Which process used the most CPU?
- Did CPU usage return to normal after stopping the server?
- Why?

---

# Common Mistakes

❌ Thinking the CPU understands JavaScript.

❌ Assuming CPUs "think."

❌ Believing higher CPU usage always means poor performance.

---

# Interview Questions

### Junior

- What is a CPU?

### Junior

- What is the Fetch–Decode–Execute cycle?

### Mid-Level

- Why can't a CPU execute JavaScript directly?

### Senior

- Explain the path from a JavaScript file to CPU execution.

---

# Engineering Notebook

## Rule #013

> The CPU executes instructions—not programming languages.

## Rule #014

> Every running application is ultimately a stream of machine instructions executed by the CPU.

---

# Summary

You learned:

- What the CPU is.
- The Fetch–Decode–Execute cycle.
- Why JavaScript requires translation.
- The difference between CPU and GPU.
- Why CPU speed isn't the only factor affecting performance.

---

# Revision Checklist

- [ ] I know what a CPU is.
- [ ] I can explain the Fetch–Decode–Execute cycle.
- [ ] I understand why JavaScript can't run directly on the CPU.
- [ ] I know the difference between CPU and GPU.
- [ ] I completed the experiment.

---

# Related Chapters

Previous

- What Is a Computer?

Next

- Memory (RAM)