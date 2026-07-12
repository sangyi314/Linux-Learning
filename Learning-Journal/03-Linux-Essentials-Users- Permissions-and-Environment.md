# Chapter 3 — Users, Groups, and Permissions

> **Learn how Linux manages users, groups, file ownership, permissions, and essential file operations.**
>
> This chapter introduces the fundamental concepts of Linux user management, privilege escalation, permission control, environment variables, and common file management commands.

---

# Executive Summary

In this chapter, you will learn how to:

* Switch between different user accounts.
* Execute commands with administrator privileges.
* Understand Linux file ownership and permission models.
* Change file ownership and permissions.
* Create and manage users and groups.
* View user and group information.
* Work with environment variables.
* Perform common file operations.
* Edit text files using Linux editors.
* Publish your work to GitHub.

---

# Introduction

Linux is a **multi-user operating system**, meaning multiple users can work on the same system simultaneously while maintaining independent permissions and environments.

To use Linux efficiently, you need to understand how users, groups, and permissions work together to protect files and system resources.

This chapter covers the essential commands and concepts every Linux beginner should master, including practical command examples, expected outputs, and common mistakes to avoid.

By the end of this chapter, you should be able to confidently perform common user administration tasks and manage file permissions in a Linux environment.

---

# Learning Objectives

After completing this chapter, you should be able to:

* Understand the difference between **root** and normal users.
* Switch users using `su`.
* Execute commands with `sudo`.
* Read Linux file ownership and permissions.
* Modify ownership using `chown`.
* Change permissions using `chmod`.
* Create and manage users and groups.
* View user IDs and group memberships.
* Display environment variables.
* Perform common file operations.
* Edit files using `vi` or `nano`.

---

# 1. Switching Users and Privileges

Linux separates ordinary users from administrators to improve system security.

The administrator account is called the **root user**, also known as the **superuser**.

Normal users have limited permissions, while the root user has unrestricted access to the entire system.

Two important commands are used for privilege management:

* `su`
* `sudo`

---

# 1.1 The `su` Command

## Purpose

Switch to another user account.

If no username is specified, `su` switches to the **root** user by default.

---

## Syntax

```bash
su
```

or

```bash
su -
```

or

```bash
su - username
```

---

## Description

* `su` switches to another user.
* `su -` loads the target user's **login environment**.
* `su - username` switches to a specific user while loading that user's environment variables.

Using the hyphen (`-`) is recommended because it creates a complete login session.

---

## Example

```bash
$ whoami
sam

$ su -
Password:

# whoami
root
```

### Output

```text
sam
root
```

The prompt changes from:

```text
$
```

to

```text
#
```

which indicates that you are now the **root** user.

---

## Return to the Previous User

To leave the root account:

```bash
exit
```

or press

```text
Ctrl + D
```

---

## Notes

* `su` requires the **target user's password**.
* `su -` also changes:

  * HOME
  * PATH
  * Shell environment
  * Working directory

---

# 1.2 The `sudo` Command

## Purpose

Execute a single command with administrator (root) privileges.

Unlike `su`, `sudo` does **not** switch users permanently.

---

## Syntax

```bash
sudo command
```

---

## Example

```bash
$ sudo ls /root
[sudo] password for sam:
rootfile.txt
```

---

## Explanation

When running a command with `sudo`:

1. Enter **your own password**.
2. The command executes with root privileges.
3. After execution, you automatically return to your normal user account.

Unlike `su`, `sudo` does **not** require the root password.

---

## Example Workflow

Current user:

```bash
$ whoami
sam
```

Run a privileged command:

```bash
$ sudo ls /root
```

Continue as the normal user:

```bash
$ whoami
sam
```

---

## Advantages of `sudo`

| Feature                              | `su`      | `sudo` |
| ------------------------------------ | --------- | ------ |
| Switch user                          | ✔         | ✘      |
| Requires root password               | Usually ✔ | ✘      |
| Requires your own password           | ✘         | ✔      |
| Runs one command as root             | ✘         | ✔      |
| Recommended for daily administration | ✘         | ✔      |

---

# 1.3 The `/etc/sudoers` File

User permissions for `sudo` are configured in:

```text
/etc/sudoers
```

This file determines:

* Who can use `sudo`
* Which commands they can execute
* Whether a password is required

---

## Recommended Editing Method

Never edit the file directly.

Instead, use:

```bash
sudo visudo
```

`visudo` performs syntax checking before saving changes, helping prevent configuration errors.

---

## Example Configuration

```text
sam ALL=(ALL) NOPASSWD: ALL
```

### Meaning

