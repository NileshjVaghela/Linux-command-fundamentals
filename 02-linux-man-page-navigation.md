# Linux for Absolute Beginners - Man Page Navigation

## What is a Man Page?

Linux provides built-in documentation called Manual Pages (man pages).

Think of it as the user guide for a command.

Example:

```bash
man ls
```

This displays documentation for the `ls` command.

---

## Command Structure

```text
man    command
(1)    (2)
```

Example:

```bash
man grep
```

---

## Opening a Man Page

```bash
man ls
man cat
man mkdir
man date
```

---

## Navigating Inside a Man Page

| Key | Action |
|------|---------|
| Enter | Move down one line |
| Space | Move down one page |
| b | Move up one page |
| g | Go to beginning |
| G | Go to end |
| q | Quit |

---

## Searching Inside a Man Page

```text
/keyword
```

Next match:

```text
n
```

Previous match:

```text
N
```

---

## Practice Exercise

Open documentation for ls, cat, mkdir and date.

---

## Challenge Exercise

Using only the `man ls` page, find:

1. The option used to display hidden files.
2. The option used for long listing format.
3. The option used for human-readable file sizes.

---

## MCQ

### Q1. Which command displays documentation for ls?

A. help ls  
B. man ls  
C. ls help  
D. info ls

### Q2. Which key is used to quit a man page?

A. x  
B. z  
C. q  
D. e

---

## Answer Key

### Challenge Exercise

```text
Hidden files : -a
Long listing : -l
Human readable : -h
```

### MCQ Answers

| Question | Answer |
|----------|--------|
| Q1 | B |
| Q2 | C |
