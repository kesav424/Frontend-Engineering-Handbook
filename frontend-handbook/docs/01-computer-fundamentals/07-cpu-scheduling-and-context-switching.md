---
title: CPU Scheduling & Context Switching
sidebar_position: 2
description: Learn how the Operating System shares the CPU between multiple applications using scheduling and context switching.
---

# CPU Scheduling & Context Switching

> "One CPU can appear to run hundreds of applications because the Operating System rapidly switches between threads."

---

# Learning Objectives

After this chapter, you should understand:

- Why CPU scheduling exists
- What a scheduler is
- What a time slice is
- What context switching is
- Why context switching has a cost
- Why computers appear to run many applications simultaneously

---

# The Problem

Imagine you have:

- Chrome
- VS Code
- Spotify
- Terminal
- Slack
- Node.js

running at the same time.

But your computer has only **one CPU core**.

Question:

**Who gets to use the CPU?**

The answer is:

The **Operating System Scheduler**.

---

# CPU Scheduling

CPU Scheduling is the process of deciding:

- Which thread runs next
- How long it runs
- When it should pause
- Which thread should run after that

Think of the scheduler as a traffic police officer.

```
Chrome Thread

↓

VS Code Thread

↓

Spotify Thread

↓

Node Thread

↓

Repeat...
```

The scheduler constantly decides who gets the CPU.

---

# Time Slice

A thread is not allowed to use the CPU forever.

Instead, it receives a small amount of CPU time called a **time slice** (also called a time quantum).

Example:

```
Chrome

↓

2 ms
```

Then:

```
VS Code

↓

2 ms
```

Then:

```
Spotify

↓

2 ms
```

Then:

```
Node.js

↓

2 ms
```

Then the cycle repeats.

The exact duration depends on the operating system and scheduling policy.

---

# Why Doesn't Everything Freeze?

Humans cannot perceive events happening every few milliseconds.

The CPU switches so quickly that we experience:

- Smooth scrolling
- Music playback
- Typing
- Video playback

as if everything is happening simultaneously.

In reality:

Only one thread per CPU core executes at any instant.

---

# Under the Hood

Imagine this sequence:

```
Chrome

↓

CPU executes

↓

Timer Interrupt

↓

Operating System

↓

Save Chrome Context

↓

Load VS Code Context

↓

CPU executes VS Code
```

This happens continuously while your computer is running.

---

# What Is Context?

Context is everything required to continue executing a thread exactly where it stopped.

Examples include:

- Program Counter
- CPU Registers
- Stack Pointer
- Processor Status Information

Without this information, the CPU would have to restart the program from the beginning every time.

---

# Context Switching

A context switch happens whenever the Operating System stops one thread and starts another.

Flow:

```
Thread A Running

↓

Save Context

↓

Scheduler Selects Thread B

↓

Load Thread B Context

↓

Resume Execution
```

The user never notices because this happens extremely quickly.

---

# Why Context Switching Has a Cost

Context switching is not free.

The Operating System must:

- Save the current thread's execution state
- Load another thread's execution state
- Update scheduling information
- Resume execution

All of this takes CPU time.

That means:

The CPU is temporarily managing threads instead of running your application.

---

# Too Few Context Switches

Imagine the scheduler never switched threads.

Chrome begins downloading a huge file.

The CPU waits for Chrome to finish before running anything else.

Result:

- Mouse freezes
- Music stops
- VS Code becomes unresponsive

The computer feels frozen.

---

# Too Many Context Switches

Now imagine the scheduler switches after every CPU instruction.

```
Chrome

↓

VS Code

↓

Spotify

↓

Node

↓

Chrome

↓

VS Code

...
```

The CPU spends most of its time:

- Saving context
- Loading context

instead of executing useful work.

Performance decreases.

The scheduler must find a balance between responsiveness and efficiency.

---

# Restaurant Analogy

Imagine a chef cooking for five tables.

If the chef never changes tables:

Everyone waits.

If the chef changes tables after every single ingredient:

The chef spends more time walking than cooking.

The best strategy is to work for a short period before switching.

The Operating System scheduler works the same way.

---

# Discussion Highlights

## Why don't applications freeze?

During our discussion we realized:

Every process receives a short amount of CPU time.

The switching happens so quickly that humans cannot perceive it.

This creates the illusion that all applications are running simultaneously.

---

## Where does the program continue after being paused?

We reasoned that:

The Operating System saves the execution state before switching.

Later, it restores that state and execution continues exactly where it stopped.

This saved execution state is called the **context**.

---

## Why not switch constantly?

Initially we thought:

Smaller time slices must always be better.

Then we discovered:

Every context switch has overhead.

Too many switches reduce overall performance.

The scheduler must balance:

- Fairness
- Responsiveness
- Efficiency

---

# Common Misconceptions

❌ The CPU executes multiple threads simultaneously on one core.

✅ A single CPU core executes one thread at a time.

---

❌ Context switching is free.

✅ Saving and restoring execution state costs CPU time.

---

❌ Applications freeze because they wait for each other.

✅ The Operating System rapidly switches between runnable threads.

---

❌ Every thread always gets exactly 2 ms.

✅ Time slices vary depending on the operating system and scheduler.

---

# Engineering Rules

## Rule #032

The Operating System scheduler decides which thread runs next.

---

## Rule #033

A time slice is a small amount of CPU time allocated to a thread.

---

## Rule #034

Context switching saves one thread's execution state and restores another's.

---

## Rule #035

Context switching has overhead.

Too many switches reduce performance.

Too few switches reduce responsiveness.

---

## Rule #036

On a single CPU core, only one thread executes at any instant.

---

# Interview Questions

1. What is CPU scheduling?
2. What is a scheduler?
3. What is a time slice?
4. What is context switching?
5. Why is context switching necessary?
6. Why does context switching have overhead?
7. Why doesn't your computer freeze when many applications are open?
8. What information is saved during a context switch?
9. Why can't a single CPU core execute two threads simultaneously?
10. Why doesn't the scheduler switch after every CPU instruction?

---

# Summary

A CPU core can execute only one thread at a time.

The Operating System scheduler shares CPU time among runnable threads using short time slices.

When switching between threads, the Operating System saves the current thread's context and restores the next thread's context. This is called a **context switch**.

Context switching allows many applications to appear to run simultaneously, but it also introduces overhead. The scheduler must balance responsiveness with performance.

Understanding CPU scheduling and context switching is essential because these concepts explain how modern operating systems support multitasking and form the foundation for understanding JavaScript's runtime, the Event Loop, and asynchronous programming.