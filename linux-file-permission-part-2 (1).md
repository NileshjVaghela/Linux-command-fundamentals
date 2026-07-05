# Linux File Permissions - Part 2 (Symbolic Bits & Octal Numbers)

This session continues from **Part 1** (user / group / other, `ls -l` breakdown). Here we look at how `rwx` permissions map to **binary bits** and then to a single **octal digit (0-7)** — the format used by `chmod`.

## 1. Recap: The Three Permission Blocks

Every file has three permission blocks — **u**ser, **g**roup, **o**ther — each made of three characters:

```
rwx   rwx   rwx
 u     g     o
```

Each block is independent: a file can be fully open to its owner and completely closed to everyone else.

## 2. Turning `rwx` Into Bits

Each of `r`, `w`, `x` is really just a **switch** — either it's granted (`1`) or not (`0`).

| Permission | Present | Absent |
|------------|---------|--------|
| `r` (read) | `1` | `-` → `0` |
| `w` (write) | `1` | `-` → `0` |
| `x` (execute) | `1` | `-` → `0` |

So a single block like `rwx`, `r-x`, or `r--` becomes a 3-bit binary number:

| Symbolic | Binary |
|----------|--------|
| `rwx` | `111` |
| `rw-` | `110` |
| `r-x` | `101` |
| `r--` | `100` |
| `-wx` | `011` |
| `-w-` | `010` |
| `--x` | `001` |
| `---` | `000` |

## 3. Turning Bits Into an Octal Digit

Each position in the 3-bit group has a fixed weight, same as binary place values:

```
 r   w   x
 4   2   1
```

Add up the weights of whichever permissions are **present**. That sum is a single digit from **0 to 7**.

| Symbolic | Bits | Calculation | Octal |
|----------|------|-------------|-------|
| `rwx` | `111` | 4+2+1 | **7** |
| `rw-` | `110` | 4+2+0 | **6** |
| `r-x` | `101` | 4+0+1 | **5** |
| `r--` | `100` | 4+0+0 | **4** |
| `-wx` | `011` | 0+2+1 | **3** |
| `-w-` | `010` | 0+2+0 | **2** |
| `--x` | `001` | 0+0+1 | **1** |
| `---` | `000` | 0+0+0 | **0** |

This gives you every possible combination for **one** block (user, group, or other). Since a file has three blocks, a full permission set is written as **three octal digits**, one per block: `u g o`.

## 4. Full Worked Example

Take this file:

```
-rwxr-xr-- 1 user1 group1 0 Jul 5 10:13 file1
```

Split it into its three blocks and convert each one:

| Block | Symbolic | Binary | Weights (4-2-1) | Octal digit |
|-------|----------|--------|------------------|-------------|
| User | `rwx` | `111` | 4+2+1 | **7** |
| Group | `r-x` | `101` | 4+0+1 | **5** |
| Other | `r--` | `100` | 4+0+0 | **4** |

Put the digits together in order **u g o**:

```
rwxr-xr--  =  7 5 4  =  754
```

So `chmod 754 file1` would produce exactly the permission string `rwxr-xr--`.

## 5. More Practice Conversions

| Symbolic | u | g | o | Octal |
|----------|---|---|---|-------|
| `rwxrwxrwx` | 7 | 7 | 7 | **777** |
| `rw-rw-r--` | 6 | 6 | 4 | **664** |
| `rwxr-xr-x` | 7 | 5 | 5 | **755** |
| `rw-------` | 6 | 0 | 0 | **600** |
| `rwxrwx---` | 7 | 7 | 0 | **770** |
| `r--r--r--` | 4 | 4 | 4 | **444** |

## 6. Going Backwards: Octal → Symbolic

You can reverse the process just as easily. Take each digit and break it back down against the `4-2-1` weights.

Example: convert **640** to symbolic form.

| Digit | Value | Breakdown | Symbolic |
|-------|-------|-----------|----------|
| 6 (user) | 4+2+0 | r + w, no x | `rw-` |
| 4 (group) | 4+0+0 | r only | `r--` |
| 0 (other) | 0+0+0 | nothing | `---` |

