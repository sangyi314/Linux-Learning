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

```bash
mkdir [OPTION] DIRECTORY
```

### Example

```bash
mkdir /home/sam/test
mkdir project
mkdir -p project/src/docs
```

Ensure that you run this command inside the **Home** directory. Permission errors will occur
if you run it outside the **Home** floder.

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

Display the entire contents of a file.s

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

Means copy.

Copy files or directories.

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

Means move.

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

Means remove.

Delete files or directories.

Examples:

```bash
rm [OPTION] FILE1 FILE2 FILE3 ......

rm file.txt

rm -r project

rm -rf project
```

Wildcards can be used in this command.

Wildcards:

| Pattern | Description          |
| ------- | -------------------- |
| `*`     | Match any characters |

| Option | Description                        |
| ------ | ---------------------------------- |
| `-r`   | Remove directories recursively     |
| `-f`   | Force removal without confirmation |

> ⚠️ **Warning**
>
> Be extremely careful when using `rm -rf`, especially as the **root** user.

*Learned up to here on 09/07/2026* 

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

```bash
find PATH -name PATTERN

find PATH -size PATTERN
```

Examples:

```bash
find . -name "*.txt"

find /home -name "test*"

find . -size +100M

find ~ -size -10k
```

| Size Pattern | Description |
| ------------ | ----------- |
|     `+`      | More than   |
|     `-`      | Less than   | 

Wildcards can be used in this command.

---

# Text Processing

## grep

Means Global Regular Expression and Print

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

Means word count.

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

# Output Commands

## echo

Print text to the terminal.

```bash
echo TEXT
```

Example:

```bash
echo "Hello Linux"

echo "My name is Jack"
```

---

## Backticks (`)

Execute the command inside backticks first, then output its result.

Example:

```bash
echo `pwd`
```

Output:

```text
/home/sam
```

---

# Redirection

Redirect command output to a file.

| Symbol | Description                                  |
| ------ | -------------------------------------------- |
| `>`    | Overwrite the target file.                   |
| `>>`   | Append output to the end of the target file. |

Examples:

```bash
echo "Hello" > test.txt
```

```bash
echo "Linux" >> test.txt
```

```bash
ls > filelist.txt
```

---

## tail

Display the last lines of a file.

```bash
tail [OPTION] FILE
```

Examples:

```bash
tail test.txt
```

```bash
tail -f /var/log/messages
```

Options:

| Option | Description                                                      |
| ------ | ---------------------------------------------------------------- |
| `-f`   | Follow the file and display newly appended content continuously. |
| `-num` | Dispaly the last `num` lines of the file.                        | 

Press CTRL+C to stop the `-f` running.

---

## head

Display the first lines of a file.

```bash
head [OPTION] FILE
```

Options:

| Option | Description                             |
| ------ | --------------------------------------- |
| `-n`   | Specify the number of lines to display. |

Examples:

```bash
head test.txt
```

```bash
head -n 10 test.txt
```

---

# Text Editor

## vim

A built-in Linux text editor.

Open a file:

```bash
vim FILE
```

Example:

```bash
vim test.txt
```

---

## Insert Mode

Enter Insert mode from Command mode.

| Key   | Description                                 |
| ----- | ------------------------------------------- |
| `i`   | Insert before the cursor                    |
| `a`   | Insert after the cursor                     |
| `I`   | Insert at the beginning of the current line |
| `A`   | Insert at the end of the current line       |
| `o`   | Open a new line below and enter Insert mode |
| `O`   | Open a new line above and enter Insert mode |
| `Esc` | Return to Command mode                      |

---

## Cursor Movement

Move the cursor in Command mode.

| Key        | Description                       |
| ---------- | --------------------------------- |
| `↑` or `k` | Move up                           |
| `↓` or `j` | Move down                         |
| `←` or `h` | Move left                         |
| `→` or `l` | Move right                        |
| `0`        | Move to the beginning of the line |
| `$`        | Move to the end of the line       |
| `PageUp`   | Previous page                     |
| `PageDown` | Next page                         |

---

## Search

Search text inside a file.

| Key        | Description    |
| ---------- | -------------- |
| `/keyword` | Search forward |
| `n`        | Next match     |
| `N`        | Previous match |

Example:

```text
/Linux
```

---

## Copy, Delete and Paste

Useful editing commands.

| Key        | Description              |
| ---------- | ------------------------ |
| `dd`       | Delete current line      |
| `ndd`      | Delete **n** lines       |
| `yy`       | Copy current line        |
| `nyy`      | Copy **n** lines         |
| `p`        | Paste below current line |
| `u`        | Undo                     |
| `Ctrl + r` | Redo                     |

Examples:

```text
5dd
```

Delete five lines.

```text
3yy
```

Copy three lines.

---

## Jump Commands

Quickly move to different positions.

| Key  | Description          |
| ---- | -------------------- |
| `gg` | Go to the first line |
| `G`  | Go to the last line  |

---

## Delete Part of a Line

Delete specific parts of the current line.

| Key   | Description                                           |
| ----- | ----------------------------------------------------- |
| `dG`  | Delete from current line to the end of the file       |
| `dgg` | Delete from current line to the beginning of the file |
| `d$`  | Delete from cursor to the end of the line             |
| `d0`  | Delete from cursor to the beginning of the line       |

---

## Last Line Commands

Commands entered after pressing `:`.

| Command      | Description         |
| ------------ | ------------------- |
| `:w`         | Save file           |
| `:q`         | Quit                |
| `:wq`        | Save and quit       |
| `:q!`        | Quit without saving |
| `:set nu`    | Show line numbers   |
| `:set paste` | Enable paste mode   |

Example:

```text
:wq
```

---

# Command Options

Most Linux commands support options.

Common features:

* Options usually begin with `-`
* Multiple short options can often be combined
* Different commands support different options

Example:

```bash
ls -al
```

Equivalent to:

```bash
ls -a -l
```

---

# Help

Display the help information for a command.

```bash
COMMAND --help
```

Example:

```bash
ls --help
```

```bash
cp --help
```

```bash
grep --help
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
* Use the vim to edit files

These commands are the foundation of everyday Linux usage and will be used throughout the rest of this tutorial.

*Learned up to here on 10/07/2026*
