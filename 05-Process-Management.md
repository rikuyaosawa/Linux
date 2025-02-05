# Process Management

Table of Contents

- [Process Management](#process-management)
  - [Overview](#overview)
  - [Program vs. Processes](#program-vs-processes)
  - [Process Initialization on Linux](#process-initialization-on-linux)
    - [Kernel Space and User Space](#kernel-space-and-user-space)
  - [Process Creation using Fork and Exec](#process-creation-using-fork-and-exec)
  - [Identifying running processes on Linux](#identifying-running-processes-on-linux)
  - [Background and foreground processes](#background-and-foreground-processes)
    - [Jobs](#jobs)
    - [Using the `bg` and `fg` commands](#using-the-bg-and-fg-commands)
  - [Interacting with processes using signals](#interacting-with-processes-using-signals)
    - [Signals categories explained](#signals-categories-explained)
  - [Signals and Processes States](#signals-and-processes-states)
  - [Manipulating process with `pgrep` and `pkill`](#manipulating-process-with-pgrep-and-pkill)
  - [Adjusting process priority using nice and renice](#adjusting-process-priority-using-nice-and-renice)
  - [Source](#source)

## Overview

**Processes** are at the center of the Linux operating system: created by the Kernel itself, **they represent running operations currently happening on your Linux host.**

Processes are everywhere, they may run in the background or you may choose to initialize them by yourself for custom operations.

You may choose to start them, to interrupt them, to resume them or to stop them.

## Program vs. Processes

In short, **processes** are running programs on your Linux host that perform operations such as writing to a disk, writing to a file, or running a web server for example.

Process have a owner and they are identified by a **process ID (also called PID)**.

On the other hand, programs are lines or code or lines of machine instructions stored on a persistent data storage. They can just sit on your data storage, or they can be in execution, i.e running as processes.

![processes vs. program](https://devconnected.com/wp-content/uploads/2019/09/program-process.png)

In order to perform the operations they are assigned to, processes need **resources: CPU time, memory** (such as RAM or disk space), but also virtual memory such as swap space in case your process gets too greedy.

## Process Initialization on Linux

By default, when you boot a Linux system, your Linux kernel is loaded into memory, it is given a virtual filesystem in the RAM (also called **initramfs**) and the initial commands are executed.

One of those commands starts the very first process on Linux.

Historically, this process was called the **init** process but it got replaced by the **systemd initialization process** on many recent Linux distributions.

To prove it, run the following command on your host

```bash
ps -aux | head -n 2
```

Output:

```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.7  0.1 165812 10936 ?        Ss   19:34   0:20 /sbin/init
```

As you can see, the systemd process has a PID of 1.

If you were to print all processes on your system, using a tree display (`pstree` command), you would find that all processes are children of the systemd one.

### Kernel Space and User Space

It is noteworthy to underline the fact that all those initialization steps (except for the launch of the initial process) are done in a reserved space called **the kernel space**.

The kernel space is a space reserved to the Kernel in order for it to run essential system tools properly and to make sure that your entire host is running in a consistent way.

On the other hand, **user space** is reserved for processes launched by the user and managed by the kernel itself.

![kernel space and user space](https://devconnected.com/wp-content/uploads/2019/09/user-kernel-space.png)

## Process Creation using Fork and Exec

When you are creating and running a program on Linux, it generally involves two main steps : fork and execute.

- **Fork Operation** - a clone operation, it takes the current process, also called the parent process, and it clones it in a new process with a brand new process ID. When forking, everything is copied from the parent process : the stack, the heap, but also the file descriptors meaning the standard input, the standard output and the standard error.
- **Execute Operation** - used on Linux to replace the current process image with the image from another process.

## Identifying running processes on Linux

By default, `ps` command will show you the list of the current running processes owned by the current user.

To display the processes that are owned and executed by the current connected user, run the following command:

```bash
ps u
```

> [!NOTE]
> There are plenty of different options for the `ps` command, and they can be seen by running the manual command: `man ps`.

## Background and foreground processes

**A background process** on Linux is a process that runs in the background, meaning that it is not actively managed by a user through a shell for example.

On the opposite side, **a foreground process** is a process that can be interacted with via direct user input.

To execute a process in the background, simply put a `&` sign at the end of your command:

```bash
# delay 1000 seconds in the background
sleep 1000 &
```

### Jobs

**Jobs** are a list of processes that were started in the context of the current shell and that may still be running in the background.

`jobs` command displays the information about processes spawned by the current shell.

Example output:

```
[1]  + running    sleep 1000
```

The different columns from left to right represent **the job ID**, **the process state**, and **the command executed**.

### Using the `bg` and `fg` commands

In order to interact with jobs, you have two commands available : `bg` and `fg`.

The `bg` command is used on Linux in order to send a process to the background and the syntax is as follows:

```bash
bg %<job_id>
```

Similarly, in order to send a process to the foreground, you can use the `fg` in the same fashion:

```bash
fg %<job_id>
```

## Interacting with processes using signals

On Linux, **signals** are a form of **interprocess communication (also called IPC)** that creates and sends asynchronous notifications to running processes about the occurrence of a specific event.

Signals are often used in order to send a **kill** or a termination command to a process in order to shut it down (also called kill signal).

In order to send a signal to a process, you have to use the `kill` command.

```bash
kill -<signal number> <pid>|<process_name>
```

For example, in order to force a HTTPD process (PID = 123) to terminate (without a clean shutdown), you would run the following command

```bash
kill -9 123
```

### Signals categories explained

As explained, there are many signals that one can send in order to notify a specific process.

- **SIGINT**: short for signal interrupt is a signal used in order to interrupt a running process. It is also the signal that is being sent when a user pressed Ctrl + C on a terminal;

- **SIGHUP**: short for signal hangup is the signal sent by your terminal when it is closed. Similarly to a SIGINT, the process terminates;

- **SIGKILL**: signal used in order to force a process to stop whether it can be gracefully stopped or not. This signal can not be ignored except for the init process (or the systemd one on recent distributions);

- **SIGQUIT**: specific signal sent when a user wants to quit or to exit the current process. It can be invoked by pressing Ctrl + D and it is often used in terminal shells or in SSH sessions;

- **SIGUSR1, SIGUSR2**: those signals are used purely for communication purposes and they can be used in programs in order to implement custom handlers;

- **SIGSTOP**: instructs the process to stop its execution without terminating the process. The process is then waiting to be continued or to be killed completely;

- **SIGCONT**: if the process is marked as stopped, it instructs the process to start its execution again.

In order to see the full list of all signals available, you can run the following command:

```bash
kill -L
```

> [!NOTE]
> Built-in shell command might not have `-L` option available. \
> Use `/bin/kill -L` instead.

## Signals and Processes States

Processes have many different states, they can be :

- **Running** : processes running are the ones using some computational power (such as CPU time) in the current time. A process can also be called “runnable” if all running conditions are met, and it is waiting for some CPU time by the CPU scheduler.
- **Stopped** : a signal is stopped is linked to the `SIGSTOP` signal or to the `Ctrl + Z` keyboard shortcut. The process execution is suspended and it is either waiting for a `SIGCONT` or for a `SIGKILL`.
- **Sleeping** : a sleeping process is a process waiting for some event or for a resource (like a disk) to be available.

![Processes Steates](https://devconnected.com/wp-content/uploads/2019/09/process-states.png)

## Manipulating process with `pgrep` and `pkill`

The `pgrep` command is a shortcut for using the `ps` command piped with the `grep` command.

Syntax:

```bash
pgrep <options> <pattern>
```

On the other hand, the `pkill` command is also a shortcut for the `ps` command used with the `kill` command.

Syntax:

```bash
pkill <options> <pattern>
```

## Adjusting process priority using nice and renice

On Linux, not all processes are given the same priority when it comes to the CPU time.

Some processes, such as very important processes run by root, are given a higher priority in order for the operating system to work on tasks that truly matter to the system.

**Process priority on Linux is called the nice level.**

The nice level is a priority scale going from -20 to 19. The lower you go on the niceness scale, the higher the priority will be. Similarly, the higher you are on the niceness scale, the lower your priority will be.

![Niceness scale on Linux](https://devconnected.com/wp-content/uploads/2019/09/nice.png)

In order to start a certain program or process with a given nice level, you will run the following command:

```bash
nice -n <level> <command>
```

Similarly, you can use the renice command in order to set the nice level of a running process to a given value:

```bash
renice -n <priority> <pid>
```

> [!NOTE]
> As a non-root (or sudo) user, you won’t be able to set a nice level lower than the default assigned one (which is zero), and you won’t be able to renice a running process to a lower level than the current one.

## Source

[Understanding Processes on Linux - devconnected](https://devconnected.com/understanding-processes-on-linux/)
