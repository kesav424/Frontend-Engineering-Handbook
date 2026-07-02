---
title: Handbook Style Guide
sidebar_position: 0
---

# Frontend Engineer Handbook Style Guide

This document defines the writing standards used throughout the handbook.

Every chapter should follow these rules to ensure consistency and maintainability.

---

# Philosophy

The goal of this handbook is not to teach tools.

The goal is to teach engineering.

Every chapter should answer:

- What problem existed?
- Why was this invented?
- How does it work?
- What trade-offs were made?
- How does it connect to other concepts?

Understanding these questions is more valuable than memorizing syntax.

---

# Writing Principles

## 1. Teach from First Principles

Always explain **why** before **how**.

Example:

❌ React uses the Virtual DOM.

✅ Browsers were slow at updating large DOM trees, so React introduced the Virtual DOM to reduce unnecessary DOM operations.

---

## 2. Connect Concepts

Every chapter should relate to previous knowledge.

Knowledge should form a graph, not isolated facts.

---

## 3. Prefer Mental Models

Avoid isolated definitions.

Use analogies and diagrams where appropriate.

---

## 4. Encourage Experimentation

Every chapter should include at least one hands-on lab.

Learning happens through experimentation, not memorization.

---

# Chapter Template

Every chapter follows this structure.

```text
Learning Objectives

Problem

History

First Principle

Mental Model

Deep Explanation

Internal Working

Diagram

Experiment (Lab)

Common Mistakes

Interview Questions

Engineering Notebook

Summary

Revision Checklist

Related Chapters
```

---

# Difficulty Levels

⭐☆☆☆☆ Beginner

⭐⭐☆☆☆ Easy

⭐⭐⭐☆☆ Intermediate

⭐⭐⭐⭐☆ Advanced

⭐⭐⭐⭐⭐ Expert

---

# Reading Time

Every chapter includes an estimated reading time.

Example:

Reading Time: 15 minutes

---

# Interview Importance

⭐☆☆☆☆ Rare

⭐⭐☆☆☆ Sometimes Asked

⭐⭐⭐☆☆ Common

⭐⭐⭐⭐☆ Frequently Asked

⭐⭐⭐⭐⭐ Core Interview Topic

---

# Code Rules

All code should:

- Compile successfully.
- Be minimal.
- Demonstrate one concept at a time.
- Avoid unnecessary complexity.

---

# Diagram Rules

Prefer simple ASCII diagrams over images.

Example:

Application
    │
    ▼
Node.js Runtime
    │
    ▼
V8 Engine
    │
    ▼
Machine Code

---

# Naming Rules

Use lowercase filenames.

Examples:

runtime.md

package-json.md

event-loop.md

Avoid:

Runtime.md

PackageJSON.md

Runtime_Final_v2.md

---

# Admonitions

Use Docusaurus admonitions.

Example:

:::tip

Useful advice.

:::

:::warning

Common mistake.

:::

:::info

Additional information.

:::

---

# Labs

Every chapter should contain at least one practical lab.

Labs should require the reader to predict the outcome before executing the code.

---

# Engineering Notebook

Every chapter ends with one engineering principle.

Example:

> Languages don't execute themselves.

These principles are intended to be memorable and timeless.

---

# Revision Checklist

Every chapter concludes with a checklist.

Example:

- [ ] I can explain this concept without notes.
- [ ] I completed the lab.
- [ ] I understand why this concept exists.
- [ ] I know where it is used.

---

# Cross References

Every chapter should include links to:

Prerequisite chapters.

Related chapters.

Next recommended chapter.

---

# Versioning

The handbook follows Semantic Versioning.

Major versions represent significant expansions.

Minor versions introduce new modules.

Patch versions fix errors and improve clarity.

---

# Tone

The handbook should be:

- Friendly.
- Curious.
- Technically accurate.
- Precise.
- Encouraging.

Avoid unnecessary jargon.

Explain difficult concepts through examples.

---

# Final Goal

This handbook should remain valuable even after specific frameworks evolve.

Frameworks may change.

Engineering principles endure.