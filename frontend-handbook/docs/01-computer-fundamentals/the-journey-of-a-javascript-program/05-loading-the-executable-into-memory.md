---
title: Loading the Executable into Memory
sidebar_position: 3
description: Learn how the Operating System loads an executable from storage into memory before the CPU can execute the first instruction.
---

# Loading the Executable into Memory

> *"A program stored on an SSD is just data. It becomes a running program only after the Operating System loads it into memory."*

---

# Learning Objectives

After completing this chapter, you will understand:

- Why programs cannot execute directly from an SSD
- What an executable file is
- What the Operating System Loader does
- How an executable is loaded into RAM
- What the Code, Data, BSS, Heap, and Stack sections are
- Why virtual memory exists
- What happens before the CPU executes the first instruction

---

# Prerequisites

Before reading this chapter, you should understand:

- CPU Fundamentals
- RAM vs SSD
- Operating System Basics
- Processes
- Process Creation
- Virtual Memory (Basic Idea)

---

# Where We Are

In the previous chapter, the shell asked the Operating System to start the `npm` executable.

The Operating System has already:

- Located the executable
- Created a Process
- Assigned a PID
- Created a PCB
- Allocated a Virtual Address Space

Current position:

```text
You
 │
 ▼
Terminal
 │
 ▼
Shell
 │
 ▼
Find npm
 │
 ▼
Operating System
 │
 ▼
Create Process
 │
 ▼
Ready...
```

One important thing is still missing.

The CPU cannot execute a program that still lives on the SSD.

---

# Why Can't the CPU Execute From an SSD?

Imagine this:

```
SSD

node

npm

chrome

photos
```

These files are only **stored**.

The CPU does not execute files directly from storage.

Instead, the CPU fetches instructions from **RAM**.

```
CPU

↓

RAM

✓ Executes
```

```
CPU

↓

SSD

✗ Cannot execute directly
```

---

# Why?

Because SSDs are optimized for **permanent storage**, not execution.

RAM is optimized for:

- Extremely fast reads
- Extremely fast writes
- Low latency
- Constant communication with the CPU

Think of it like this:

```
Library

↓

Book Shelf

↓

Reader
```

Would you read an entire book while it remains locked inside the shelf?

No.

You first take the book to your desk.

RAM is the desk.

The SSD is the bookshelf.

---

# The Operating System Loader

The Operating System contains a special component called the **Loader**.

Its job is simple:

```
Executable File

↓

Load Into Memory

↓

Prepare For Execution
```

Without the Loader:

The executable would remain a file forever.

---

# Big Picture

```text
               SSD
┌────────────────────────────────┐
│                                │
│      npm Executable            │
│                                │
└──────────────┬─────────────────┘
               │
               │ Loader
               ▼
       Operating System
               │
               ▼
              RAM
┌────────────────────────────────┐
│ Code                           │
│ Data                           │
│ BSS                            │
│ Heap                           │
│ Stack                          │
└────────────────────────────────┘
               │
               ▼
              CPU
```

---

# What Is an Executable?

An executable is **not** source code.

Your JavaScript file:

```javascript
console.log("Hello");
```

is **not executable**.

Instead:

```
JavaScript

↓

Node Runtime

↓

Machine Instructions
```

However,

Programs like:

```
node

npm

git

chrome
```

already contain machine instructions.

Those machine instructions are stored inside executable files.

Examples:

Windows

```
node.exe
```

Linux

```
/usr/bin/node
```

macOS

```
/opt/homebrew/bin/node
```

---

# Inside an Executable

An executable is carefully organized.

It is **not** one giant block of bytes.

Imagine it like this:

```text
Executable File

┌─────────────────────────────┐
│ Header                      │
├─────────────────────────────┤
│ Machine Code                │
├─────────────────────────────┤
│ Initialized Data            │
├─────────────────────────────┤
│ Metadata                    │
└─────────────────────────────┘
```

The Operating System understands this format.

It knows where each section belongs in memory.

---

# Loading Into RAM

When the Loader starts working:

```
SSD

↓

Read Executable

↓

Copy Into RAM
```

The executable is **not** copied as one huge block.

Instead, each section is placed in the correct location.

---

# Memory Layout

A simplified process memory layout looks like this.

```text
High Memory Address
────────────────────────────────────

+-------------------------------+
|            Stack              |
|                               |
+-------------------------------+

|                               |
|        Free Space             |
|                               |

+-------------------------------+
|            Heap               |
|                               |
+-------------------------------+

|            BSS                |
+-------------------------------+

|      Initialized Data         |
+-------------------------------+

|         Code Segment          |
+-------------------------------+

Low Memory Address
```

Every section has a purpose.

---

# Code Segment

The Code Segment contains:

```
Machine Instructions
```

Example:

```
ADD

MOV

CALL

RET
```

This section is normally **read-only**.

Why?

Imagine malware changing machine instructions while the program is running.

Making code read-only helps prevent accidental or malicious modification.

---

# Initialized Data Segment

Suppose a C program contains:

```c
int count = 10;
```

The value `10` already exists before the program starts.

