# Linux for Absolute Beginners - File System Navigation

## Think of Linux as a City

Imagine Linux as a city.

Every location has an address.

Some addresses start from the city entrance (`/`).

Others start from where you are currently standing.

---

## The Root Directory

Everything in Linux starts from:

```text
/
```

Think of it as:

```text
/
├── home
├── etc
├── tmp
├── usr
├── var
└── root
```

---

## Common Directories

| Directory | Purpose                     |
| --------- | --------------------------- |
| `/`       | Starting point of Linux     |
| `/home`   | User home directories       |
| `/root`   | Home directory of root user |
| `/etc`    | Configuration files         |
| `/tmp`    | Temporary files             |
| `/var`    | Logs and application data   |
| `/usr`    | Installed programs          |
| `/opt`    | Optional applications       |

---

## Where Am I?

Use:

```bash
pwd
```

Example:

```bash
$ pwd
/home/student
```

Output:

```text
/home/student
```

---

## Two Types of Paths

| Path Type     | Starts With `/` | Example         |
| ------------- | --------------- | --------------- |
| Absolute Path | Yes             | `/home/student` |
| Relative Path | No              | `Documents`     |

---

## Absolute Path

Absolute path always starts from the Root Directory.

Examples:

```text
/home/student
/tmp
/etc
/usr/local
```

Navigate using an absolute path:

```bash
cd /tmp
```

No matter where you are, Linux can find the destination.

---

## Relative Path

Relative path starts from your current location.

Current location:

```text
/home/student
```

Move to Downloads:

```bash
cd Downloads
```

Move to Documents:

```bash
cd ../Documents
```

---

## Special Symbols

| Symbol | Meaning           |
| ------ | ----------------- |
| `.`    | Current directory |
| `..`   | Parent directory  |
| `~`    | Home directory    |

Examples:

```bash
cd .
```

```bash
cd ..
```

```bash
cd ~
```

---

## The cd Command

Command structure:

```text
cd    path
(1)   (2)
```

Examples:

```bash
cd /tmp
```

```bash
cd Downloads
```

```bash
cd ..
```

```bash
cd ~
```

---

## Previous Directory Shortcut

Linux remembers your previous location.

Command:

```bash
cd -
```

Example:

```bash
$ pwd
/home/student

$ cd /tmp

$ cd -

/home/student
```

---

## Getting Help

Display command documentation:

```bash
man cd
```

Display ls documentation:

```bash
man ls
```

---

## Navigating Inside a Manual Page

| Key     | Action         |
| ------- | -------------- |
| `/text` | Search text    |
| `n`     | Next match     |
| `N`     | Previous match |
| `q`     | Quit           |

Example:

```text
/error
```

---

## Practice Exercise

Identify whether the following paths are Absolute or Relative.

```text
/home/student
Downloads
/tmp
../Documents
etc
```

---

## Challenge Exercise

Current location:

```text
/tmp/project/dev
```

Write commands to:

1. Move to `/tmp/project/test`
2. Move to `/home/student`
3. Move to `/tmp`

---

## Answer Key

### Practice Exercise

| Path            | Type     |
| --------------- | -------- |
| `/home/student` | Absolute |
| `Downloads`     | Relative |
| `/tmp`          | Absolute |
| `../Documents`  | Relative |
| `etc`           | Relative |

### Challenge Exercise

```bash
cd ../test
```

```bash
cd /home/student
```

```bash
cd /tmp
```