| Field    | Description              |
| -------- | ------------------------ |
| sam      | Username                 |
| ALL      | All hosts                |
| (ALL)    | May act as any user      |
| NOPASSWD | No password required     |
| ALL      | May execute all commands |

This configuration allows **sam** to execute **any command without entering a password**.

---

# 1.4 Summary

| Command        | Purpose                                |
| -------------- | -------------------------------------- |
| `su`           | Switch to another user                 |
| `su -`         | Switch user and load login environment |
| `sudo command` | Execute one command as root            |
| `whoami`       | Display the current user               |
| `exit`         | Return to the previous user            |
| `sudo visudo`  | Safely edit the sudoers file           |

---

## Best Practices

> **Use `sudo` whenever possible instead of logging in as the root user.**

This approach is safer because:

* It limits accidental system changes.
* Every privileged command is explicitly authorized.
* Administrative actions are easier to audit.
* You remain in your normal user environment.

---

# 2. File Ownership and Permissions

Every file and directory in Linux belongs to:

* **An owner (user)**
* **A group**
* **A permission set**

Linux uses these attributes to determine **who can read, modify, or execute a file**.

---

# 2.1 Viewing File Permissions

The `ls -l` command displays detailed information about files and directories.

## Syntax

```bash
ls -l
```

---

## Example

```bash
$ ls -l example.txt
-rw-r--r-- 1 sam staff 98 Jul 10 12:34 example.txt
```

---

## Output Breakdown

| Field          | Meaning                   |
| -------------- | ------------------------- |
| `-rw-r--r--`   | File type and permissions |
| `1`            | Number of hard links      |
| `sam`          | Owner                     |
| `staff`        | Group                     |
| `98`           | File size (bytes)         |
| `Jul 10 12:34` | Last modified time        |
| `example.txt`  | File name                 |

---

# 2.2 Understanding Permission Strings

The first field shown by `ls -l` describes the file type and permissions.

Example:

```text
-rw-r--r--
```

This string contains **10 characters**.

```
-rw-r--r--
││││││││││
│││││││││└── Others Execute
││││││││└─── Others Write
│││││││└──── Others Read
││││││└───── Group Execute
│││││└────── Group Write
││││└─────── Group Read
│││└──────── Owner Execute
││└───────── Owner Write
│└────────── Owner Read
└─────────── File Type
```

---

## File Type

The first character indicates the file type.

| Symbol | Meaning      |
| ------ | ------------ |
| `-`    | Regular file |
| `d`    | Directory    |

---

## Permission Characters

| Symbol | Meaning       |
| ------ | ------------- |
| `r`    | Read          |
| `w`    | Write         |
| `x`    | Execute       |
| `-`    | No permission |

---

## Permission Groups

Permissions are divided into three groups.

| Position     | Represents                  |
| ------------ | --------------------------- |
| User (Owner) | File owner                  |
| Group        | Members of the file's group |
| Others       | Everyone else               |

Example:

```text
-rw-r--r--
```

becomes

| User | Group | Others |
| ---- | ----- | ------ |
| rw-  | r--   | r--    |

Meaning:

* **Owner** → Read + Write
* **Group** → Read Only
* **Others** → Read Only

---

## Directory Permissions

For directories, permissions have slightly different meanings.

| Permission | Meaning                                                   |
| ---------- | --------------------------------------------------------- |
| `r`        | View directory contents                                   |
| `w`        | Create, delete, or rename files (with execute permission) |
| `x`        | Enter (`cd`) the directory                                |

Without **execute (`x`) permission**, users cannot access the directory even if they can read its contents.

---

# 2.3 Changing File Ownership (`chown`)

The `chown` command changes the owner and/or group of a file or directory.

---

## Syntax

```bash
chown owner file
```

```bash
chown owner:group file
```

```bash
chown :group file
```

---

## Examples

Change owner:

```bash
sudo chown bob example.txt
```

Change owner and group:

```bash
sudo chown bob:staff example.txt
```

Change only the group:

```bash
sudo chown :staff example.txt
```

---

## Example Session

```bash
$ sudo chown bob:staff example.txt

$ ls -l example.txt

-rw-r--r-- 1 bob staff 98 Jul 10 12:34 example.txt
```

The owner becomes **bob**, and the group becomes **staff**.

---

## Recursive Ownership Change

Apply ownership changes to all files and subdirectories.

```bash
sudo chown -R bob:staff project/
```

---

## Notes

* Only **root** or the file owner (subject to system policy) can change ownership.
* The `-R` option applies changes recursively.

---

# 2.4 Changing Permissions (`chmod`)

The `chmod` command modifies file or directory permissions.

