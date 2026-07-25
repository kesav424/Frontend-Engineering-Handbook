---
title: Terminal and the Shell
sidebar_position: 3
description: Learn the difference between the Terminal and the Shell, and discover what really happens after you press Enter.
---

# Terminal and the Shell

> "The Terminal is the window you see. The Shell is the program that understands your commands."

---

# Learning Objectives

After completing this chapter, you will understand:

- What a Terminal is
- What a Shell is
- Why they are different
- How your command reaches the Shell
- What happens before npm starts
- Why shells exist

---

# Recap

In the previous chapter we stopped here:

```
You

↓

Press Enter

↓

Keyboard

↓

Keyboard Driver

↓

Operating System

↓

Terminal Receives Enter
```

Many developers think:

> "The terminal now runs npm."

Not yet.

The terminal itself **does not understand commands**.

That job belongs to another program.

---

# What Is a Terminal?

A terminal is simply a user interface.

It provides:

- A window
- Text input
- Text output
- Cursor
- Copy/Paste
- Colors

It does **not** know what:

```
npm
```

means.

It does **not** know what:

```
cd
```

means.

It does **not** know what:

```
git
```

means.

Think of the terminal as a chat window.

It displays text.

Someone else understands the language.

---

# Real Examples

## macOS

The Terminal application is:

```
Terminal.app
```

or

```
iTerm2
```

---

## Windows

Examples include:

- Windows Terminal
- Command Prompt
- PowerShell

---

## Linux

Examples include:

- GNOME Terminal
- Konsole
- Alacritty

Although they look different, they all have the same purpose:

Provide a way for you to communicate with a shell.

---

# What Is a Shell?

A shell is a program.

Its job is to:

- Read commands
- Understand commands
- Start programs
- Show output
- Manage the current working directory
- Handle environment variables

The shell is your interpreter between you and the Operating System.

---

# Think of It Like This

Imagine ordering food.

```
You

↓

Waiter

↓

Chef
```

The waiter is like the **terminal**.

The chef is like the **shell**.

The waiter takes your order.

The chef understands how to prepare it.

The waiter brings back the result.

---

# Terminal vs Shell

```
Terminal

↓

Displays Text

↓

Shell

↓

Understands Commands

↓

Operating System
```

The terminal displays.

The shell thinks.

---

# Common Shells

macOS:

```
zsh
```

Older macOS versions:

```
bash
```

Linux:

```
bash

zsh

fish
```

Windows:

```
PowerShell

cmd.exe
```

Different shells have different features, but their primary job is the same.

---

# What Happens After You Press Enter?

Suppose you typed:

```bash
npm start
```

When you press Enter:

```
Terminal

↓

Sends text

↓

Shell
```

Notice something important.

The shell receives plain text.

```
"npm start"
```

It has not executed anything yet.

---

# The Shell Reads Your Command

The shell now asks itself:

```
What is:

npm
?
```

Is it:

- A built-in command?
- An executable file?
- A script?
- An alias?
- A function?

It begins searching.

---

# Parsing the Command

The shell breaks your command into pieces.

```
npm start
```

becomes

```
Command

npm

Arguments

start
```

The shell now understands:

```
Executable:

npm

Argument:

start
```

This process is called **parsing**.

---

# Why Parsing Matters

Consider these commands:

```
git commit
```

```
node server.js
```

```
npm install react
```

Every command has:

```
Executable

Arguments

Options
```

The shell separates these parts before execution.

---

# The Current Working Directory

When you type:

```bash
pwd
```

the shell knows where you currently are.

Example:

```
/Users/kesav/projects/frontend-handbook
```

When you run:

```bash
npm start
```

the shell executes the command from the current directory.

That's why running the same command in a different folder can produce different results.

---

# Why Does npm Use package.json?

Suppose you're here:

```
frontend-handbook/

package.json
```

When the shell eventually starts npm, npm will look in the **current working directory** for a file named:

```
package.json
```

If it cannot find one:

```
npm ERR! package.json not found
```

This explains why running `npm start` outside your project folder fails.

---

# Complete Flow

```
You

↓

Terminal

↓

Shell

↓

Read Text

↓

Parse Command

↓

Identify Executable

↓

Identify Arguments

↓

Prepare for Execution
```

At this point:

- No npm process exists.
- No Node.js process exists.
- No JavaScript has executed.

The shell is still preparing.

---

# Under the Hood

The shell is constantly maintaining information such as:

- Current directory
- Environment variables
- Command history
- Aliases
- PATH variable

All of this influences how your command will be executed.

---

# Engineering Thinking

Users think:

> "The terminal executes commands."

Engineers know:

The terminal is just the interface.

The shell is the command interpreter.

Understanding this distinction makes many debugging problems much easier.

For example:

If a command works in one shell but not another, the problem may be with shell configuration rather than the program itself.

---

# Try It Yourself

Open your terminal and run:

```bash
pwd
```

What does it print?

Now run:

```bash
cd ..
pwd
```

Notice how the current working directory changes.

Now run:

```bash
npm start
```

outside a Node.js project.

Observe the error message.

Then return to your project and run it again.

Understanding the role of the current directory is an important debugging skill.

---

# Common Misconceptions

❌ The terminal executes commands.

✅ The shell interprets and executes commands.

---

❌ The shell automatically knows where every program is.

✅ The shell searches for executables using rules that we'll explore in the next chapter.

---

❌ `npm start` works from any folder.

✅ npm looks for `package.json` in the current working directory.

---

# Interview Questions

1. What is the difference between a terminal and a shell?
2. Is the terminal responsible for executing commands?
3. What is command parsing?
4. What is the current working directory?
5. Why does `npm start` fail outside a Node.js project?
6. Name three common shells.
7. What information does a shell maintain?

---

# Summary

The terminal is the graphical interface that allows users to type commands and view output.

The shell is the command interpreter that receives those commands, parses them, identifies the executable and its arguments, and prepares them for execution.

When you type:

```bash
npm start
```

the shell has not yet started Node.js. It has only interpreted the text and begun preparing to locate the `npm` executable.

In the next chapter, we'll follow the shell as it searches your system for the `npm` executable using the **PATH environment variable**, one of the most important concepts in command-line environments.