Result: `640` → `rw-r-----`

**Trick to memorize:** for any digit 0-7, ask "does 4 fit? does 2 fit? does 1 fit?" — whatever fits, subtract it and mark that letter as granted.

- 4 fits → `r`
- 2 fits → `w`
- 1 fits → `x`

Example: digit **5** → 4 fits (r, remainder 1) → 2 doesn't fit → 1 fits (x) → `r-x`

## 7. Common Octal Permission Patterns

| Octal | Symbolic | Typical use |
|-------|----------|--------------|
| `777` | `rwxrwxrwx` | Fully open — rarely a good idea |
| `755` | `rwxr-xr-x` | Owner full control, everyone else can read/execute (common for scripts, directories) |
| `700` | `rwx------` | Owner-only, nobody else has any access |
| `644` | `rw-r--r--` | Owner can edit, everyone else can only read (common default for regular files) |
| `600` | `rw-------` | Owner-only read/write (private files, keys, credentials) |
| `664` | `rw-rw-r--` | Owner and group can edit, others can only read |

## 8. Real Examples — System Files vs. User-Created Files

Theory is easier to remember once you see it on files that actually exist on every Linux system. Run these yourself:

```bash
ls -ld /tmp
ls -l /etc/passwd
ls -l /etc/shadow
ls -ld /home
ls -ld /home/student
ls -l ~/.bashrc
ls -l ~/.ssh/id_rsa   # if it exists
```

### 8.1 `/tmp` — shared scratch space

```
drwxrwxrwt 10 root root 4096 Jul  5 10:00 /tmp
```

| Block | Symbolic | Octal |
|-------|----------|-------|
| User (root) | `rwx` | 7 |
| Group (root) | `rwx` | 7 |
| Other | `rwt` | 7 |

Everyone gets full `rwx` (**777**) so any user can create files in `/tmp`. Notice the final character is `t`, not `x` — that's the **sticky bit**. It means a user can only delete or rename their *own* files inside `/tmp`, even though the directory itself is world-writable. (We'll cover special bits like sticky, SUID, and SGID in a later session.)

### 8.2 `/etc/passwd` — world-readable user database

```
-rw-r--r-- 1 root root 2210 Jul  5 09:00 /etc/passwd
```

| Block | Symbolic | Octal |
|-------|----------|-------|
| User (root) | `rw-` | 6 |
| Group (root) | `r--` | 4 |
| Other | `r--` | 4 |

Octal **644**. Only `root` can edit it (needed so tools like `useradd` can update it), but everyone can read it — commands like `ls -l`, `id`, and `whoami` all rely on being able to read `/etc/passwd` to resolve usernames.

### 8.3 `/etc/shadow` — password hashes, locked down

```
-rw-r----- 1 root shadow 1230 Jul  5 09:00 /etc/shadow
```

| Block | Symbolic | Octal |
|-------|----------|-------|
| User (root) | `rw-` | 6 |
| Group (shadow) | `r--` | 4 |
| Other | `---` | 0 |

Octal **640**. This file holds encrypted passwords, so unlike `/etc/passwd` it is **not** world-readable — only `root` and members of the `shadow` group can even view it. This is a good real-world reason to use `640` instead of `644`: the data is sensitive.

### 8.4 `/home` — the parent directory for all user home folders

```
drwxr-xr-x 3 root root 4096 Jul  5 09:00 /home
```

Octal **755**. `root` owns and controls it; everyone else can only `cd` into it and list it (`r-x`) — they can't create or delete entries directly under `/home` itself.

### 8.5 A user's own home directory

```
drwxr-xr-x 20 student student 4096 Jul  5 10:11 /home/student
```

Octal **755** by default on many distros — the owner has full control, and others can `cd` in and list contents but not modify anything. Some distros (e.g. RHEL-based) default new home directories to **700** instead, so that other users can't even browse into someone else's home folder at all — worth checking with `ls -ld ~` on your own system.

### 8.6 A user-created regular file

```
-rw-rw-r-- 1 student student 0 Jul  5 10:13 file1.txt
```

