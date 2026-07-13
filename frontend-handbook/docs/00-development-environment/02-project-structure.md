---
title: Project Structure
sidebar_label: 02 - Project Structure
sidebar_position: 3
description: Understand how a modern JavaScript project is organized and why each file and folder exists.
---

# Project Structure

> *"A project is more than source code. It is a collection of instructions, configuration, documentation, and dependencies that work together to build software."*

---

## Reading Time

**20–25 minutes**

## Difficulty

⭐☆☆☆☆ Beginner

## Interview Importance

⭐⭐⭐⭐⭐ Essential

---

# Learning Objectives

By the end of this chapter, you should be able to:

- Explain why projects have folders.
- Understand the purpose of common project files.
- Navigate a JavaScript project confidently.
- Identify which files should and should not be modified.
- Understand why configuration files exist.

---

# The Problem

Imagine opening a project that contains every file in one folder.

```text
index.js
App.js
Button.js
Header.js
Footer.js
styles.css
README.md
package.json
logo.png
config.js
server.js
...
```

Finding anything becomes difficult.

As projects grow, organization becomes essential.

---

# First Principle

> **Software becomes easier to maintain when related things are grouped together.**

Folders are not just for organization—they help developers understand a project quickly.

---

# Mental Model

Think of a project like a house.

```text
House

├── Kitchen
├── Bedroom
├── Bathroom
└── Garage
```

Everything has a place.

A software project follows the same idea.

```text
Project

├── Source Code
├── Images
├── Configuration
├── Documentation
└── Dependencies
```

---

# Your Docusaurus Project

Your project might look like this:

```text
frontend-handbook/

├── docs/
├── src/
├── static/
├── blog/
├── node_modules/
├── .docusaurus/
├── package.json
├── package-lock.json
├── docusaurus.config.ts
├── sidebars.ts
├── tsconfig.json
├── README.md
└── .gitignore
```

Every item exists for a reason.

Let's understand each one.

---

# docs/

```text
docs/
```

This is where your handbook lives.

Every Markdown (`.md`) or MDX (`.mdx`) file becomes a documentation page.

Example:

```text
docs/

00-development-environment/

README.md

01-debugging-mindset.md

02-project-structure.md
```

Think of this folder as the **content** of your website.

---

# src/

```text
src/
```

Contains the React application that powers Docusaurus.

Example:

```text
src/

components/

HomepageFeatures/

pages/

css/
```

Think of this as the **application code**.

---

# static/

Contains files copied directly into the final website.

Examples:

```text
logo.png

favicon.ico

images/

robots.txt
```

These files are served exactly as they are.

No compilation.

---

# blog/

Optional.

Contains blog posts.

If you're only writing documentation, you may rarely use this folder.

---

# node_modules/

```text
node_modules/
```

This folder contains every package your project depends on.

Important:

You did **not** write this code.

npm downloaded it.

Never edit files inside `node_modules`.

If the folder is deleted, run:

```bash
npm install
```

and npm recreates it.

We'll study this folder in depth later.

---

# .docusaurus/

Generated automatically.

Contains temporary files produced by Docusaurus.

You should not edit this folder manually.

If deleted, Docusaurus will regenerate it.

---

# package.json

The blueprint of your project.

It describes:

- Project name
- Version
- Scripts
- Dependencies
- Metadata

We'll dedicate an entire chapter to this file.

---

# package-lock.json

Records the exact dependency versions installed.

This ensures everyone working on the project installs the same versions.

We'll study it in detail later.

---

# docusaurus.config.ts

The main configuration file.

It controls:

- Site title
- Navigation
- Theme
- Plugins
- Deployment settings

Think of it as the settings page for your documentation site.

---

# sidebars.ts

Defines the navigation menu shown in the documentation sidebar.

Without it, users would struggle to navigate your handbook.

---

# tsconfig.json

Configuration for TypeScript.

It tells TypeScript how to understand your project.

Examples:

- Strict mode
- Module resolution
- Path aliases
- Compiler options

Earlier, VS Code showed errors because it temporarily couldn't resolve:

```text
@docusaurus/tsconfig
```

That was an IDE issue, not a Docusaurus issue.

---

# README.md

The homepage of your GitHub repository.

This file answers:

- What is this project?
- Why does it exist?
- How do I run it?

A good README is often the first thing users see.

---

# .gitignore

Lists files Git should ignore.

Example:

```text
node_modules/
build/
.docusaurus/
```

Why?

Because these files can be regenerated automatically.

There is no need to store them in Git.

---

# Internal Working

When you run:

```bash
npm start
```

This happens:

```text
You

↓

npm

↓

Reads package.json

↓

Runs the "start" script

↓

Executes Docusaurus

↓

Builds the React application

↓

Starts a local development server

↓

Browser displays your website
```

Understanding this flow helps you debug startup issues more effectively.

---

# Common Mistakes

## ❌ Editing node_modules

Changes will be lost after reinstalling dependencies.

---

## ❌ Committing node_modules to Git

It makes the repository unnecessarily large.

---

## ❌ Deleting package-lock.json without understanding why

This can lead to inconsistent dependency versions across machines.

---

## ❌ Treating configuration files as "magic"

Every configuration file has a purpose.

Read them.

---

# Lab

## Objective

Explore your own project.

Answer the following questions:

1. Which folder contains your documentation?
2. Which folder contains React components?
3. Where is the project configuration?
4. Where are dependencies installed?
5. Which files are generated automatically?
6. Which files would you commit to Git?

Write your answers before checking the documentation.

---

# Interview Questions

### Junior

What is `package.json`?

---

### Junior

What is `node_modules`?

---

### Junior

What is the purpose of `.gitignore`?

---

### Mid-Level

Why shouldn't `node_modules` be committed to Git?

---

### Senior

If a new developer clones the repository, what files are required to recreate the development environment?

---

# Engineering Notebook

## Rule #003

> Every file exists to solve a specific problem.

Don't ignore configuration files.

Understand them.

---

## Rule #004

> A well-organized project reduces cognitive load.

Structure is a feature.

---

# Summary

In this chapter, you learned:

- Why projects are organized into folders.
- The purpose of common project files.
- How Docusaurus is structured.
- Which files should and should not be modified.
- How `npm start` launches your project.

---

# Revision Checklist

- [ ] I can explain the purpose of `docs/`.
- [ ] I can explain the purpose of `src/`.
- [ ] I know what `static/` is used for.
- [ ] I know why `node_modules` exists.
- [ ] I know what `.gitignore` does.
- [ ] I understand the high-level startup flow of `npm start`.

---

# Related Chapters

Previous:

- Debugging Mindset

Next:

- Package Management (`package.json`, `package-lock.json`, `node_modules`)

---

> **Key Takeaway**

**A project structure is not arbitrary. Every folder and file exists because it solves a specific problem in software development.**