Linux supports two methods:

* Numeric (Octal)
* Symbolic

---

# Numeric Mode

Each permission has a numeric value.

| Permission | Value |
| ---------- | ----: |
| Read       |     4 |
| Write      |     2 |
| Execute    |     1 |

Permissions are calculated by adding these values together.

---

## Permission Value Table

| Number | Permission | Meaning                |
| -----: | ---------- | ---------------------- |
|      7 | rwx        | Read + Write + Execute |
|      6 | rw-        | Read + Write           |
|      5 | r-x        | Read + Execute         |
|      4 | r--        | Read Only              |
|      3 | -wx        | Write + Execute        |
|      2 | -w-        | Write Only             |
|      1 | --x        | Execute Only           |
|      0 | ---        | No Permission          |

---

## Common Permission Modes

| Command               | Result      |
| --------------------- | ----------- |
| `chmod 644 file`      | `rw-r--r--` |
| `chmod 755 script.sh` | `rwxr-xr-x` |
| `chmod 600 file`      | `rw-------` |
| `chmod 777 file`      | `rwxrwxrwx` |

---

## Examples

```bash
chmod 644 file.txt
```

Result:

```text
rw-r--r--
```

---

```bash
chmod 755 script.sh
```

Result:

```text
rwxr-xr-x
```

---

# Symbolic Mode

Instead of numbers, symbolic mode uses letters.

## User Classes

| Symbol | Meaning   |
| ------ | --------- |
| `u`    | Owner     |
| `g`    | Group     |
| `o`    | Others    |
| `a`    | All users |

---

## Example

```bash
chmod u=rwx,g=rx,o=rx script.sh
```

This produces the same result as:

```bash
chmod 755 script.sh
```

---

## Recursive Permission Change

```bash
chmod -R 755 project/
```

This command applies permissions to every file and subdirectory under `project/`.

---

## Example Output

```bash
$ ls -l script.sh

-rwxr-xr-x 1 sam staff 200 Jul 10 12:40 script.sh
```

---

# 2.6 Summary

| Command                      | Purpose                             |
| ---------------------------- | ----------------------------------- |
| `ls -l`                      | View file permissions and ownership |
| `chown owner file`           | Change file owner                   |
| `chown owner:group file`     | Change owner and group              |
| `chmod 644 file`             | Set numeric permissions             |
| `chmod u=rwx,g=rx,o=rx file` | Set symbolic permissions            |
| `chmod -R`                   | Apply changes recursively           |

---

## Best Practices

> **Verify permission changes using `ls -l` after running `chmod` or `chown`.**

Remember:

* Use **numeric mode** (`644`, `755`) for quick and common permission settings.
* Use **symbolic mode** when modifying specific permissions.
* Be cautious with recursive (`-R`) operations, as they affect all files and directories within the target path.
* Avoid granting unnecessary permissions, especially `777`, unless absolutely required.

---

# 3. User and Group Management

Linux is designed to support multiple users working on the same system.

To organize users efficiently, Linux uses **groups**, allowing multiple users to share permissions and resources.

This section introduces the most commonly used commands for managing users and groups.

---

# 3.1 Linux User and Group Basics

Every Linux user has:

* A **User ID (UID)**
* A **Primary Group (GID)**
* One or more **Supplementary Groups**
* A **Home Directory**
* A **Login Shell**

For example:

```text
User: sam
UID : 1001
Primary Group : sam
Home Directory : /home/sam
Shell : /bin/bash
```

---

# 3.2 Creating a Group (`groupadd`)

## Purpose

Create a new user group.

---

## Syntax

```bash
sudo groupadd groupname
```

---

## Example

```bash
sudo groupadd devteam
```

This creates a new group named **devteam**.

---

## Notes

* Only users with administrative privileges can create groups.
* Group information is stored in:

```text
/etc/group
```

---

# 3.3 Creating a User (`useradd`)

## Purpose

Create a new user account.

---

## Syntax

```bash
sudo useradd username
```

Create a user **with a home directory**:

```bash
sudo useradd -m username
```

---

## Common Options

| Option | Description               |
| ------ | ------------------------- |
| `-m`   | Create a home directory   |
| `-g`   | Specify the primary group |
| `-G`   | Add supplementary groups  |

---

## Example

```bash
sudo useradd -m sam
```

This command creates:

```text
/home/sam
```

as sam's home directory.

---

## Example Session

```bash
$ sudo useradd -m sam

$ id sam

uid=1001(sam)
gid=1001(sam)
groups=1001(sam)
```

---

## Notes

Creating a user **without `-m`** does **not** automatically create a home directory.

---

