# Linux for Absolute Beginners - Command Structure

## Who is the Boss?

| OS | Super User | Regular Users |
|----|-----------|---------------|
| Windows | Administrator | Standard users |
| Unix/Linux | `root` (default super user) | Regular users (you create them) |

---

## The 3 Parts of a Linux Command

Every Linux command follows this structure:

```
command   option   argument
  (1)      (2)      (3)
```

**Example:**
```
ls        -l       /home
command   option   argument
```

---

## Rules to Remember

| Rule | Explanation |
|------|-------------|
| Command is **mandatory** | You must type a command, or nothing happens |
| Commands are **case sensitive** | `ls` works, `LS` does NOT work |
| Commands are in **lowercase** | Almost all Linux commands are lowercase |
| **No spaces** inside a command | `ls` is correct, `l s` is wrong |
| **Minimum one space** between command, option, and argument | `ls -l /home` ✅ `ls-l/home` ❌ |

---

## Understanding Options

Options are **optional** — they enhance what the command does.

Options come in two forms:

| Form | Example | Meaning |
|------|---------|---------|
| Full name (double dash) | `--list` | Long/readable form |
| Short name (single dash) | `-l` | Short/quick form |

**Both do the same thing!**

### Combining Short Options

You can combine multiple short options together:

```bash
ls -l -t -a        # separate (works)
ls -lta            # combined (also works, same result!)
```

---

## Reading Command Help

To get help for any command:
```bash
command --help
```

### How to Read the Help Output

```
Usage: mkdir [OPTION]... DIRECTORY...
```

| Symbol | Meaning |
|--------|---------|
| `[OPTION]` | Square brackets `[]` = **not mandatory** (optional) |
| `...` | Three dots = **one or more values** allowed |
| `DIRECTORY` | No brackets = **mandatory** |

**Let's break it down:**

```
Usage: mkdir [OPTION]... DIRECTORY...
             ^^^^^^^^    ^^^^^^^^^
             optional    mandatory
             one or more one or more
```

This means:
- You MUST give at least one directory name
- You CAN give options (but don't have to)
- You can create multiple directories at once

---

## Examples with Breakdown

### Example 1: `ls`

```
Usage: ls [OPTION]... [FILE]...
```

| Part | Mandatory? | One or more? |
|------|-----------|--------------|
| OPTION | No (has `[]`) | Yes (has `...`) |
| FILE | No (has `[]`) | Yes (has `...`) |

```bash
ls                    # no option, no argument (both optional, so OK!)
ls -l                 # with option, no argument
ls /home              # no option, with argument
ls -l -a /home /tmp   # multiple options, multiple arguments
```

### Example 2: `mkdir`

```
Usage: mkdir [OPTION]... DIRECTORY...
```

| Part | Mandatory? | One or more? |
|------|-----------|--------------|
| OPTION | No (has `[]`) | Yes (has `...`) |
| DIRECTORY | **Yes** (no `[]`) | Yes (has `...`) |

```bash
mkdir mydir                  # one directory (OK)
mkdir dir1 dir2 dir3         # three directories at once (OK)
mkdir -p /home/user/a/b/c    # with option -p (create parent dirs)
mkdir                        # ERROR! directory name is mandatory
```

### Example 3: `pwd`

```bash
pwd
```

| Part | Present? |
|------|----------|
| Command | `pwd` |
| Option | None needed |
| Argument | None needed |

`pwd` = **P**rint **W**orking **D**irectory (shows where you are right now)

### Example 4: `head`

```
Usage: head [OPTION]... [FILE]...
```

```bash
head file.txt           # shows first 10 lines (default)
head -n 5 file.txt      # shows first 5 lines
head -n 20 a.txt b.txt  # first 20 lines of both files
```

### Example 5: `tail`

```
Usage: tail [OPTION]... [FILE]...
```

```bash
tail file.txt           # shows last 10 lines (default)
tail -n 3 file.txt      # shows last 3 lines
tail -f /var/log/syslog # follow (live updates) - very useful!
```

---

## More Basic Commands

| Command | What it does | Example |
|---------|-------------|---------|
| `pwd` | Print current directory | `pwd` |
| `ls` | List files and folders | `ls -l /home` |
| `cd` | Change directory | `cd /tmp` |
| `mkdir` | Create directory | `mkdir mydir` |
| `touch` | Create empty file | `touch file.txt` |
| `cat` | Show file content | `cat file.txt` |
| `head` | Show first lines | `head -n 5 file.txt` |
| `tail` | Show last lines | `tail -n 5 file.txt` |
| `cp` | Copy file | `cp file.txt backup.txt` |
| `mv` | Move/rename file | `mv old.txt new.txt` |
| `rm` | Remove/delete file | `rm file.txt` |
| `clear` | Clear the screen | `clear` |

---

## Practice Questions

### Question 1: Identify the Parts

For each command below, identify the **command**, **option**, and **argument**:

```bash
a) ls -l /home
b) mkdir projects
c) head -n 20 report.txt
d) cp -r folder1 folder2
e) rm -f oldfile.txt
```

---

### Question 2: Mandatory or Optional?

Look at this usage line and answer:

```
Usage: cp [OPTION]... SOURCE... DEST
```

a) Is OPTION mandatory?  
b) Can you copy multiple source files?  
c) Is DEST (destination) mandatory?  
d) Write a command to copy 3 files to /tmp  