It belongs in the **Initialized Data Segment**.

---

# BSS Segment

Suppose:

```c
int total;
```

No value has been assigned.

The Operating System initializes it to zero.

These variables belong in the **BSS (Block Started by Symbol)** section.

---

# Heap

The Heap is used for **dynamic memory allocation**.

Whenever a program requests memory while running:

```
Need More Memory

↓

Heap Grows
```

Languages such as JavaScript use the heap extensively for:

- Objects
- Arrays
- Functions
- Closures

We'll study the heap in much greater detail in Module 2.

---

# Stack

The Stack stores temporary execution information.

Examples:

- Function calls
- Local variables
- Return addresses

Every time a function is called:

```
Push Frame
```

When the function returns:

```
Pop Frame
```

The stack constantly grows and shrinks during execution.

---

# Memory Diagram

```text
                RAM

High Address
┌───────────────────────────────┐
│ Stack                         │
│                               │
├───────────────────────────────┤
│                               │
│ Free Space                    │
│                               │
├───────────────────────────────┤
│ Heap                          │
│ Objects                       │
│ Arrays                        │
├───────────────────────────────┤
│ BSS                           │
├───────────────────────────────┤
│ Initialized Data              │
├───────────────────────────────┤
│ Machine Code                  │
└───────────────────────────────┘
Low Address
```

---

# Does the Entire Program Load Immediately?

Not always.

Modern Operating Systems often use:

- Demand Paging
- Lazy Loading
- Memory Mapping

Instead of loading every byte immediately, the OS loads pages into RAM when they are actually needed.

This reduces startup time and memory usage.

We'll revisit these topics in later modules.

---

# Under the Hood

The loader performs many tasks behind the scenes:

- Reads the executable file.
- Validates its format.
- Creates memory mappings.
- Loads the code segment.
- Loads initialized data.
- Creates the stack.
- Creates the heap.
- Sets up the initial thread.
- Chooses the program's entry point.

Only after all of this is the process ready to execute.

---

# Engineering Thinking

Many developers think:

> "The Operating System opens the executable."

An engineer thinks:

> "The Operating System's loader parses the executable format, maps its sections into the process's virtual address space, initializes memory regions, and prepares the CPU's initial execution context."

That's a much more accurate picture of what happens.

---

# Try It Yourself

### macOS / Linux

Find the Node executable:

```bash
which node
```

Example:

```
/opt/homebrew/bin/node
```

Check what kind of file it is:

```bash
file $(which node)
```

You'll see information about the executable format for your operating system.

---

# Common Misconceptions

❌ Programs execute directly from the SSD.

✅ Programs are loaded into RAM before execution.

---

❌ RAM stores only variables.

✅ RAM stores code, data, heap, stack, and more.

---

❌ The CPU understands JavaScript.

✅ The CPU executes machine instructions.

---

❌ Every byte of an executable is loaded immediately.

✅ Modern operating systems often load memory on demand.

---

# Performance Notes

Loading large executables takes time.

Operating systems optimize startup by:

- Using page caching
- Demand paging
- Shared libraries
- Memory mapping

These techniques reduce disk access and improve application startup performance.

---

# Security Notes

Modern operating systems protect memory using techniques such as:

- Read-only code sections
- Address Space Layout Randomization (ASLR)
- Data Execution Prevention (DEP)

These help prevent many classes of software attacks.

You'll encounter these concepts when studying systems programming and browser security.

---

# Interview Corner

### Beginner

1. Why can't the CPU execute directly from an SSD?
2. What is an executable?
3. What does the Operating System Loader do?

### Intermediate

4. What is the difference between the Stack and the Heap?
5. Why is the Code Segment usually read-only?
6. What is the purpose of the BSS section?

### Advanced

7. What is demand paging?
8. Why do operating systems use virtual memory?
9. What happens between creating a process and executing its first instruction?

---

# Summary

Creating a process is only part of starting a program.

The Operating System must also load the executable into memory.

The Loader reads the executable from the SSD, validates its format, maps different sections into the process's virtual address space, creates the stack and heap, initializes memory, and prepares the first thread.

Only after these steps is the process ready for the CPU to execute its first instruction.

---

# What Actually Happened So Far

```text
✓ You pressed Enter
✓ Keyboard generated a scan code
✓ Operating System received the keyboard event
✓ Terminal passed the command to the Shell
✓ Shell parsed "npm start"
✓ Shell searched the PATH variable
✓ Shell found the npm executable
✓ Operating System created a process
✓ PID assigned
✓ PCB created
✓ Virtual memory allocated
✓ Loader copied the executable into RAM

NEXT →

The CPU will finally execute the very first machine instruction of the process.
```

---

# Next Chapter

➡ **CPU Starts Executing**

In the next chapter you'll watch the CPU execute the very first instruction of the `npm` process.

We'll explore:

- The Program Counter (Instruction Pointer)
- CPU Registers
- The Fetch–Decode–Execute Cycle
- How machine instructions are read from RAM
- What actually happens every clock cycle

By the end of the next chapter, you'll understand how the CPU begins transforming a file in memory into a running program.