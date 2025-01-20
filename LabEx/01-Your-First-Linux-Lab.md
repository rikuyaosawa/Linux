# Your First Linux Lab

Table of Contents

- [Your First Linux Lab](#your-first-linux-lab)
  - [Hello LabEx](#hello-labex)
  - [Displaying the Current User](#displaying-the-current-user)
  - [Displaying User and Group Information](#displaying-user-and-group-information)
  - [htop System Monitor](#htop-system-monitor)

## Hello LabEx

> `echo` command prints given arguments to the terminal

A few important things to remember:

-   Linux is case-sensitive. This means echo, Echo, and ECHO are all different to Linux. Always type commands exactly as you see them.
-   Spaces are important in Linux commands. Make sure you have a space between `echo` and the opening quotation mark.
-   The quotation marks tell echo exactly what to repeat. Without them, `echo` might get confused.

## Displaying the Current User

> `whoami` command prints the username associated with the current effective user ID

The `whoami` command is useful when you're working on different computers or with different user accounts. It's a quick way to check which user account you're currently using.

## Displaying User and Group Information

Now let's dive a little deeper into user information with the `id` command.

> `id` command displays current user and group identity

In Linux, every user belongs to one or more groups. These groups help manage permissions - what each user is allowed to do on the system.

You'll see output similar to this:j

```bash
uid=5000(labex) gid=5000(labex) groups=5000(labex),27(sudo),121(ssl-cert),5002(public)
```

Let's break this down:

-   `uid` stands for User ID. Every user has a unique number.
-   `gid` is the Group ID of your primary group.
-   `groups` lists all the groups you belong to.

## htop System Monitor

Now that we've explored some basic commands, let's install a useful tool called `htop`.

> `htop` command displays dynamic real-time information about running processes. An enhanced version of `top`.

This is a program that shows you what's happening inside your computer - like a dashboard for your system.

The `htop` screen shows:

-   At the top: CPU usage, memory usage, and how long your computer has been running.
-   In the middle: A list of all the programs (processes) currently running.
-   At the bottom: Options for interacting with htop.

You can use the arrow keys on your keyboard to move around.
When you're done exploring, press the `q` key to quit.

---

END
