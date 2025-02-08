# User Management

Table of Contents

- [User Management](#user-management)
  - [Overview](#overview)
  - [Creating a New User](#creating-a-new-user)
    - [**Example Usage:**](#example-usage)
  - [Setting a User Password](#setting-a-user-password)
  - [Modifying User Properties](#modifying-user-properties)
  - [Adding a User to a Group](#adding-a-user-to-a-group)
  - [Deleting a User](#deleting-a-user)
  - [Source](#source)

## Overview

You'll learn how to create, modify, and delete user accounts, as well as how to set and change passwords.

**List of Commands to Use**

- `useradd` command creates a new user.
- `passwd` command changes user's password.
- `usermod` command modifies user account.
- `groups` command prints groupe membership for a user.
- `userdel` command removes a user account or remove a user from a group.

## Creating a New User

`useradd` command creates a new user.

```bash
sudo useradd [USER_NAME]
```

| **Option**         | **Description**                                                                        |
| ------------------ | -------------------------------------------------------------------------------------- |
| `-m`               | Create the user's home directory if it doesn’t exist.                                  |
| `-d /path`         | Specify a custom home directory for the new user.                                      |
| `-s /bin/bash`     | Set the default shell for the user (e.g., `/bin/bash`, `/bin/sh`, `/bin/zsh`).         |
| `-G group1,group2` | Add the user to additional groups (comma-separated).                                   |
| `-c "comment"`     | Add a comment or full name for the user.                                               |
| `-e YYYY-MM-DD`    | Set an expiration date for the account.                                                |
| `-u UID`           | Assign a specific User ID (UID) to the new user.                                       |
| `-r`               | Create a system user (used for services or daemons, usually without a home directory). |
| `-N`               | Do not create a user group with the same name as the user.                             |
| `-p PASSWORD`      | Set the encrypted password for the user. (Not recommended for plaintext use.)          |

---

### **Example Usage:**

```bash
sudo useradd -m -d /home/johndoe -s /bin/bash -G sudo,developers -c "John Doe" johndoe
```

This command creates a user `johndoe` with a home directory at `/home/johndoe`, sets `/bin/bash` as the default shell, adds the user to the `sudo` and `developers` groups, and attaches the comment "John Doe".

> [!NOTE]
>
> If you try to run this command without sudo, you'll get a "permission denied" error. This is because regular users aren't allowed to create new user accounts - it's a task reserved for system administrators.

To verify that the user was created, we'll examine the /etc/passwd file:

```bash
sudo grep -w '[USER_NAME]' /etc/passwd
```

You should see output similar to:

```
[USER_NAME]:x:5001:5001::/home/[USER_NAME]:/bin/sh
```

This line shows:

- Username
- Password: x (the actual password is stored securely elsewhere)
- User ID: 5001
- Group ID: 5001
- Home Directory
- Default Shell: /bin/sh

## Setting a User Password

`passwd` command changes user's password.

```bash
sudo passwd [USER_NAME]
```

If successful, you'll see a message saying "passwd: password updated successfully".

> [!IMPORTANT]
>
> Behind the scenes, Linux stores encrypted passwords in a secure file called `/etc/shadow`. This is more secure than storing them in the `/etc/passwd` file where anyone could see them.

## Modifying User Properties

`usermod` command modifies user account.

| **Option**         | **Description**                                                                       |
| ------------------ | ------------------------------------------------------------------------------------- |
| `-aG group`        | Add the user to a group without removing them from other groups.                      |
| `-G group1,group2` | Set the user’s groups (overwrites existing groups unless combined with `-a`).         |
| `-d /path`         | Change the user's home directory.                                                     |
| `-m`               | Move the contents of the current home directory to the new location (used with `-d`). |
| `-s /bin/bash`     | Change the user's default shell.                                                      |
| `-l new_name`      | Change the username.                                                                  |
| `-L`               | Lock the user account (disables login).                                               |
| `-U`               | Unlock the user account (re-enables login).                                           |
| `-e YYYY-MM-DD`    | Set the account expiration date.                                                      |
| `-u UID`           | Change the user's UID.                                                                |
| `-c "comment"`     | Modify the user's comment or full name.                                               |

## Adding a User to a Group

`groups` command prints groupe membership for a user.

```bash
groups [USER_NAME]
```

## Deleting a User

`userdel` command removes a user account or remove a user from a group.

| **Option** | **Description**                                                                  |
| ---------- | -------------------------------------------------------------------------------- |
| `-r`       | Remove the user's home directory and mail spool along with the user.             |
| `--force`  | Force deletion of the user account, even if the user is logged in.               |
| `--remove` | Same as `-r`, removes home directory and mail spool.                             |
| `-f`       | Force deletion without checking for logged-in status or files owned by the user. |

## Source

- [User Account Management - LabEx](https://labex.io/tutorials/linux-user-account-management-49)
