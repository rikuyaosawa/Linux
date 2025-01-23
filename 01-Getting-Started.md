# Getting Started

Table of Contents

- [Getting Started](#getting-started)
  - [What is Linux?](#what-is-linux)
    - [Components of the Linux Operating System](#components-of-the-linux-operating-system)
  - [Understanding Directory Hierarchy](#understanding-directory-hierarchy)
  - [Command Path](#command-path)
  - [Environment Variable](#environment-variable)
    - [Commands](#commands)
    - [Variable Scope](#variable-scope)
    - [Making Environment Variables Permanent](#making-environment-variables-permanent)
    - [Important Environment Variables](#important-environment-variables)
  - [Getting Help on Commands](#getting-help-on-commands)
  - [Redirects](#redirects)
  - [Source](#source)

## What is Linux?

Linux is an operating system, much like Windows, iOS, and Mac OS. It powers many devices, including **Android**, one of the most popular platforms worldwide. An operating system (OS) manages hardware resources and facilitates communication between software and hardware. Without an OS, software cannot function.

### Components of the Linux Operating System

1. **Bootloader**

    - Manages the boot process of your computer.
    - Often appears as a splash screen during startup.

2. **Kernel**

    - The core of the system, responsible for managing the CPU, memory, and peripherals.
    - The only part officially called "Linux."

3. **Init System**

    - Bootstraps the user space and controls daemons.
    - Commonly used init system: **systemd** (though controversial).

4. **Daemons**

    - Background services, such as printing, sound, and scheduling.
    - Run during boot or after logging in.

5. **Graphical Server**

    - Displays graphics on the monitor.
    - Often referred to as the **X server** or just **X**.

6. **Desktop Environment**

    - The user interface, offering various options like **GNOME**, **Cinnamon**, **KDE**, **Xfce**, etc.
    - Includes built-in applications like file managers, web browsers, and configuration tools.

7. **Applications**
    - Linux provides thousands of high-quality apps.
    - Most distributions include app store-like tools for easy installation (e.g., **Ubuntu Software Center**).

Linux is highly customizable and provides a powerful, versatile experience for both casual users and professionals.

## Understanding Directory Hierarchy

In Linux, understanding the directory hierarchy is crucial for efficient navigation and file management. A Linux system's directory structure, also known as **the Filesystem Hierarchy Standard (FHS)**, is a defined tree structure that helps to prevent files from being scattered all over the system and instead organise them in a logical and easy-to-navigate manner.

-   `/`: Root directory, the top level of the file system.
-   `/home`: User home directories.
-   `/bin`: Essential binary executables.
-   `/sbin`: System administration binaries.
-   `/etc`: Configuration files.
-   `/var`: Variable data (logs, spool files).
-   `/usr`: User programs and data.
-   `/lib`: Shared libraries.
-   `/tmp`: Temporary files.

## Command Path

In Linux, **the command path** is an important concept under shell basics. Simply put, command path is a variable that is used by the shell to determine where to look for the executable files to run. Linux commands are nothing but programs residing in particular directories. But, one does not have to navigate to these directories every time to run these programs. The command path comes to the rescue!

These paths are stored in the `$PATH` environment variable.

```sh
echo $PATH
```

Running this command in a Linux terminal will return all the directories that the shell will search, in order, to find the command it has to run.
The directories are separated by a colon.

## Environment Variable

In Linux, you can create a variable by simply assigning a value to a name. To view the value of the variable, we use the echo command with a `$` before the variable name.

You can also use variables within other commands or assignments. For example:

```bash
echo "The value of my_var is: $my_var"
```

Now that we understand basic variables, let's explore environment variables.

> **Environment variables** are special variables accessible to any child process of the shell. These can be used by scripts and programs initiated from the shell.

### Commands

-   `env`: View all current environment variables

-   `echo $PATH`: Check the PATH variable

-   `export`: exports shell variables to child processes

    -   `export MY_ENV_VAR="This is an environment variable"`: Create an environment variable

-   `env | grep MY_ENV_VAR`: Verify the environment variable is set

-   `echo $MY_ENV_VAR`: Check the value of the environment variable

-   `unset`: To unset (remove) any variable
    -   You can also use the `-v` option with `unset` to ensure you're unsetting a variable and not a shell function:

### Variable Scope

Environment variables and shell variables each have their own scope. When you export a variable (e.g., `export MY_ENV_VAR="something"`), it becomes available to any subprocess started from that shell (for example, a shell script run by that same shell).

However, if you open a completely separate shell session, it does not inherit the variables from your current shell unless you specifically set them in a startup file (like .zshrc or .bashrc).

In other words:

-   A regular shell variable is visible only within the current session.
-   An exported variable is available to child processes launched from that session.
-   variable set in a shell startup file (like .zshrc) is applied to all new sessions of that shell.

### Making Environment Variables Permanent

To make environment variables permanent, we need to add them to a shell configuration file. The exact file depends on which shell you're using (e.g. Bash, Zsh).

You would add the following line to your config file:

```bash
export MY_ENV_VAR="This is an environment variable"
```

To apply these changes without restarting your terminal, use the `source command`

> **Note**: The `source` command reads and executes commands from the file specified as its argument in the current shell environment. This is different than simply executing the file using `bash ~/.zshrc`, which would run the script in a new shell and not affect the current one. source runs it in the current shell so your changes take effect immediately.

### Important Environment Variables

-   `HOME`: Points to the home directory of the current user.
-   `USER`: Contains the username of the current user.
-   `SHELL`: Specifies the user's default shell.
-   `PWD`: Stands for "Print Working Directory". It contains the path of the current directory.
-   `TERM`: Specifies the type of terminal to emulate when running the shell.

## Getting Help on Commands

-   `type` command: displays the type of command the shell will execute
    -   `type cd` will return `cd is a shell builtin`
-   `--help` option: provides a quick overview of the command's usage
-   `man` command: provides more detailed information about commands, including their full documentation

    -   Use the Up and Down arrow keys to scroll line by line.
    -   Use the Space bar to move forward one page.
    -   Use the b key to move back one page.
    -   Use the / key followed by a word to search for that word in the document. For example, /sort will search for "sort".
    -   Press n to move to the next occurrence of your search term.
    -   Press N to move to the previous occurrence of your search term.

-   `apropos` command: helps you find commands related to a specific keyword

    If you want to narrow down the results, you can use grep to filter the output.

    ```bash
    apropos file | grep create
    ```

## Redirects

## Source

-   [What is Linux? - Linux.com](https://www.linux.com/what-is-linux/)
-   [Environment Variables in Linux - LabEx](https://labex.io/tutorials/linux-environment-variables-in-linux-385274)
-   [Get Help on Linux Commands - LabEx](https://labex.io/tutorials/linux-get-help-on-linux-commands-18000)
-   [How to use Linux redirection symbols - LabEx](https://labex.io/tutorials/linux-how-to-use-linux-redirection-symbols-437914)
