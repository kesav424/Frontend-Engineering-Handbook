---
title: Creating a Process
sidebar_position: 3
description: Follow the Operating System as it creates a new process, assigns a Process ID (PID), allocates memory, and prepares the program for execution.
---

# Creating a Process

> "Finding an executable is only the beginning. The Operating System must now create an entirely new execution environment."

---

# Learning Objectives

After completing this chapter, you will understand:

- Why a process must be created
- What happens inside the Operating System
- What a PID really is
- Why every process has its own memory
- What a Process Control Block (PCB) is
- What "Ready" means
- How a process eventually reaches the CPU

---

# Where We Are

So far our journey looks like this:

```
You

↓

Keyboard

↓

Operating System

↓

Terminal

↓

Shell

↓

Parse Command

↓

Find npm Executable
```

The shell has located:

```
/usr/local/bin/npm
```

Now the shell asks the Operating System:

> "Please run this executable."

---

# Can the Shell Run Programs?

No.

This surprises many beginners.

The shell **does not execute programs by itself**.

Instead, it asks the Operating System.

Think of the shell as a receptionist.

It can request work.

It cannot allocate RAM.

It cannot schedule CPU time.

It cannot create a process.

Only the Operating System has those privileges.

---

# The Request

Conceptually, the shell asks:

```
Operating System

Please execute

/usr/local/bin/npm
```

At this moment, the Operating System takes control.

---

# Why Can't the OS Just Run the File?

Imagine Chrome and VS Code sharing everything:

```
Chrome

↓

Same Memory

↓

VS Code
```

Now Chrome writes:

```
Memory Address 1000

Hello
```

VS Code writes:

```
Memory Address 1000

World
```

Who owns that memory?

Nobody knows.

The programs would overwrite each other.

The computer would become unstable.

---

# Isolation

Instead, the Operating System creates a private environment.

```
Chrome

↓

Own Memory

↓

Own Stack

↓

Own Heap
```

```
VS Code

↓

Own Memory

↓

Own Stack

↓

Own Heap
```

Every process receives its own address space.

This isolation is one of the reasons modern operating systems are reliable.

---

# Creating a Process

The Operating System performs several tasks.

```
Request

↓

Create Process

↓

Assign PID

↓

Create PCB

↓

Allocate Virtual Memory

↓

Prepare Execution

↓

Ready Queue
```

Each step is important.

---

# Step 1 — Assign a Process ID (PID)

Every process needs an identity.

```
Chrome

PID 2103
```

```
VS Code

PID 3410
```

```
npm

PID 5622
```

The PID uniquely identifies the process while it is running.

The Operating System uses the PID to:

- Schedule CPU time
- Track memory
- Deliver signals
- Terminate the process
- Monitor resources

Think of the PID as the process's passport.

---

# Step 2 — Create the Process Control Block (PCB)

The Operating System now creates a data structure called the:

```
Process Control Block
```

or

```
PCB
```

The PCB is **not** your program.

It is the Operating System's notebook about your program.

---

# What Does the PCB Contain?

Typical information includes:

```
PID

↓

Process State

↓

Program Counter

↓

CPU Registers

↓

Memory Information

↓

Open Files

↓

Scheduling Information
```

Whenever the scheduler performs a context switch, much of the information needed to resume execution comes from the PCB.

Without the PCB, the Operating System would lose track of the process.

---

# Step 3 — Allocate Virtual Memory

The Operating System creates a virtual address space.

Conceptually:

```
High Memory
────────────────────

Stack

↓

Free Space

↓

Heap

────────────────────

Code Section

────────────────────

Low Memory
```

At this stage, the process has its own logical memory layout.

The operating system maps these virtual addresses to physical RAM as needed.

---

# Why Virtual Memory?

Imagine two processes.

Both think:

```
Variable A

↓

Address 1000
```

How is that possible?

Because each process has its own **virtual** address space.

The Operating System translates virtual addresses into physical RAM behind the scenes.

This provides:

- Isolation
- Security
- Simpler programming

---

# Step 4 — Load the Executable

The Operating System now begins loading the executable into memory.

Conceptually:

```
SSD

↓

Executable File

↓

RAM

↓

Code Section
```

Remember:

Programs are stored permanently on the SSD.

The CPU executes instructions from RAM.

---

# Step 5 — Initial Thread

Every process begins with at least one thread.

```
Process

↓

Main Thread
```

For Node.js, this is the JavaScript main thread that will eventually begin executing your program.

Additional threads may be created later if needed.

---

# Step 6 — Ready State

The process is **not** immediately running.

Instead:

```
Create Process

↓

Ready Queue
```

The scheduler decides when it receives CPU time.

Remember Module 1:

The CPU cannot execute every process simultaneously.

The scheduler chooses which process runs next.

---

# Process States

A simplified view:

```
New

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Terminated
```

The process moves between these states throughout its lifetime.

---

# Under the Hood

Creating a process is much more than opening a file.

The Operating System must:

- Assign a PID
- Create a PCB
- Allocate virtual memory
- Prepare the first thread
- Load executable code
- Register the process with the scheduler

Only after all of this can the process begin executing.

---

# Engineering Thinking

Many developers think:

> "Running npm starts npm."

An engineer thinks:

> "The shell requested process creation. The Operating System created a new execution environment, assigned a PID, built a PCB, allocated virtual memory, loaded the executable, and placed the process into the scheduler's ready queue."

That is what **starting a program** really means.

---

# Mental Model

Imagine opening a hotel.

A guest arrives.

The hotel doesn't simply say:

"Welcome."

Instead it:

- Assigns a room number
- Creates a reservation record
- Gives the guest a key
- Connects utilities
- Updates the hotel management system

Only then can the guest enter the room.

A process is similar.

The executable is the guest.

The Operating System prepares everything before execution begins.

---

# Try It Yourself

Start a Node.js program:

```bash
node
```

Open another terminal.

macOS / Linux:

```bash
ps
```

or

```bash
ps -ef | grep node
```

Windows PowerShell:

```powershell
Get-Process node
```

Observe:

- Process ID
- Memory usage
- CPU time

You're looking at the Operating System's record of the running process.

---

# Common Misconceptions

❌ The shell creates processes.

✅ The Operating System creates processes.

---

❌ Every process shares the same memory.

✅ Every process has its own virtual address space.

---

❌ A PID is the program.

✅ A PID is simply an identifier for a running process.

---

❌ Programs begin executing immediately after being found.

✅ They must first be prepared by the Operating System and scheduled.

---

# Interview Questions

1. What is a process?
2. Why does every process have a PID?
3. What is a Process Control Block (PCB)?
4. What information does the PCB store?
5. Why does every process receive its own virtual memory?
6. What is the Ready state?
7. Why doesn't a process start executing immediately after creation?
8. Who creates a process: the shell or the Operating System?
9. Why is process isolation important?
10. Why is virtual memory useful?

---

# Summary

Finding the executable is only one part of starting a program.

The Operating System must create a complete execution environment. It assigns a unique Process ID (PID), builds a Process Control Block (PCB), allocates a virtual address space, loads the executable into memory, creates the initial thread, and places the process into the scheduler's ready queue.

Only after the scheduler selects the process does the CPU begin executing its instructions.

In the next chapter, we'll follow the process as the scheduler gives it CPU time, the executable is loaded into memory, and the first machine instructions begin running.