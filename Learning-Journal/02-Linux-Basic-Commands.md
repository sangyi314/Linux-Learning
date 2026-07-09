# Linux Basic Commands

Before learning advanced Linux topics, it is important to master the most frequently used commands. This chapter introduces the Linux directory structure, file navigation, directory management, file manipulation, and basic text processing.

---

# Linux Directory Structure

Unlike Windows, Linux has only **one root directory**, represented by `/`.

Every file and directory exists under the root directory.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

Example:

```text
/home/alice/Documents/file.txt
```

The path above means:

* `/` → Root directory
* `home` → Home directory
* `alice` → User directory
* `Documents` → Folder
* `file.txt` → File

---

# Paths

Linux supports two different path types.

## Absolute Path

An absolute path always starts with `/`.

It describes the location of a file beginning from the root directory.

Example:

```bash
/home/sam/file.txt
```

---

## Relative Path

A relative path does **not** begin with `/`.

It is interpreted relative to the current working directory.

Example:

```bash
Documents/file.txt
```

---

## Special Path Symbols

| Symbol | Description                   |
| ------ | ----------------------------- |
| `.`    | Current directory             |
| `..`   | Parent directory              |
| `~`    | Current user's home directory |

Examples:

```bash
cd ..
cd ../..
cd ~
./script.sh
```

---

# Home Directory

Every Linux user has a dedicated home directory.

| User        | Home Directory     |
| ----------- | ------------------ |
| Normal User | `/home/<username>` |
| root        | `/root`            |

After logging in, the terminal starts inside the current user's home directory.

*Learned up to here on 08/07/2026.*

---

# Working Directory Commands

## pwd

Means print work directory.

Display the current working directory.

```bash
pwd
```

Example output:

```text
/home/sam
```

---

## ls

Means list.

List files and directories.

```bash
ls
```

Useful options:

```bash
ls -l
ls -lh
ls -a
```

| Option | Description              |
| ------ | ------------------------ |
| `-l`   | Long listing format      |
| `-h`   | Human-readable file size |
| `-a`   | Show hidden files        |

> Hidden files and directories begin with a dot (`.`).

---

## cd

Means change directory.

Change the current working directory.

Examples:

```bash
cd Documents
cd ..
cd ~
cd /
```

---

# Creating Files and Directories

## mkdir

Means make directory.

Create one or more directories.

### Syntax

```bash
mkdir [OPTION] DIRECTORY
```

### Example

```bash
mkdir /home/sam/test
mkdir project
mkdir -p project/src/docs
```

| Option | Description                             |
| ------ | --------------------------------------- |
| `-p`   | Create parent directories automatically |

---

## touch

Create an empty file.

```bash
touch notes.txt
```

Multiple files can be created at once.

```bash
touch file1.txt file2.txt file3.txt
```

---

# Viewing Files

## cat

Display the entire contents of a file.

```bash
cat file.txt
```

---

## more

View a large file page by page.

```bash
more file.txt
```

Useful shortcuts:

| Key   | Action    |
| ----- | --------- |
| Space | Next page |
| q     | Quit      |

---

# Copying and Moving Files

## cp

Copy files or directories.

### Syntax

```bash
cp [OPTION] SOURCE DESTINATION
```

Examples:

```bash
cp file.txt backup.txt

cp file.txt backup/

cp -r project project_backup
```

| Option | Description                  |
| ------ | ---------------------------- |
| `-r`   | Copy directories recursively |

---

## mv

Move or rename files.

Examples:

Move a file.

```bash
mv file.txt backup/
```

Rename a file.

```bash
mv old.txt new.txt
```

---

# Removing Files

## rm

Delete files or directories.

Examples:

```bash
rm file.txt

rm -r project

rm -rf project
```

| Option | Description                        |
| ------ | ---------------------------------- |
| `-r`   | Remove directories recursively     |
| `-f`   | Force removal without confirmation |

> ⚠️ **Warning**
>
> Be extremely careful when using `rm -rf`, especially as the **root** user.

---

# Searching

## which

Locate the executable path of a command.

```bash
which python

which ls
```

---

## find

Search for files by name.

### Syntax

```bash
find PATH -name PATTERN
```

Examples:

```bash
find . -name "*.txt"

find /home -name "test*"
```

Wildcards:

| Pattern | Description          |
| ------- | -------------------- |
| `*`     | Match any characters |

---

# Text Processing

## grep

Search for text matching a pattern.

Examples:

```bash
grep "Linux" README.md

grep -n "main" main.c
```

| Option | Description          |
| ------ | -------------------- |
| `-n`   | Display line numbers |

---

## wc

Count lines, words, characters, or bytes.

Examples:

```bash
wc file.txt

wc -l file.txt

wc -w file.txt

wc -c file.txt
```

| Option | Description      |
| ------ | ---------------- |
| `-l`   | Count lines      |
| `-w`   | Count words      |
| `-c`   | Count bytes      |
| `-m`   | Count characters |

---

# Pipe Operator

The pipe operator (`|`) passes the output of one command as the input to another command.

Examples:

```bash
cat file.txt | grep "Linux"

ls -l | grep ".txt"

cat file.txt | wc -l
```

Pipelines can be chained together.

```bash
cat log.txt | grep "ERROR" | wc -l
```

---

# Summary

In this chapter, you learned how to:

* Navigate the Linux file system
* Create and manage files and directories
* Copy, move, and remove files
* Search for files and text
* Count file contents
* Combine commands using pipes

These commands are the foundation of everyday Linux usage and will be used throughout the rest of this tutorial.
