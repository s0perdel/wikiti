## Files Permissions
### File ownership
User - it's the owner of the file, when you create a file - you become the owner.
Group - the primary group of the owner.
Other - consists of all the users on the system besides the owner.

### Understanding file permissions and ownership
If we use `ls -l` we will see something like this:
```
-rwxrw-r-- 1 abhi itsfoss 457 Aug 10 11:55 agatha.txt
```
![[ls_output.png]]

| Part                   | Definition                                                                              |
| ---------------------- | --------------------------------------------------------------------------------------- |
| File type              | `d` means directory, `-` means regular file, `l` means a symbolic link (next #1 header) |
| Permissions            | the permissions set on a file                                                           |
| Hard link count        | shows if the file has hard links, default count is one (next #1 header)                 |
| User                   | the user who owns the file                                                              |
| Group                  | the group that owns the file (usually primary group of the owner)                       |
| File size              | size of file in bytes                                                                   |
| Modification timestamp | the date and time the file was last modified                                            |
| Filename               | the name of the file or directory                                                       |
Permissions explained:
![[permission_explained.png]]
(Execution for a folder means that you can enter that folder)

### Change file permissions
Command `chmod` has two different ways to change, absolute mode and symbolic mode.

Absolute mode uses numbers for a represented permission:
- r (read) = 4
- w (write) = 2
- x (execute) = 1
- - (no permission) = 0
So to combine here is the cheatsheet:

| Number         | Permission |
| -------------- | ---------- |
| 0              | ---        |
| 1              | --x        |
| 2              | -w-        |
| 3 (i.e. 2+1)   | -wx        |
| 4              | r--        |
| 5 (i.e. 4+1)   | r-x        |
| 6 (i.e. 4+2)   | rw-        |
| 7 (i.e. 4+2+1) | rwx        |
`chmod 764 file.txt` for example to set for `file.txt` the following permissions `rwxrw-r--`

In symbolic mode, owners are denoted with the following symbols:
- u = user owner
- g = group owner
- o = other
- a = all (user + group + other)
And it uses mathematical operators to perform the permission changes:
- + for adding permissions
- - for removing permissions
- = for overriding existing permissions with a new value
Example:
We have a file with `test.txt` with the next permissions: `rw-rw-r--`
If we use this command:
```BASH
chmod u-w,g-r,o+wx test.txt
```
Our file will have now these permissions: `r---w-rwx`
### Change file ownership
Here showing of examples will be enough:
```BASH
chown new_user file.txt
chown new_user:new_group file.txt
chown :new_group file.txt
chgrp new_group file.txt
```
### Special File Permissions: SUID, SGID and Sticky Bit
![[special_permissions.png]]

| Term                | Type of permission | Apply only on                   | Effect                                                                               |
| ------------------- | ------------------ | ------------------------------- | ------------------------------------------------------------------------------------ |
| SUID (Set User ID)  | User               | executable files                | file executes with the owner's privileges                                            |
| SGID (Set Group ID) | Group              | executable files or directories | file executes with group's privileges OR file in directory inherit directory's group |
| Sticky Bit          | Other              | directories                     | only file owner can delete/rename in directory                                       |

Setting and removing special permissions:

| Command                 | Result                            |
| ----------------------- | --------------------------------- |
| `chmod 4755 file.sh`    | SUID + 755                        |
| `chmod 2755 file.sh`    | SGID + 755                        |
| `chmod 1777 /folder`    | Sticky Bit + 755                  |
| `chmod 0755 file.sh`    | Remove special permissions + 755  |
| `chmod u+s file.sh`     | Previous permissions + SUID       |
| `chmod g+s file.sh`     | Previous permissions + SGID       |
| `chmod o+t /folder`     | Previous permissions + Sticky Bit |
| `chmod u-s,g-s file.sh` | Removing SUID and SGID            |
| `chmod o-t /folder`     | Removing Sticky Bit               |
## Archiving and Compressing
### Archiving/Packing
That means just transforming a directory to a simple file for easier manipulation or sharing.
For this we use command `tar`
Example:
`tar -cvf archive.tar folder`
- `-c` this flag tells tar to create a new archive
- `-v` this turns on the verbose mode
- `-f` this option is followed by the name of the archive file we want to create
- `archive.tar` the name of our archive file
- `folder` our directory that we want to pack
To view the contents of the archive without extracting it, we use:
`tar -tvf archive.tar`
- `-t` this flag shows the contents of the archive
To extract the contents from the archive to a certain folder, we use:
`tar -xvf archive.tar -C extracted_folder`
- `-x` this flag tells tar to extract files from an archive
- `-C` this flag tells tar to change the directory before extracting
### Compressing
That means compressing the archive/file to reduce the size and save space on the disk.
Here are the most popular tools:

a) **Gzip (.gz) - the speedy & common one**
- *Compression Speed*: fast
- *Compression Ratio*: good (the weakest of the four, but still)
- *CPU Usage*: low
- *When to Use*: by default for most things, very common for software downloads
To use with `tar` you need to put flag `-z` that tells `tar` to filter the archive through `gzip`
Without `tar` we use `gzip`/`gunzip`
```BASH
tar -czvf archive.tar.gz folder # or .tgz instead of .tar.gz
tar -xzvf archive.tgz -C extract_folder
gzip file # compress to file.gz
gunzip file.gz # decompress to file
```

b) **Bzip2 (.bz2) - the balanced one**
- *Compression Speed*: slower than gzip
- *Compression Ratio*: better than gzip
- *CPU Usage*: higher than gzip
- *When to Use*: when you want better compression and are willing to wait a bit longer, common for distributing source code
To use with `tar` you need to put flag `-j` that tells tar to filter archive through `bzip2`
Without `tar` we use `bzip2`/`bunzip2`
```BASH
tar -cjvf archive.tar.bz2 folder # or .tbz2 instead of .tar.bz2
tar -xjvf archive.tbz2 -C extract_folder
bzip2 file # compress to file.bz2
bunzip2 file.bz2 # decompress to file
```