# 3.4 Viewing User Information (`id`)

## Purpose

Display user identity information.

---

## Syntax

```bash
id
```

Current user:

```bash
id
```

Specific user:

```bash
id username
```

---

## Example

```bash
id sam
```

Output:

```text
uid=1001(sam) gid=1001(sam) groups=1001(sam)
```

---

## Output Explanation

| Field  | Meaning                        |
| ------ | ------------------------------ |
| UID    | User ID                        |
| GID    | Primary Group ID               |
| Groups | All groups the user belongs to |

---

# 3.5 Adding a User to a Group (`usermod`)

## Purpose

Modify an existing user account.

One common use is adding users to supplementary groups.

---

## Syntax

```bash
sudo usermod -aG groupname username
```

---

## Option Explanation

| Option | Meaning              |
| ------ | -------------------- |
| `-a`   | Append               |
| `-G`   | Supplementary groups |

> **Important:** Always use `-aG` together. Omitting `-a` may replace the user's existing supplementary groups.

You can use the command "sudo gpasswd -d username groupname" to delete the user from the group.

---

## Example

```bash
sudo usermod -aG devteam sam
```

---

## Verify the Result

```bash
id sam
```

Output:

```text
uid=1001(sam)
gid=1001(sam)
groups=1001(sam),1002(devteam)
```

sam now belongs to the **devteam** group.

---

# 3.6 Deleting Users (`userdel`)

## Purpose

Remove a user account.

---

## Syntax

```bash
sudo userdel username
```

Remove the user **and** their home directory:

```bash
sudo userdel -r username
```

---

## Example

```bash
sudo userdel sam
```

or

```bash
sudo userdel -r sam
```

---

## Option Comparison

| Command            | Result                                   |
| ------------------ | ---------------------------------------- |
| `userdel sam`      | Remove the user only                     |
| `userdel -r sam`   | Remove the user and their home directory |

---

## Notes

By default, the user's home directory remains unless the `-r` option is used.

---

# 3.7 Deleting Groups (`groupdel`)

## Purpose

Delete an existing group.

---

## Syntax

```bash
sudo groupdel groupname
```

---

## Example

```bash
sudo groupdel devteam
```

---

## Notes

A group cannot be deleted if it is still the primary group of an existing user.

---

# 3.8 Listing Users and Groups (`getent`)

## Purpose

Display entries from Linux system databases.

---

## Syntax

List all users:

```bash
getent passwd
```

List all groups:

```bash
getent group
```

---

## Example

```bash
$ getent passwd

root:x:0:0:root:/root:/bin/bash
sam:x:1001:1001::/home/sam:/bin/bash
...
```

---

```bash
$ getent group

root:x:0:
staff:x:50:
devteam:x:1002:sam
...
```

---

## Explanation

### `getent passwd`

Displays information such as:

* Username
* UID
* GID
* Home directory
* Login shell

### `getent group`

Displays:

* Group name
* Group ID
* Group members

---

## Why Use `getent`?

Unlike reading `/etc/passwd` or `/etc/group` directly, `getent` retrieves entries from the system's configured databases, making it more reliable in environments that use network-based user directories.

---

# 3.9 Complete Example

The following example demonstrates a typical user and group management workflow.

```bash
# Create a group
sudo groupadd devteam

# Create a user
sudo useradd -m sam

# View user information
id sam

# Add the user to a group
sudo usermod -aG devteam sam

# Verify group membership
id sam
```

Expected output:

```text
uid=1001(sam)
gid=1001(sam)
groups=1001(sam),1002(devteam)
```

---

# 3.10 Summary

| Command         | Purpose                                |
| --------------- | -------------------------------------- |
| `groupadd`      | Create a group                         |
| `useradd`       | Create a user                          |
| `useradd -m`    | Create a user with a home directory    |
| `usermod -aG`   | Add a user to a supplementary group    |
| `userdel`       | Delete a user                          |
| `userdel -r`    | Delete a user and their home directory |
| `groupdel`      | Delete a group                         |
| `id`            | Display user information               |
| `getent passwd` | List all users                         |
| `getent group`  | List all groups                        |

---

## Best Practices

> **Always verify changes after managing users or groups.**

Recommended commands:

```bash
id username
```

```bash
getent passwd
```

```bash
getent group
```

These commands help confirm that users, groups, and memberships have been created or modified correctly.

---

> **In this chapter, we learned the fundamentals of Linux user and permission management. We explored how to switch users and execute commands with administrative privileges, manage file ownership and permissions, and create, modify, and organize users and groups. Mastering these essential concepts provides a solid foundation for secure system administration and efficient Linux file management.**
