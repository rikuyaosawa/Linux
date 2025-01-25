# Working with Files

Table of Contents

- [Working with Files](#working-with-files)
  - [File Permissions](#file-permissions)
    - [Viewing the Permissions](#viewing-the-permissions)
    - [Changing File Permissions](#changing-file-permissions)
    - [Changing File Ownership](#changing-file-ownership)
    - [Permission Management Best Practices](#permission-management-best-practices)
  - [Archiving and Compressing](#archiving-and-compressing)
  - [Source](#source)

## File Permissions

Linux file permissions are a fundamental concept in understanding and managing file access control. In Linux, every file and directory has a set of **permissions** that determine who can read, write, and execute the file or directory.

The basic file permissions in Linux are:

- **Read (r)**: Allows the user to view the contents of the file.
- **Write (w)**: Allows the user to modify the contents of the file.
- **Execute (x)**: Allows the user to run the file as a program or script.

These permissions can be set for three different types of users:

- **Owner**: The user who created the file or directory.
- **Group**: The group that the file or directory belongs to.
- **Others**: Any user who is not the owner or part of the group.

### Viewing the Permissions

`ls -l`: display the file permissions in the following format:

```
-rwxrw-r-- 1 user group 1024 Aug 10 11:55 agatha.txt
```

Let's break down the output:

![](https://linuxhandbook.com/content/images/2020/06/file-permission-explanation-1-1.png)

- **File type**: Denotes the type of file. `d` means directory, `–` means regular file, `l` means a symbolic link.
- **Permissions**: This field shows the permission set on a file.
- **Hard link count**: Shows if the file has hard links. Default count is one.
- **User**: The user who owns the files.
- **Group**: The group that has access to this file. Only one group can be the owner of a file at a time.
- **File size**: Size of the file in bytes.
- **Modification time**: The date and time the file was last modified.
- **Filename**: Obviously, the name of the file or directory.

A close look at permissions:

![Linux permission break down](https://linuxhandbook.com/content/images/2020/06/file-permission-explanation-2-1.png)

> NOTE: Root user has super powers and normally, it has read, write and execute permissions to all the files, even if you don’t see it in file permissions.

A single user may be the member of several groups but only the primary group of the user is the group owner of a file created by the user. The primary group of a user can be found using the id command like id `-gn <username>`. Leave the username blank if you are trying to find your own primary group.

### Changing File Permissions

`chmod` command: changes the access permissions of a file or directory.

Permissions used to be called _mode of access_ and hence chmod was the short form of change the mode of access.

There are two ways to use the chmod command:

- Absolute mode
- Symbolic mode

#### Absolute mode

In the absolute mode, permissions are represented in numeric form (octal system to be precise). In this system, each file permission is represented by a number.

- r (read) = 4
- w (write) = 2
- x (execute) = 1
- – (no permission) = 0

With these numeric values, you can combine them and thus one number can be used to represent the entire permission set.

![Linux permission with absolute mode](https://img1.wsimg.com/isteam/ip/9b62f62a-33f8-45a3-8735-d1ab2a2e0742/Absolute.png/:/cr=t:0%25,l:0%25,w:100%25,h:100%25/rs=w:600,cg:true)

Example:

```bash
# read and write permissions for all
chmod 666 <file name>
```

#### Symbolic mode

In symbolic mode, owners are denoted with the following symbols:

- `u` = user owner
- `g` = group owner
- `o` = other
- `a` = all (user + group + other)

The symbolic mode uses mathematical operators to perform the permission changes:

- `+` for adding permissions
- `–` for removing permissions
- `=` for overriding existing permissions with new value

Example:

```bash
# grant execute permission for group owner
chmod g+x <file name>
```

```bash
# combining multiple permission changes
# this command removes the read and write permission and add execute permissions for Other
chmod o-rw+x,u+x <file name>
```

### Changing File Ownership

`chown` command changes user and group ownership of files and directories.

```bash
chown <new_user_name> <filename>
```

If you want to change the user as well as group, you can use chown command like this:

```bash
chown <new_user_name>:<new_user_group> <filename>
```

or use `chgrp` command specifically used for changing group owner of a file or directory.

```bash
chgrp <new_user_group> <filename>
```

<br />

> NOTE: Two groups cannot own the same file.

### Permission Management Best Practices

Properly managing file permissions is crucial for maintaining the security and integrity of your Linux system. Below are best practices and techniques for effective Linux permission management:

1. Understand File Ownership and Permissions

   - Every file and directory in Linux has an **owner** and a set of **permissions** that control access.

2. Use the Principle of Least Privilege

   - Follow the **principle of least privilege** when granting permissions.
   - Ensure users and processes are only granted the **minimum permissions** required to perform their tasks.
   - Avoid giving unnecessary permissions to reduce the risk of unauthorized access or misuse.

3. Regularly Review and Audit Permissions

   - Periodically review the permissions on your files and directories to ensure they are appropriate.
   - Use tools like `find` and `ls -l` to list and inspect permissions on your file system.

4. Utilize Group-Based Permissions

   - Use **groups** instead of granting permissions directly to individual users.
   - Assign users to appropriate groups and set permissions at the group level for more efficient management.

5. Leverage Symbolic Links

   - Use **symbolic links (symlinks)** to provide alternative access paths to files and directories.
   - This allows you to grant access without modifying the original permissions.

6. Automate Permission Management

   - For large or complex file systems, automate permission management with scripts or tools like **Ansible** or **Puppet**.
   - Automation ensures consistent and reliable permission management across your infrastructure.

7. Document and Communicate Permissions

   - Maintain clear documentation on the **purpose** and **expected permissions** for your files and directories.
   - This helps your team understand and manage the file system more effectively.

## Archiving and Compressing

## Source

- [Manage Linux File Permissions Effectively - LabEx](https://labex.io/tutorials/linux-manage-linux-file-permissions-effectively-420758)
- [Linux File Permissions - Linux Handbook](https://linuxhandbook.com/linux-file-permissions/)
- []()
- []()