Octal **664** — this is the typical *default* permission a brand-new file gets when created with `touch` or a text editor, before any `umask` adjustment. Owner and group can both read/write; everyone else can only read.

### 8.7 A private SSH key

```
-rw------- 1 student student 411 Jul  5 10:20 /home/student/.ssh/id_rsa
```

Octal **600**. SSH will actually **refuse to use the key** and print a warning if permissions are looser than this — a private key must never be readable or writable by anyone other than its owner.

### 8.8 Quick comparison table

| Path | Symbolic | Octal | Why |
|------|----------|-------|-----|
| `/tmp` | `rwxrwxrwt` | 1777 | Shared, world-writable, sticky bit protects others' files |
| `/etc/passwd` | `rw-r--r--` | 644 | Root-editable, world-readable (usernames needed by everyone) |
| `/etc/shadow` | `rw-r-----` | 640 | Sensitive password hashes, root + `shadow` group only |
| `/home` | `rwxr-xr-x` | 755 | Root-controlled, others can browse |
| `~/` (home dir) | `rwxr-xr-x` | 755 | Owner full control, others can browse (varies by distro) |
| New file (`touch`) | `rw-rw-r--` | 664 | Default file creation permission |
| `~/.ssh/id_rsa` | `rw-------` | 600 | Private key — owner-only, enforced by SSH itself |

## 9. Using Octal With `chmod`

```bash
chmod 754 file1     # sets exact permissions: rwxr-xr--
chmod 644 notes.txt  # owner rw-, group r--, other r--
chmod 700 script.sh  # owner-only rwx
```

Unlike symbolic mode (`chmod u+x file1`), octal mode **sets the entire permission string at once** — whatever wasn't included in your calculation is removed, not left as-is.

## 10. Key Takeaways

- `r`, `w`, `x` are individual bits — present (`1`) or absent (`0`).
- The weights `4-2-1` map to `r-w-x` in that fixed order, always.
- Each block (user/group/other) converts independently into **one digit from 0-7**.
- A full permission set is three digits: **u g o** — e.g. `755`, `644`, `600`.
- `chmod <octal> <file>` sets the permission exactly, overwriting whatever was there before.
- Real systems consistently reuse a handful of octal patterns — `644` for regular files, `755` for directories and executables, `600`/`640` for anything sensitive, `777` (plus sticky bit) for shared scratch space like `/tmp`.

## 11. Practice Questions

1. Convert `rw-r--r--` to octal.
2. Convert `rwxrwxr-x` to octal.
3. What symbolic permission does octal `640` represent?
4. What symbolic permission does octal `750` represent?
5. Which is more restrictive: `640` or `600`? Why?
6. `/etc/passwd` is `644` and world-readable, but `/etc/shadow` is `640` and not world-readable. Why does that difference make sense?
7. What octal permission would you expect on a freshly generated SSH private key, and why does SSH enforce it?

### Answer Key

1. `rw-r--r--` → user=6 (rw-), group=4 (r--), other=4 (r--) → **644**
2. `rwxrwxr-x` → user=7 (rwx), group=7 (rwx), other=5 (r-x) → **775**
3. `640` → user=rw- (4+2), group=r-- (4), other=--- (0) → **`rw-r-----`**
4. `750` → user=rwx (4+2+1), group=r-x (4+1), other=--- (0) → **`rwxr-x---`**
5. `600` (`rw-------`) is more restrictive — only the owner has any access at all, whereas `640` (`rw-r-----`) still lets the owning group read the file.
6. `/etc/passwd` only stores usernames and account metadata, which many tools legitimately need to read, so `644` is safe. `/etc/shadow` stores password hashes, which are sensitive, so it's locked to `640` (root + `shadow` group only) to prevent ordinary users from even reading the hashes.
7. **600** (`rw-------`) — owner-only read/write. SSH enforces this because a private key readable by other users (or the group) could be copied and used to impersonate the owner; SSH will refuse to use a key with looser permissions.

---

**Previous session:** [linux-file-permission-part-1.md](./linux-file-permission-part-1.md) | **Next up:** Directory permissions and `chmod` / `chown` in practice (Part 3)
