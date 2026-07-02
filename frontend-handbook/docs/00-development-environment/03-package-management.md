---
title: 03 - Package Management
sidebar_position: 4
description: Learn how modern JavaScript projects manage dependencies, why package.json exists, what package-lock.json does, and how npm installs packages.
---

# Package Management

> *"Software is rarely built from scratch. Modern applications are assembled from thousands of reusable libraries. Package management makes this possible."*

---

# Reading Time

**30–40 minutes**

# Difficulty

⭐⭐☆☆☆

# Interview Importance

⭐⭐⭐⭐⭐

---

# Learning Objectives

By the end of this chapter you will be able to explain:

- What a package is
- Why package managers exist
- What npm is
- What package.json does
- What package-lock.json does
- Why node_modules exists
- What happens during npm install
- Semantic Versioning
- dependencies vs devDependencies
- Why every Node.js project looks similar

---

# The Problem

Imagine building a web application.

You need:

- React
- TypeScript
- ESLint
- Tailwind CSS
- Axios
- Zod

Should you write all of those yourself?

Of course not.

Instead, you install them.

But another problem appears.

How does your teammate install exactly the same packages?

How does npm know what to download?

How does everyone stay on the same versions?

Package management solves these problems.

---

# First Principle

> Every software project must describe itself before tools can automate it.

Without a description of your project, automation becomes impossible.

---

# Mental Model

Think of building a house.

```text
Blueprint
      │
      ▼
Shopping List
      │
      ▼
Construction Materials
      │
      ▼
House
```

A Node.js project follows a similar pattern.

```text
package.json
      │
      ▼
package-lock.json
      │
      ▼
node_modules
      │
      ▼
Running Application
```

Remember this model.

We'll refer back to it throughout the handbook.

---

# What Is a Package?

A package is simply reusable software.

Examples include:

- React
- Next.js
- Express
- Axios
- Lodash

Instead of copying code between projects, developers publish packages that others can install.

---

# What Is npm?

npm stands for **Node Package Manager**.

It has two major responsibilities:

1. Download packages.
2. Run project scripts.

Many beginners think npm executes JavaScript.

It doesn't.

Node.js executes JavaScript.

npm helps manage packages and scripts.

---

# package.json

This is the blueprint of your project.

Example:

```json
{
  "name": "frontend-handbook",
  "version": "1.0.0",
  "scripts": {
    "start": "docusaurus start"
  },
  "dependencies": {
    "react": "^19.0.0"
  }
}
```

---

## What information does package.json contain?

- Project name
- Version
- Scripts
- Dependencies
- Development dependencies
- Project metadata

Think of it as the identity card of your project.

---

# Why Does package.json Exist?

Imagine cloning a repository.

Without package.json you would have no idea:

- which packages are needed
- which scripts exist
- which versions were intended

The project would be impossible to reproduce.

---

# package-lock.json

This file records the exact dependency tree that was installed.

Example:

```text
React

↓

Depends on Package A

↓

Depends on Package B

↓

Depends on Package C
```

The lock file remembers the entire tree.

---

## Why was it invented?

Imagine yesterday React downloaded version:

```
19.1.2
```

Today a new compatible version is released.

```
19.1.3
```

Without a lock file:

Developer A installs one version.

Developer B installs another.

CI installs another.

Production installs another.

Now everyone is debugging different software.

The lock file prevents this.

---

# node_modules

This folder contains the actual downloaded packages.

Think of it as your project's local library.

Important:

You didn't write this code.

npm downloaded it.

---

# package.json vs package-lock.json

| package.json | package-lock.json |
|--------------|-------------------|
| Blueprint | Exact snapshot |
| Human edited | Automatically generated |
| Describes intent | Describes installed result |
| Flexible versions | Exact versions |

---

# dependencies

These packages are required for the application to run.

Examples:

- React
- Axios
- Express

Without them your application cannot function.

---

# devDependencies

These packages help developers.

Examples:

- TypeScript
- ESLint
- Prettier
- Jest
- Docusaurus CLI

They improve development but are not part of your application's runtime.

---

# Semantic Versioning

Most packages use:

```
MAJOR.MINOR.PATCH
```

Example

```
2.5.7
```

- Major = breaking changes
- Minor = new features
- Patch = bug fixes

---

## Caret (^)

```
^2.5.7
```

Allows:

```
2.6.0

2.7.3

2.9.9
```

Not allowed:

```
3.0.0
```

---

## Tilde (~)

```
~2.5.7
```

Allows

```
2.5.8

2.5.9
```

Not

```
2.6.0
```

---

## Exact Version

```
2.5.7
```

Only

```
2.5.7
```

---

# What Happens During npm install?

Many developers type this command every day.

Few understand it.

Here's the process.

```text
npm install

↓

Read package.json

↓

Resolve dependency versions

↓

Download packages

↓

Download sub-dependencies

↓

Verify integrity

↓

Create node_modules

↓

Generate package-lock.json

↓

Project ready
```

---

# Why Can node_modules Be Deleted?

Because it is generated.

Your real project consists of:

```text
Source Code

+

package.json

+

package-lock.json
```

Everything else can be recreated.

---

# Internal Working

Imagine installing React.

You request:

```
React
```

npm discovers React itself depends on many other packages.

Those packages depend on more packages.

Eventually npm builds a dependency tree.

Example

```text
React

├── scheduler

├── loose-envify

└── ...
```

node_modules contains the complete tree.

---

# Common Misconceptions

## ❌ node_modules is my project

No.

It is downloaded software.

---

## ❌ package-lock.json is unnecessary

No.

It ensures reproducible installations.

---

## ❌ npm executes JavaScript

No.

Node.js executes JavaScript.

npm manages packages and scripts.

---

## ❌ package.json installs packages

No.

It only describes them.

npm performs the installation.

---

# Lab

Create a new folder.

Run:

```bash
npm init -y
```

Observe:

```
package.json
```

Now run:

```bash
npm install lodash
```

Observe:

- package.json changes
- package-lock.json appears
- node_modules appears

Delete node_modules.

Run:

```bash
npm install
```

Observe what gets recreated.

Answer:

Why was npm able to rebuild everything?

---

# Interview Questions

### Junior

What is package.json?

---

### Junior

What is package-lock.json?

---

### Junior

What is node_modules?

---

### Mid-Level

Explain what happens when you run npm install.

---

### Mid-Level

Why shouldn't node_modules be committed to Git?

---

### Senior

Why is package-lock.json important in CI/CD pipelines?

---

# Engineering Notebook

## Rule #005

> package.json describes your project.

---

## Rule #006

> package-lock.json guarantees reproducibility.

---

## Rule #007

> node_modules is generated, not authored.

---

# Summary

In this chapter you learned:

- What packages are
- Why npm exists
- package.json
- package-lock.json
- node_modules
- dependencies
- devDependencies
- Semantic Versioning
- npm install
- Dependency trees

---

# Revision Checklist

- [ ] I can explain package.json.
- [ ] I can explain package-lock.json.
- [ ] I can explain node_modules.
- [ ] I know what npm install does.
- [ ] I understand dependency resolution.
- [ ] I know the difference between dependencies and devDependencies.
- [ ] I understand semantic versioning (^, ~, exact).

---

# Related Chapters

Previous

- Project Structure

Next

- Node.js Runtime

---

> **Key Takeaway**

**Modern JavaScript development isn't about writing every line yourself—it's about managing reusable software reliably and reproducibly.**