c) **XZ (.xz) - the space saver**
- *Compression Speed*: slowest
- *Compression Ratio*: best
- *CPU Usage*: highest
- *When to Use*: when you need the smallest possible file size and don't care about the time or CPU cost; used for distributing Linux kernel images and large, static files.
To use with `tar` you need to put flag `-J` (capital J) that tells tar to filter archive through `xz`
Without `tar` we use `xz`/`unxz`
```BASH
tar -cJvf archive.tar.xz folder # or .txz instead of .tar.xz
tar -xJvf archive.txz -C extract_folder
xz file # compress to file.xz
unxz file.xz # decompress to file
```

d) **Zip (.zip): the all-in-one**
`zip` is different, it both archives and compresses in one tool.
It's less efficient on Linux but is very common because it's the standard on Windows.
*When to Use*:
- when you need to share files with Windows users
- when you need to create an archive that can be easily explored without decompressing the whole thing
```BASH
zip compressed.zip file
zip -r compressed.zip folder
unzip compressed.zip
```

## Soft Links / Hard Links
A file link is a reference to the actual file data stored in the file system.
When you create a file, the operating system assigns a unique identifier called an *inode* to that file. The *inode* contains information about the file, such as its permissions, ownership, and the physical location of the file data on the storage device.
File link come in two main types:
- **Hard Links**: it's a direct reference to the *inode* of the file; you're creating an additional name for the same file data, without duplicating the actual file content.
- **Symbolic (Soft) Links**: it's a special type of file that contains a reference to another file or directory; symlinks store the path to the target file or directory, not to the inode.
Using:
```BASH
ln file.txt hard_link.txt
ln -s file.txt soft_link.txt
```
Comparison and Use Cases:

| Feature                | Hard Links                          | Symbolic Links                                      |
| ---------------------- | ----------------------------------- | --------------------------------------------------- |
| Storage Utilization    | No additional space required        | Additional storage space required for the link file |
| File System Boundaries | Cannot cross file system boundaries | Can cross file system boundaries                    |
| Broken Links           | Not possible                        | Possible if the target file is deleted or moved     |
| Inode Reference        | Direct reference to the inode       | Reference to the path of the target file            |

Hard links are generally used for:
- Organizing files in different locations without duplicating the content
- Backup and restoration processes, where hard links are preserved to maintain the original file structure
Symbolic links are commonly used for:
- Creating shortcuts or aliases to frequently accessed files or directories
- Maintaining compatibility between different versions of software or libraries
- Bridging the gap between file system boundaries