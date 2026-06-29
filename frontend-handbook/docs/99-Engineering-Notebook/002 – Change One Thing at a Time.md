---
title: 002 - Change One Thing at a Time
sidebar_position: 1
---
 📒 Engineering Notebook Entry
# Change One Thing at a Time


When debugging, avoid making multiple changes simultaneously. If you change three things and the issue disappears, you can't confidently identify the root cause. Isolate variables and test one hypothesis at a time.

This principle applies to programming, infrastructure, databases, and even performance tuning.

## Better Debugging Process

Imagine this:


```
Problem

↓

Hypothesis 1

↓

Test

↓

Still broken

↓

Hypothesis 2

↓

Test

↓

Still broken

↓

Hypothesis 3

↓

Works

↓

Now we know the cause.
```

Instead, we did:

```
Problem

↓

Change A

Change B

Change C

↓

Works

↓

🤔 Which one fixed it?

```

We don't know.

That's why experienced engineers try to change one thing at a time whenever possible.