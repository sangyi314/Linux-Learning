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
* `su - username` switches to a specific user while loading that user's environment variables（recommended）.

Using the hyphen (`-`) is recommended because it creates a complete login session.

---

## Example

```bash
$ whoami
alice

$ su -
Password:

# whoami
root
```

### Output

```text
alice
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
[sudo] password for alice:
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
alice
```

Run a privileged command:

```bash
$ sudo ls /root
```

Continue as the normal user:

```bash
$ whoami
alice
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
alice ALL=(ALL) NOPASSWD: ALL
```

### Meaning

| Field    | Description              |
| -------- | ------------------------ |
| alice    | Username                 |
| ALL      | All hosts                |
| (ALL)    | May act as any user      |
| NOPASSWD | No password required     |
| ALL      | May execute all commands |

This configuration allows **alice** to execute **any command without entering a password**.

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

**End of Part 1**

**Next Part:** **File Ownership and Permissions** (`ls -l`, `chown`, `chmod`, permission numbers, symbolic mode, and the Mermaid permission diagram).
