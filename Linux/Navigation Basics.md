## Basic Commands
---

| Command                | Action                                                                     |
| ---------------------- | -------------------------------------------------------------------------- |
| `pwd`                  | show where you are currently                                               |
| `cd directory`         | move to directory                                                          |
| `.`                    | current directory                                                          |
| `..`                   | parent directory                                                           |
| `ls`                   | show all the files in the current directory                                |
| `ls -l`                | show detailed info about files                                             |
| `ls -a`                | show everything including hidden files                                     |
| `ls -h`                | human-readable format of file size                                         |
| `ls -i`                | show the [[Working with Files#Soft Links / Hard Links\|inode]] of the file |
| `echo "Hello, world!"` | `python3 << print('Hello, world!')`                                        |
| `whoami`               | display the username of the current logged user                            |


## Moving Files / Directories
---

| Command                             | Action                                          |
| ----------------------------------- | ----------------------------------------------- |
| `mv file.txt /folder/`              | basic move or rename                            |
| `mv -i file.txt /folder/`           | prompt before overwriting existing files        |
| `mv -u file.txt /folder/`           | move only if the source file is newer           |
| `mv -v file.txt /folder/`           | verbose mode (shows the details of moving)      |
| `mv -f file.txt /folder/`           | force moving without confirmation               |
| `mv --backup file.txt /folder/`     | create a backup of a destination file if exists |
| `mv --no-clobber file.txt /folder/` | do not overwrite any existing files             |
| `mv /folder/ /path/`                | moving a directory with the contents            |
| `mv file1.txt file2.txt`            | rename a file                                   |
## Creating & Deleting Files / Dirs
---

| Command                                 | Action                                                  |
| --------------------------------------- | ------------------------------------------------------- |
| `touch file.txt`                        | create an empty file                                    |
| `echo 123 > 123.txt`                    | output to a existing or creating a new file             |
| `cat > file.txt`                        | interactive input directly to a new file                |
| `cp example.txt copy.txt`               | copy and making a new same file                         |
| `cp -r /folder /backup`                 | copy and making a new same folder                       |
| `mkdir /folder`                         | make an empty folder                                    |
| `mkdir -p /parent/son`                  | make a nested folder                                    |
| `mkdir -p /parent/{son,daughter}`       | make a nested folder with more than one son             |
| `rmdir /folder`                         | deleting **only an empty** folder                       |
| `rm file.txt`                           | delete a file                                           |
| `rm -v file.txt`                        | delete a file (verbose mode)                            |
| `rm -i file.txt`                        | prompt before deleting                                  |
| `rm -r /folder`                         | recursive delete, to remove the folder and all contents |
| `rm -f file.txt` OR<br>`rm -rf /folder` | force delete                                            |
## Directory Hierarch Overview
---
![[linux_directories.png]]

| Directory    | Overview                                                                                                                                                                                                                                                                          |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/`          | *the root:* directory, where everything is stored in Linux                                                                                                                                                                                                                        |
| `bin/`       | *binaries:* executables of many basic shell commands, mostly of programs are in binary format and are accessible for all the users in Linux                                                                                                                                       |
| `dev/`       | *device files:* virtual special files often related to the device<br>interesting examples:<br>`/dev/null` - can be sent any file or string there to destroy<br>`/dev/zero` - contains an infinite sequence of 0<br>`/dev/random` - contains an infinite sequence of random values |
| `etc/`       | *configuration files:* contains the core configuration files of the system, use primarily by administrator or services                                                                                                                                                            |
| `usr/`       | *user binaries and program data:* all executable files, libraries and source of most of the system programs, for this reason most of them are reandonly                                                                                                                           |
| `usr/bin/`   | contains basic user commands                                                                                                                                                                                                                                                      |
| `usr/sbin/`  | contains additional commands for the administrator                                                                                                                                                                                                                                |
| `usr/lib/`   | contains the system libraries                                                                                                                                                                                                                                                     |
| `usr/share/` | contains documentation or common to all libraries                                                                                                                                                                                                                                 |
| `home/`      | *user personal data:* contains personal directories for the users                                                                                                                                                                                                                 |
| `lib/`       | *shared libraries:* codes that can be used by the executable binaries                                                                                                                                                                                                             |
| `sbin/`      | *system binaries:* `bin/` but for administrator                                                                                                                                                                                                                                   |
| `tmp/`       | *temporary files:* directory that holds temporary files, they are deleted every restart of the system                                                                                                                                                                             |
| `var/`       | *variable data files:* where programs store runtime information like system logging, user tracking, caches, and other files that system programs create and manage                                                                                                                |
| `boot/`      | *boot files:* the files of the kernel and boot image                                                                                                                                                                                                                              |
| `proc/`      | *process and kernel files:* contains the information about currently running processes and kernel parameters                                                                                                                                                                      |
| `opt/`       | *optional software:* used for installing/storing the files of third-party applications that are not available from the distribution's repository                                                                                                                                  |
| `root/`      | *the home directory of the root:* home directory for the root user                                                                                                                                                                                                                |
| `media/`     | *mount point for removable media*: usb, sd, cd, dvd etc                                                                                                                                                                                                                           |
| `mnt/`       | *mount directory*: user by system administrators to manually mount a filesystem                                                                                                                                                                                                   |
| `srv/`       | *service data:* data for services provided by the system                                                                                                                                                                                                                          |
