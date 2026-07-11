# Linux Essentials: Users, Permissions, and Environment

## Introduction

This chapter focuses on some of the most important Linux basics: switching users, managing privileges, changing file permissions, modifying file ownership, and checking user, group, and environment information. These commands are foundational for daily Linux operations.

---

## 1. Switching Users with su

The su command is used to switch from the current user to another user.

```bash
 su [-] [user] 
```

### Example

```bash
 su root 
```

Use - to start a full login shell for the target user(Recommended)

```bash
 su - username 
```

### Notes

- su means substitute user.
- su - loads the target user’s environment.
- It is commonly used when you need to act as another user, especially root.

---
## 2. Running Commands with sudo

The sudo command allows an authorized user to run commands with elevated privileges.

### Example

```bash
 sudo apt update 
```

### Passwordless sudo

You can configure sudo in visudo so that a user can run commands without entering a password.

Example configuration:

text sam ALL=(ALL) NOPASSWD: ALL 

This means:

- sam can run commands as any user
- sam can run all commands
- No password is required

> Use passwordless sudo carefully, because it weakens system security.

---

## 3. Changing Permissions with chmod

The chmod command is used to change file or directory permissions.

### Syntax

bash chmod [-R] permissions file_or_directory 

### Example

bash chmod 755 test.sh 

The permission 755 means:

- Owner: rwx
- Group: r-x
- Others: r-x

So 755 is equivalent to:

text rwxr-xr-x 

### Recursive Mode

The -R option applies the change to a directory and everything inside it.

bash chmod -R 755 myfolder 

---

## 4. Changing Ownership with chown

The chown command is used to change the owner and/or group of a file or directory.

### Syntax

bash chown [-R] [user][:group] file_or_directory 

### Examples

Change the owner of a file:

bash chown alice file.txt 

Change both the owner and the group:

bash chown alice:developers file.txt 

Change ownership recursively for a directory:

bash chown -R alice:developers project/ 

### Notes

- -R applies the change to a directory and all its contents.
- This command is useful for managing access and file responsibility.

---

## 5. Managing Users and Groups

Linux stores user and group information in the system database. You can view them with getent.

### View All Groups

bash getent group 

This displays all groups on the system.

### View All Users

bash getent passwd 

This displays all user accounts on the system.

---

## 6. Viewing Environment Variables with env

The env command shows the current environment variables.

### Syntax

bash env 

### Example

bash env 

Common environment variables include:

text HOME=/home/user PATH=/usr/local/bin:/usr/bin USER=user SHELL=/bin/bash 

Environment variables are used by the shell and programs to store configuration and runtime information.

---

## Summary

| Command | Purpose |
|--------|---------|
| su | Switch to another user |
| sudo | Run commands with elevated privileges |
| chmod | Change file or directory permissions |
| chown | Change file owner or group |
| getent group | View all groups |
| getent passwd | View all users |
| env | Display environment variables |

---

## Key Takeaways

- su and sudo are both related to privilege management, but they work in different ways.
- chmod and chown are essential for controlling access to files and directories.
- getent helps inspect system user and group information.
- env is useful for checking the environment variables available in the current shell.

---

Chapter 3 notes for quick review and GitHub documentation.
