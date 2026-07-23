---
title: Operating System Part 1
sidebar_position: 2
description: Learn what an Operating System really is, why it exists, and how it manages hardware, processes, memory, files, and applications.
---

# Operating System Part 1

> "The Operating System is the manager of the computer. The CPU, RAM, and SSD are resources. The OS decides who gets to use them."

---

# 🤔 Think Before Reading

Without searching, answer these questions:

1. If a CPU can execute instructions, why do we need an Operating System?
2. What would happen if there were no Operating System?
3. Does an application talk directly to the CPU?
4. Who decides how much memory Chrome gets?
5. Who decides which application uses the CPU next?

Write your answers before reading.

---

# Learning Objectives

By the end of this chapter, you will understand:

- What an Operating System is.
- Why it exists.
- How it manages hardware.
- How it manages processes.
- How it manages memory.
- How it manages files.
- Why applications use system calls.

---

# The Problem

Imagine a computer with:

- CPU
- RAM
- SSD

No Operating System.

You double-click Chrome.

Who should:

- Load Chrome into RAM?
- Create a process?
- Allocate memory?
- Give Chrome CPU time?
- Read files?
- Display the window?

Nobody.

The hardware cannot organize itself.

---

# First Principle

> The Operating System is software that manages all hardware resources and provides services to applications.

---

# Think of a Hotel

Imagine a hotel.

Resources:

- Rooms
- Electricity
- Water
- Staff

Guests:

- You
- Me
- Other visitors

The hotel manager decides:

- Who gets which room.
- Who checks in.
- Who checks out.
- How resources are shared.

The Operating System is like that hotel manager.

Applications are the guests.

Hardware is the hotel.

---

# The Operating System Manages

## CPU

Who gets CPU time?

Chrome?

Spotify?

VS Code?

Node.js?

The scheduler decides.

---

## Memory (RAM)

Who gets 500 MB?

Who gets 2 GB?

Who gets released when an app closes?

The OS manages this.

---

## Storage

When you press Ctrl + S:

```text
VS Code

↓

Operating System

↓

File System

↓

SSD
```

Applications don't write directly to the SSD.

---

## Devices

Mouse

Keyboard

Monitor

Printer

Camera

Microphone

Applications don't usually control these devices directly.

They ask the Operating System.

---

# Applications Cannot Touch Hardware Directly

Imagine if every application could directly control:

- RAM
- CPU
- SSD

Chaos.

One buggy application could overwrite another application's memory or delete important files.

The Operating System protects applications from each other.

---

# System Calls

Applications request services from the Operating System.

Examples:

```text
Application

↓

"I want to open a file."

↓

Operating System

↓

SSD
```

Or:

```text
Application

↓

"I need more memory."

↓

Operating System

↓

RAM
```

These requests are called **system calls**.

---

# Example

When Node.js executes:

```javascript
const fs = require("fs");
```

and later:

```javascript
fs.readFile(...)
```

Node.js doesn't read the SSD directly.

It asks the Operating System.

The OS reads the file and returns the data.

---

# Operating System Responsibilities

- Process management
- Memory management
- File management
- Device management
- CPU scheduling
- Security and permissions

---

# Engineering Rules

Rule #023

Applications do not own hardware.

Rule #024

The Operating System controls hardware access.

Rule #025

Applications request services through system calls.

---

# Summary

The Operating System is the manager that coordinates all hardware resources and provides safe services to applications.

Without it, every application would have to control hardware itself.