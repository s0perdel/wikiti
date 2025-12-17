---
tags:
---
## Create / Delete / Update

UID - User IDentifier.
UID (by default) depends from what type of user you are:

| UID Range  | Description                       |
| ---------- | --------------------------------- |
| 0          | Reserved for the root (superuser) |
| 1-999      | Reserved for system/service users |
| 1000-60000 | Normal user accounts              |

User data is stored:

| File          | Purpose                             |
| ------------- | ----------------------------------- |
| `/etc/passwd` | Stores user account info            |
| `/etc/shadow` | Stores user password and aging info |

`/etc/login.defs` contains the default setting that are applied when a user is created.

### Creating a new user

| Command                         | Description                                                                                                                         |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `useradd new_user`              | create an user named `new_user`                                                                                                     |
| `useradd -m new_user`           | create an user and a home directory for it (it's created by default, in `/etc/login.defs` the variable `CREATE_HOME` is set to yes) |
| `useradd -M new_user`           | create an user but not create home directory                                                                                        |
| `useradd -s /bin/bash new_user` | create an user and specify for it a login shell                                                                                     |
| `useradd -u 8888 new_user`      | create an user and specify for it a custom UID                                                                                      |
| `useradd -r sys_user`           | create a system user with no aging info                                                                                             |
| `useradd -G wheel new_user`     | create an user and give it `wheel` group                                                                                            |
| `passwd`                        | change your password                                                                                                                |
| `passwd new_user`               | change the `new_user`'s password                                                                                                    |
| `su new_user`                   | switch user to a new_user                                                                                                           |

Note: `wheel` is a group for sudoers in RHEL/CentOS/Fedora/Rocky, `sudo` is a group for sudoers in Debian/Ubuntu/Mint.

### Update

| Command                            | Description                                                         |
| ---------------------------------- | ------------------------------------------------------------------- |
| `usermod -L user`                  | lock user password, make impossible for it to login                 |
| `usermod -U user`                  | unlock user password                                                |
| `usermod -u 9999 user`             | change the UID of the user                                          |
| `usermod -g 8500 user`             | change the GID of the primary group of the user                     |
| `usermod -l cool_user user`        | change username to `cool_user`                                      |
| `usermod -d /home/cool_user user`  | change the home directory for the user                              |
| `usermod -md /home/cool_user user` | change the home directory for the user and move all the files there |

### Delete

| Command           | Description                               |
| ----------------- | ----------------------------------------- |
| `userdel user`    | delete user from the system               |
| `userdel -f user` | delete user even if it is online          |
| `userdel -r user` | delete user and remove its home directory |

## Users and Groups

GID - Group IDentifier.
All the groups are stored in `/etc/group`.

### Creating

| Command                    | Description                            |
| -------------------------- | -------------------------------------- |
| `groupadd hello`           | create a new group "hello"             |
| `groupadd -g 8888 hello`   | create a new group and specify a GID   |
| `groupadd -r sys`          | create a new system group              |
| `groupadd -U me,you hello` | create a new group and add users to it |

### Modifying

| Command                  | Description                                    |
| ------------------------ | ---------------------------------------------- |
| `groupmod -g 9999 hello` | change the GID of "hello" group to 9999        |
| `groupmod -n bye hello`  | change the name of group from "hello" to "bye" |
| `groupmod -u me hello`   | removing "you" from the group                  |
| `usermod -aG hello user` | add user to the group                          |

### Deleting

| Command             | Description              |
| ------------------- | ------------------------ |
| `groupdel hello`    | delete the "hello" group |
| `groupdel -f hello` | force delete             |

## Managing Permissions

You can check [[Working with Files#Files Permissions|here]].

## `umask` - The Permission "Substractor"

This command is a mask that removes permissions from new files/directories.
How it works:
- Files start as `rw-rw-rw-` (666)
- Directories start as `rwxrwxrwx` (777)
- `umask` substracts permissions from these defaults
The most common and default value is 002.
You can check and change the mask using the command:
- `umask` - check the current mask
- `umask 1234` - temporarily change the mask (until logout), first digit is SGID, SUID or Sticky Bit