---

### Question 3: Fix the Errors

What is wrong with these commands?

```bash
a) LS -l /home
b) ls-l /home
c) Mkdir mydir
d) ls -l/home
```

---

### Question 4: True or False

a) Linux commands are case sensitive  
b) Options are always mandatory  
c) `ls -la` and `ls -l -a` give the same result  
d) `root` is the super user in Linux  
e) You can have spaces inside a command name  

---

### Question 5: Command Help

Look at this help output:

```
Usage: tail [OPTION]... [FILE]...
```

a) Is FILE mandatory?  
b) Can you use tail on multiple files?  
c) What does `tail` show by default?  
d) How would you see the last 15 lines of a file called `log.txt`?  

---

### Question 6: Write the Command

a) List all files (including hidden) in long format in `/etc`  
b) Create three directories named `dev`, `test`, `prod` in one command  
c) Show the first 3 lines of `/etc/passwd`  
d) Print your current directory  
e) Show the last 20 lines of `/var/log/syslog`  

---

## Answer Key

### Answer 1:

| # | Command | Option | Argument |
|---|---------|--------|----------|
| a | `ls` | `-l` | `/home` |
| b | `mkdir` | (none) | `projects` |
| c | `head` | `-n 20` | `report.txt` |
| d | `cp` | `-r` | `folder1 folder2` |
| e | `rm` | `-f` | `oldfile.txt` |

### Answer 2:

a) No — it has `[]`  
b) Yes — SOURCE has `...`  
c) Yes — DEST has no `[]`  
d) `cp file1.txt file2.txt file3.txt /tmp`  

### Answer 3:

a) `LS` should be `ls` (lowercase)  
b) No space between `ls` and `-l` → should be `ls -l /home`  
c) `Mkdir` should be `mkdir` (lowercase)  
d) No space between `-l` and `/home` → should be `ls -l /home`  

### Answer 4:

a) **True** — `ls` ≠ `LS`  
b) **False** — options are optional  
c) **True** — short options can be combined  
d) **True** — root is the super user  
e) **False** — no spaces allowed in command names  

### Answer 5:

a) No — it has `[]`  
b) Yes — FILE has `...`  
c) Last 10 lines of a file  
d) `tail -n 15 log.txt`  

### Answer 6:

a) `ls -la /etc`  
b) `mkdir dev test prod`  
c) `head -n 3 /etc/passwd`  
d) `pwd`  
e) `tail -n 20 /var/log/syslog`  
