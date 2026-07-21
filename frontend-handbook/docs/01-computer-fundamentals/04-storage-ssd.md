---
title: Storage (SSD & HDD)
sidebar_position: 4
description: Understand how computers store data permanently, how SSDs and HDDs differ, and what really happens when you save or delete a file.
---

# Storage (SSD & HDD)

> *"RAM remembers while the computer is running. Storage remembers after the computer is turned off."*

---

# 🤔 Think Before Reading

Without searching, answer these questions:

1. Where is your JavaScript file stored before you run it?
2. What happens when you press **Ctrl + S** in VS Code?
3. If you delete a file, is it immediately erased from the SSD?
4. Why is an SSD faster than an HDD?

Write your answers first.

---

# Reading Time

30–40 minutes

# Difficulty

⭐⭐☆☆☆

# Interview Importance

⭐⭐⭐⭐☆

---

# Learning Objectives

By the end of this chapter, you will understand:

- What storage is.
- Why storage exists.
- HDD vs SSD.
- How files are saved.
- What happens when files are deleted.
- Why SSDs are faster.
- Why SSDs eventually wear out.

---

# The Real Problem

Imagine writing:

```javascript
console.log("Hello");
```

You press:

```text
Ctrl + S
```

Question:

Where did your code go?

Did it go into RAM?

The CPU?

The operating system?

The SSD?

Let's find out.

---

# First Principle

> **Storage keeps information even after power is turned off.**

Unlike RAM, storage is **non-volatile**.

---

# What Is Storage?

Storage is permanent memory.

It keeps:

- Programs
- Photos
- Videos
- Documents
- Games
- Source Code

Even after shutdown.

Examples:

- SSD
- HDD

---

# Mental Model

Think of your office.

```text
Bookshelf (Storage)

↓

Desk (RAM)

↓

You (CPU)
```

The bookshelf stores everything.

The desk only contains what you're currently working on.

---

# HDD (Hard Disk Drive)

An HDD stores data on spinning magnetic disks.

Imagine a record player.

```text
Disk spins

↓

Read/Write head moves

↓

Reads data
```

Because physical parts move, HDDs are slower.

---

# SSD (Solid State Drive)

An SSD has **no moving parts**.

Instead, it stores data in electronic memory cells.

Advantages:

- Faster
- Silent
- Lower power consumption
- More resistant to physical shock

---

# HDD vs SSD

| HDD | SSD |
|------|-----|
| Moving parts | No moving parts |
| Slower | Much faster |
| Cheaper per GB | More expensive per GB |
| Noisy | Silent |
| More fragile | More durable |

---

# Where Is My JavaScript File?

Suppose you save:

```javascript
console.log("Hello");
```

The file is stored here:

```text
SSD

↓

project

↓

src

↓

app.js
```

Nothing executes yet.

It's just data.

---

# What Happens When You Press Save?

A simplified flow:

```text
VS Code

↓

Operating System

↓

File System

↓

SSD

↓

Data Written
```

Notice:

The CPU isn't "running" your code.

It's simply helping write data to storage.

---

# What Happens When You Run the Program?

```text
SSD

↓

Operating System

↓

RAM

↓

Node.js

↓

CPU

↓

Output
```

This should look familiar from the previous chapter.

---

# What Happens When You Delete a File?

Many people imagine:

```text
Delete

↓

Gone Forever
```

That's usually **not** what happens.

Instead:

```text
Delete

↓

Operating System marks space as available

↓

Actual data may remain until overwritten
```

This is why deleted files can sometimes be recovered using recovery software.

(Modern SSDs also use mechanisms like TRIM, which we'll discuss later.)

---

# Why Are SSDs Faster?

HDD:

```text
Move read head

↓

Wait for disk to rotate

↓

Read data
```

SSD:

```text
Access memory cell electronically

↓

Read data
```

No mechanical movement means much lower access time.

---

# Do SSDs Last Forever?

No.

Each memory cell can only be written a limited number of times.

Modern SSDs use techniques like wear leveling to spread writes across the drive, greatly extending their lifespan.

For normal everyday use, SSDs typically last many years.

---

# Internal Working

Saving a file:

```text
VS Code

↓

Operating System

↓

File System

↓

SSD Controller

↓

Flash Memory Cells

↓

Data Stored
```

Running a file:

```text
SSD

↓

RAM

↓

CPU
```

Saving and running are different operations.

---

# Engineering Insight

Many beginners think:

> "Deleting a file removes it immediately."

In reality, the operating system often just removes the reference to the file and marks the space as free.

The underlying data may still exist until new data overwrites it.

---

# Hands-on Experiment

Create a text file.

Write:

```
Hello Storage
```

Save it.

Questions:

- Where is the file now?
- Is it in RAM?
- Is it on the SSD?
- What happens if you restart the computer?

---

# Common Mistakes

❌ RAM and storage are the same thing.

❌ SSDs never wear out.

❌ Deleting a file instantly destroys the data.

❌ Saving a file means the CPU is executing it.

---

# Interview Questions

### Junior

- What is the difference between RAM and SSD?

### Junior

- Why are SSDs faster than HDDs?

### Mid-Level

- Explain what happens when you save a file.

### Mid-Level

- Explain what happens when you delete a file.

### Senior

- Why can deleted files sometimes be recovered?

---

# Engineering Notebook

## Rule #020

> Storage keeps files. RAM runs files.

## Rule #021

> Saving data and executing data are two different operations.

## Rule #022

> Deleting a file usually removes its reference before removing its actual contents.

---

# Summary

You learned:

- What storage is.
- SSD vs HDD.
- Saving files.
- Running files.
- File deletion.
- Why SSDs are faster.
- Why SSDs wear out.

---

# Revision Checklist

- [ ] I understand storage.
- [ ] I know the difference between SSD and HDD.
- [ ] I understand how files are saved.
- [ ] I understand why SSDs are faster.
- [ ] I know why deleted files may be recoverable.

---

# Related Chapters

Previous

- Memory (RAM)

Next

- Operating System

---

> ## Key Takeaway

> **Storage preserves information. RAM prepares it. The CPU executes it.**