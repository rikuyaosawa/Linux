# Working with Files

- [Working with Files](#working-with-files)
  - [File Permissions](#file-permissions)
    - [Viewing the Permissions](#viewing-the-permissions)
    - [Changing File Permissions](#changing-file-permissions)
    - [Changing File Ownership](#changing-file-ownership)
    - [Permission Management Best Practices](#permission-management-best-practices)
  - [Archiving and Compressing](#archiving-and-compressing)
    - [Common Tar Files](#common-tar-files)
    - [Common Tar Commands](#common-tar-commands)
    - [Compressing Files with `gzip`](#compressing-files-with-gzip)
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

**Tar** is one of the most common tool used for archiving files in Linux.

What is tar? Tar stands for “tape archive” and refers to a practice from the earlier days of computing when data was backed up to tapes. Despite the nostalgic origin of the name, tar is very powerful and uses modern technologies to archive and compress files.

- **Archiving** – The act of storing multiple files as one file.
- **Compression** – The act of shrinking a larger file or files.

Tar is an archiving tool. It creates a single file out of multiple files. **This saves network bandwidth, time and processing power while transferring the files**. A single file of 100 MB takes a lot less than transferring 100 files of 1 MB because of the file overhead.

This is why you’ll often find software available in a ‘_tarball_‘. Tarball is the common term used for a tar file.

While tar itself cannot compress files, you can use one of the common compression algorithms to compress the files while creating a tarball.

### Common Tar Files

- `.Tar`: This is a tarball file. It is only an archive and no compression is performed.

- `.Tar.Gz` or `.tgz`: This is the extension of an archive that has been compressed with **Gzip**.

- `.Tar.Bz2` or `.tbz`: This is the extension of an archive that has been compressed with **Bz2**. This is a relatively new technology. It features a higher ratio of compression, but that increased shrinking power means it takes a bit longer to complete.

- `.Tar.xz` or `.txz`, etc.: Tar also features built in support for `xz`, `lzip`, and more. These tools primarily use the same compression algorithm, LZMA. The popular 7z that has become fairly common in Windows environments also uses this algorithm. Further differences in the files result from structure and metadata.

### Common Tar Commands

#### Basic Options

- `-c` : **Create** a new tar archive.
- `-x` : **Extract** files from a tar archive.
- `-t` : **List** the contents of a tar archive without extracting.

#### File Specification

- `-f` : Specify the **archive file name** (used with `-c`, `-x`, or `-t`). \
  Example: `tar cf archive.tar file.txt`.

#### Compression

- `-z` : Compress or decompress using **gzip**. \
  Example: `tar czf archive.tar.gz folder/`.
- `-j` : Compress or decompress using **bzip2**. \
  Example: `tar cjf archive.tar.bz2 folder/`.
- `-J` : Compress or decompress using **xz**. \
  Example: `tar cJf archive.tar.xz folder/`.

#### Verbose Output

- `-v` : Show detailed information (verbose) during the operation. \
  Example: `tar cvf archive.tar file.txt`.

#### File Operations

- `-C` : Change to a directory before performing an operation. \
  Example: `tar xvf archive.tar -C /target/directory`.
- `--exclude=<pattern>` : Exclude files matching a pattern from the archive. \
  Example: `tar czf archive.tar.gz folder/ --exclude=*.log`.

#### File Integrity and Comparison

- `--diff` or `-d` : Compare the archive contents with the file system.
- `--verify` : Verify the archive after writing it.

#### Others

- `--wildcards` : Use wildcards to match file patterns in the archive.
  Example: `tar xvf archive.tar --wildcards "*.txt"`.
- `--append` or `-r` : Append files to an existing tar archive.
- `--delete` : Remove files from an archive (only works with uncompressed `.tar` files).

<br />

> ※ **Pay attention while using hyphen – with tar options** \
> Usually, when you use options with a Linux command, you add hyphen (-) before the options. The hyphen before options is not mandatory and is best avoided. This is why I haven’t used it in the examples.
>
> If you use hyphen before the options, you should always keep the f at the end of the options. If you use tar `-cvfz`, the `z` becomes an argument for option `z`. And then you’ll see an error like this:
>
> `tar: doc.tar.gz: Cannot stat: No such file or directory`
>
> This is why it is a good practice to use the option `f` at the end of all other options so that even if you use hyphen out of habit, it won’t create a problem.

### Compressing Files with `gzip`

If you combine `tar` with `gzip`, the tar command will create one single archive file from the folder and then gzip will compress this archive file.

```bash
gzip <archived file (e.g. tar)>
```

The compressed file will have the extention `.gz`.

To see the size of the compressed file, you can use the following command:

```bash
ls -lh <compressed file>.tar.gz
```

The `-lh` options will show the file size in a human-readable format (like KB, MB, etc.).

The good thing is that you can do both of these steps in one single command by using the z option. The command looks something like this:

```bash
tar -zcvf output_file_name directory_to_compress
```

**It’s important to provide the filename in the command otherwise you’ll see error.**

<small>Note: While `tar` and `gzip` are common in Linux and Unix-like systems, the `zip` format is often used for better compatibility with Windows systems.</small>

## Source

- [Manage Linux File Permissions Effectively - LabEx](https://labex.io/tutorials/linux-manage-linux-file-permissions-effectively-420758)
- [Linux File Permissions - Linux Handbook](https://linuxhandbook.com/linux-file-permissions/)
- [Basic Tar Commands - Linux Handbook](https://linuxhandbook.com/basic-tar-commands/)
- [Gzip Directory - Linux Handbook](https://linuxhandbook.com/gzip-directory/)
- [Linux File Packaging and Compression - LabEx](https://labex.io/tutorials/linux-file-packaging-and-compression-385413)
