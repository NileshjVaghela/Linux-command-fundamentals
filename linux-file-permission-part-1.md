# Linux File Permissions - Part 1

This session continues from the previous fundamentals sessions. Here we cover how Linux classifies files, how to read `ls -l` output, and how file permissions and ownership actually work.

## 1. File Types in Linux

> **Everything is a file in Linux.**

Some common file types you'll come across:

| Type | Description |
|------|-------------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Link (symbolic link) |

## 2. Finding a File's Type

Use the long listing format to see file types and details:

```bash
ls -l
```

Example output:

```
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Desktop
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Documents
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Downloads
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Music
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Pictures
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Public
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Templates
drwxr-xr-x 2 student student 4096 Jul  5 10:11 Videos
-rw-rw-r-- 1 student student    0 Jul  5 10:13 file1.txt
drwx------ 1 student student    0 Jul  5 10:11 thinclient_drives
```

The **first character** of each line tells you the file type:
- `d` → Directory (Desktop, Documents, Downloads, ...)
- `-` → Regular file (file1.txt)

## 3. Breaking Down `ls -l` Output

Let's take one line and break it into its columns:

```
-rw-rw-r-- 1 student student    0 Jul  5 10:13 file1.txt
```

| Column | Value | Meaning |
|--------|-------|---------|
| 1 | `-` | File type |
| 2 | `rw-rw-r--` | Permissions |
| 3 | `1` | Number of hard links |
| 4 | `student` | User (owner) |
| 5 | `student` | Group (owner) |
| 6 | `0` | File size (bytes) |
| 7 | `Jul 5 10:13` | Last modified timestamp |
| 8 | `file1.txt` | File name |

## 4. Understanding Permission Characters

Each permission block is made of three possible letters:

| Symbol | Name | Meaning |
|--------|------|---------|
| `r` | Read | Can read the file's content |
| `w` | Write | Can modify the file's content |
| `x` | Execute | Can run the file as a command or script |
| `-` | No permission | Permission not granted |

## 5. The Three Permission Groups: User, Group, Other

Every file's permission string (after the file type character) is split into **three sets of three**:

```
-  rwx  r-x  r-x
   ---  ---  ---
    u    g    o
```

Example — a real system file:

```
-rwxr-xr-x 1 root root 142312 Apr  5  2024  ls
```

| Position | Characters | Applies to |
|----------|------------|------------|
| 2-4 | `rwx` | **u**ser (owner) |
| 5-7 | `r-x` | **g**roup |
| 8-10 | `r-x` | **o**ther (everyone else) |

## 6. Worked Example — `user1`, `group1`

Suppose we have five users: `user1, user2, user3, user4, user5`, and `user1` and `user2` belong to `group1`.

```
-rwxr-xr-x 1 user1 group1 ---------- file1
```

Breaking `file1`'s permissions into user / group / other:

```
rwx   r-x   r-x
 u     g     o
```

How access is decided when someone tries to open `file1`:

- **user1** accesses `file1` → user1 *is* the owner → **user permission** applies (`rwx`)
- **user2** accesses `file1` → user2 is not the owner but *is* in `group1` → **group permission** applies (`r-x`)
- **user3** accesses `file1` → not the owner, not in the group → **other permission** applies (`r-x`)

**The rule Linux follows, in order:**

```
Is it the owner?  --Yes--> apply USER permission
       |No
       v
Is it in the owning group? --Yes--> apply GROUP permission
       |No
       v
Otherwise --> apply OTHER permission
```

## 7. More Practice Files

```
-rw-rw-r-- 1 user2 group1 0  Apr  4  2025 file2
-rwxr-xr-x 1 user1 group1 10 Mar  6  2026 file3
-rw-rw-r-- 1 root  root   50 Apr  3  2022 file4
```

- `file2` is owned by `user2`, group `group1`
- `file3` is owned by `user1`, group `group1`
- `file4` is owned by `root`, group `root`

Try working out for yourself what permission `user3` would get on each of these (hint: `user3` is not the owner and not in any of these groups, so **other** always applies).

## 8. Worked Example — Users & Groups (Ramayan / Gita)

Setup:

- Users: `ram, shyam, sita, radha, pita`
- Group `ramayan` → members: `ram, sita`
- Group `gita` → members: `shyam, radha`
- `pita` belongs to their own group `pita` (not a member of `ramayan` or `gita`)

Files:

```
-rw-rw-rw- 1 ram   ramayan  ---------- file11
-rw-rw-r-- 1 sita  ramayan  --------- file12
-rw-rw-r-- 1 shyam gita     ----------- file13
-rw-rw-r-- 1 radha gita     ----------- file14
-rw-rw-rw- 1 pita  pita     ----------- file15
```

### Decision flow for any user accessing a file

```
        user --------------------------- file
         |  if owner?        |  if in group?
         v                    v
       USER                GROUP  ------> otherwise: OTHER
```

### Case 1: What permission does `shyam` get?

| File | Owner | Group | Is `shyam` the owner? | Is `shyam` in the group? | Permission applied |
|------|-------|-------|------------------------|----------------------------|----------------------|
| file11 | ram | ramayan | No | No (shyam is in `gita`, not `ramayan`) | **Other** → `rw-` |
| file12 | sita | ramayan | No | No | **Other** → `r--` |
| file13 | shyam | gita | **Yes** | — | **User** → `rw-` |
| file14 | radha | gita | No | **Yes** (shyam is in `gita`) | **Group** → `rw-` |
| file15 | pita | pita | No | No | **Other** → `rw-` |

### Case 2: What permission does `ram` get?

| File | Owner | Group | Is `ram` the owner? | Is `ram` in the group? | Permission applied |
|------|-------|-------|-----------------------|--------------------------|----------------------|
| file11 | ram | ramayan | **Yes** | — | **User** → `rw-` |
| file12 | sita | ramayan | No | **Yes** (ram is in `ramayan`) | **Group** → `rw-` |
| file13 | shyam | gita | No | No | **Other** → `r--` |

## 9. Key Takeaways

- Every file has exactly **one owner** and **one owning group**.
- Permissions are checked in a strict order: **owner → group → other**. Linux stops at the first match — it never checks group permission for the owner, even if the owner's permission is more restrictive.
- `r`, `w`, and `x` mean different things for files vs. directories (directories are covered in the next session).
- Use `ls -l` to inspect type, permissions, ownership, size, and modification time in one shot.

## 10. Practice Questions

Using the `ram / shyam / sita / radha / pita` setup from Section 8, answer the following:

1. What permission does `sita` get on `file13` (owned by `shyam`, group `gita`)?
2. What permission does `radha` get on `file11` (owned by `ram`, group `ramayan`)?
3. What permission does `pita` get on `file14` (owned by `radha`, group `gita`)?
4. Why does `ram` get **group** permission on `file12` instead of **other**, even though he's not the owner?
5. If a file's permission string is `rwxr-----`, what can someone in the owning group do with it?

### Answer Key

1. `sita` is not the owner and not in group `gita` → **Other** permission → `r--`
2. `radha` is not the owner and not in group `ramayan` → **Other** permission → `rw-`
3. `pita` is not the owner and not in group `gita` → **Other** permission → `rw-`
4. Because Linux checks **group membership**, not just "are you the owner." `ram` is a member of the `ramayan` group (which owns `file12`), so group permission applies to him even though `sita` is the actual owner.
5. `rwxr-----` → group block is `r--`, so a group member can only **read** the file — no write, no execute.

---

**Previous session:** [linux-basic-command-part-1.md](./linux-basic-command-part-1.md) | **Next up:** Directory permissions and `chmod` / `chown` (Part 2)
