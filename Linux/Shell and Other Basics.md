## Command Path

### Definition:
`$PATH` - it's a variable that contains paths to programs and scripts. This thing allows you to run scripts and programs from anywhere in your system.
To check the path variable: `echo $PATH`
To check where is source of any script or command: `which command`
### Add Directory to Path Variable:
#### Temporarily:
`export PATH="bin/myscripts:$PATH"` - we extend the variable `$PATH` with a new path
It will affect only current session (until we close the terminal)
#### Permanently:
We need to edit `~/.bashrc`
`.bashrc` - is a script file that runs every time you start a new interactive terminal session. It stands for "**Bash** **R**un **C**ommands".

`nano ~/.bashrc` and in the end of the file we add:
`export PATH="/bin/myscripts:$PATH"`
We save the changes and exit the file.
To make changes affect the current session we run:
`source ~/.bashrc` - this command tells the shell to execute the commands in a given file inside the current shell session.
### Remove Directory from $PATH
Either editing `~/.bashrc` or if system wide variables `/etc/environment`

## Environment Variables

Variables have the following format:
```
KEY=value
KEY="Some other value"
KEY=value1:value2
```
- The names of the variables are case-sensitive. By convention, environment variables should have UPPER CASE names.
- When assigning multiple values to the variable they must be separated by the colon `:` character.
- There is no space around the equals `=` symbol

**Environment variables** are variables that are available system-wide and are inherited by all spawned child processes and shells.
**Shell variables** are variables that apply only to the current shell instance. Each shell such as `zsh` and `bash`, has its own set of internal shell variables.

Several commands that allow you to list and set variables in Linux:

| Command              | Description                                                                                                                                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `env`                | run another program in a custom environment, without modifying the current one; without an argument it will print a list of the current environment variables. |
| `printenv HOME PATH` | print all or the specified environment variables                                                                                                               |
| `set`                | set or unset shell variables                                                                                                                                   |
| `unset`              | delete shell and environment variables                                                                                                                         |
| `export`             | set environment variables                                                                                                                                      |
To make environment variables persistent you need to define those variables in the bash configuration files.

| File/Directory    | Description                                                                                                                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/etc/profile`    | system-wide configuration script that sets up the environment for every user on the system, it is executed only for login shells                                                                  |
| `/etc/profile.d/` | a directory for modular system-wide scripts, usually `/etc/profile` contains a loop that sources (runs) every script in `/etc/profile.d/`                                                         |
| `~/.bash_profile` | user-specific login shell config, `/etc/profile` but for every user                                                                                                                               |
| `~/.bashrc`       | user-specific interactive shell config, it's like `~/.bash_profile` but is sourced every time you open a new terminal tab or window, or start a sub-shell; usually sourced from `~/.bash_profile` |

```
# When you login:
/etc/profile          # System-wide settings first
    ↓
/etc/profile.d/*.sh  # System modules
    ↓  
~/.bash_profile      # Then user-specific settings
    ↓
~/.bashrc            # Usually sourced from .bash_profile
```
Also [[Default Environmental Variables]]
## Command Help
Linux command help provides documentation and usage information for shell commands.

| Command          | Description                                            |
| ---------------- | ------------------------------------------------------ |
| `man command`    | for detailed manuals                                   |
| `help command`   | for shell built-ins                                    |
| `command --help` | for quick options                                      |
| `tldr command`   | for practical examples (it's not installed by default) |
## Redirects
The way of managing input and output streams of a command or program.
Every process typically has at least 3 streams opened:

|      Name       | System Name | ID  |             Definition              |      Default      |          Operator           |               Example               |
| :-------------: | :---------: | :-: | :---------------------------------: | :---------------: | :-------------------------: | :---------------------------------: |
| Standard Input  |    stdin    |  0  |      the source of input data       |     Keyboard      |            ``<``            |          `head < file.txt`          |
| Standard Output |   stdout    |  1  |       the outcome of command        | Terminal (screen) |  `>`<br>`>>` (append mode)  |        `echo 123 > file.txt`        |
| Standard Error  |   stderr    |  2  | the error message (if any produced) | Terminal (screen) | `2>`<br>`2>>` (append mode) | `cat not_exist > outfile 2> errors` |
Also `&>` operator exists that is basically a shorthand for `2>&1`
## Super User
The Super User, also known as "root user", represents a user account in Linux with all powers, privileges and capabilities. The super user can be accessed through the `sudo` (Super User DO) or `su` (Super User) commands.