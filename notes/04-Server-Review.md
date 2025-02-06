# Server Review

Table of Contents

- [Server Review](#server-review)
  - [Overview](#overview)
  - [Uptime Load](#uptime-load)
    - [Using `uptime` Command](#using-uptime-command)
    - [Using `top` Command](#using-top-command)
  - [System Logs](#system-logs)
    - [Syslog Configuration](#syslog-configuration)
  - [Available Memory / Disk](#available-memory--disk)
    - [Monitoring Memory Usage with `free` command](#monitoring-memory-usage-with-free-command)
  - [Source](#source)

## Overview

The process of reviewing a server in Linux involves assessing the server’s performance, security, and configuration to identify areas of improvement or any potential issues. The scope of the review can include checking security enhancements, examining log files, reviewing user accounts, analyzing the server’s network configuration, and checking its software versions.

## Uptime Load

**Load Average** in Linux is a metric that is used by Linux users to keep track of system resources. It also helps you monitor how the system resources are engaged. To understand the Load Average in Linux, we need to know what do we define as **load**.

**In a Linux system, the load is a measure of CPU utilization at any given moment.** It refers to the number of processes which are either currently being executed by the CPU or are waiting for execution.

An idle system has a load of `0`. With each process that is being executed or is on the waitlist, the load increases by `1`.

On its own, the load doesn’t give any useful information to the user. The load can change in split seconds. This is because the number of processes using or waiting for the CPU time doesn’t remain constant. This is why we use **Load Average** in Linux to monitor resource usage.

### Using `uptime` Command

We can check the Load Average using `uptime` command, and the output looks like this:

```
17:34:53 up 7 min,  1 user,  load average: 0.29, 0.21, 0.08
```

While most people are used to the load percentages shown in Windows systems, Load Average in Linux is depicted as three different decimal values.

Going left to right:

- The first value depicts the average load on the CPU for the last minute.
- The second gives us the average load for the last 5-minute interval
- The third value gives us the 15-minute average load

While a load of `1` can mean approximately 100% resource usage on a single processor system, such systems are practically non-existent today. Unless you haven’t upgraded your system in over a decade, your system should run on a multi-core processor.

For a dual-core processor, a load of `1` means that 1 core was 100% idle. This translates to approximately 50% CPU usage. Similarly, it would represent 25% CPU usage for a quad-core processor.

Load Average in Linux takes into account the waiting threads and tasks along with processes being executed. Also, it is an average value instead of being an instantaneous value.

However, an approximate idea of resource usage can be determined by the ratio of Load Average over the number of cores of your processor. While it is not an exact value for the CPU utilization at any given time, it can be helpful for resource monitoring.

### Using `top` Command

Another way to monitor the Load Average on your system is to utilise the `top` command in Linux. This will open the top interface in your terminal. Unlike the uptime command, this gives an in-depth view of the resource usage for your system.

## System Logs

Linux logs are critical system event records that capture detailed information about system activities, software performance, and potential issues. These log files serve as essential diagnostic tools for system administrators and developers to monitor, troubleshoot, and maintain Linux systems.

Linux systems typically store log files in the `/var/log` directory. Different log files capture various system events:

| Log File           | Purpose                            |
| ------------------ | ---------------------------------- |
| `var/log/syslog`   | General system activities          |
| `var/log/auth.log` | Authentication and security events |
| `var/log/kern.log` | Linux kernel messages              |
| `var/log/messages` | System-wide message logs           |

Here's a bash script demonstrating basic log file inspection:

```bash
#!/bin/bash

echo "Recent System Logs:"
tail -n 10 /var/log/syslog

echo "Total Log Entries:"
wc -l /var/log/syslog

echo "SSH Authentication Attempts:"
grep "sshd" /var/log/auth.log | grep "Accepted" | tail -n 5
```

### Syslog Configuration

Linux uses syslog daemon to manage log generation and routing. Configuration files like `/etc/rsyslog.conf` define log handling rules, enabling systematic log management across different system components.

## Available Memory / Disk

Linux memory management is a fundamental aspect of operating system design and implementation. Understanding the concepts of physical memory, virtual memory, kernel memory, and user memory is crucial for efficient resource utilization and troubleshooting memory-related issues.

Types of memory:

| **Type**            | **Description**                                                                 |
| ------------------- | ------------------------------------------------------------------------------- |
| **Physical Memory** | Actual hardware memory (RAM) storing OS, apps, and active data.                 |
| **Virtual Memory**  | Combines RAM and disk (swap) to allow processes to use more memory.             |
| **Kernel Memory**   | Memory used by the Linux kernel for system operations, not accessible to users. |
| **User Memory**     | Memory for user applications, managed by the kernel's virtual memory system.    |
| **Swap Space**      | Disk space used as extra memory when RAM is full, managed by the kernel.        |

### Monitoring Memory Usage with `free` command

The `free` command is a simple and effective tool for monitoring the system's memory usage. It displays the total, used, and available physical and swap memory.

```bash
free -m
```

The `-m` option specifies that the output should be displayed in megabytes (MB).

Output example:

```

              total        used        free      shared  buff/cache   available
Mem:           7880        1234        5233         155        1412        6220
Swap:          2047           0        2047
```

## Source

- [What is Load Average in Linux? - DigitalOcean](https://www.digitalocean.com/community/tutorials/load-average-in-linux)
- [How to Analyze Linux System Logs - LabEx](https://labex.io/tutorials/linux-how-to-analyze-linux-system-logs-409907)
- [How to Check Linux memory with with free - LabEx](https://labex.io/tutorials/linux-how-to-check-linux-memory-with-free-421913)
