---
tags:
---
Process is an instance of a running program.
Commands are also processes.

## Background / Foreground Processes

To send command to background we need to add an ampersand (&) at the end of the command.
`command &`
When the command finish the work, you will receive a message in the terminal like this:
`[1]+ Done                 command`

If we need to send a running command to background, we need to suspend the running process using  Ctrl+Z and then use a command `bg`

To show all background processes we use command `jobs`
The output is like:
```BASH
$ jobs
[1]   Running                 sleep 500 &
[2]-  Running                 sleep 800 &
[3]+  Running                 sleep 700 &
```

1, 2, 3 are IDs of the background processes.
You can see also the statuses and the command.
`+` after ID means that is the last process you have run or foregrounded.
`-` after ID means that is the second last process you have run or foregrounded.

To bring a process to foreground we use command `fg`
Without an argument it will bring the process with `+` next to ID.
Or you can return certain process (e.g. `fg %2`)

If you close the terminal (terminal logout) the *jobs* will stop.
To deal with that we have 2 commands:

`nohup` (NO Hang UP)
It is used before the command if you know in advance that you will close the terminal.
```BASH
nohup python3 long_script.py &
# You can now close the terminal. Output goes to nohup.out 
```

`disown`
It is used when you already have a background process that you want to save.
Important notice: this command removes the process from `jobs`, after you can track it only with `ps`.
```BASH
python3 long_script.py &
disown # or disown %ID (like command fg)
# The output will still go to the terminal
```

## Listing and Finding Processes

`/proc` directory contains a directory for each process, there you can find a lot information, for example to view specific process details you can use `cat /proc/{PID}/status`

Types of processes:
- *Child Process*: created by another process known as the *Parent Process*.
- *Zombie Process*: has finished its execution but still has an entry in the process table.
- *Orphan Process*: continues to run even after the *Parent Process* has been completed or terminated.
- *Daemon Process*: is a system-related background process, do not require a controlling terminal to operate.

`ps` (Process Snapshot)
It shows the processes that you require.
For example the most common call is `ps aux` (see everything)
- a = show process of all users
- u = user-friendly format (shows user, CPU, memory)
- x = include processes without a terminal (daemons, background)
`ps -ef` show PPID (Parent PID) instead of %CPU/%MEM.
`ps -u username` filter by user.
`ps -p PID` check specific process
`ps aux --forest` show process tree.
`ps -eo` custom output, for example:
```BASH
ps -eo pid,ppid,user,%cpu,%mem,cmd --sort=-%cpu | head -10
```
- `-e` = all processes
- `-o` = custom format (choose your columns)
- `--sort=-%cpu` = sort by CPU% descending
Also instead of `ps aux | grep something` we have built-in `pgrep`.
`pstree` to see visual process hierarchy.

`top` (The Dashboard)
It is a real-time monitoring, like `ps aux` but live.
Also there you can manage processes.
Exist more user-friendly and advanced utility `htop`, but installation is required.

## Process Signals & Killing Processes

Process Signals allow user and system to communicate with running processes.
The most common signals:

| Name    | Value | Description                                                                                |
| ------- | ----- | ------------------------------------------------------------------------------------------ |
| SIGHUP  | 1     | hangup signal, used to restart or reload processes                                         |
| SIGINT  | 2     | interrupt signal, sent when you press Ctrl+C                                               |
| SIGQUIT | 3     | quit signal, used to create a core dump of a process                                       |
| SIGILL  | 4     | an illegal instruction signal, sent when a process tries to execute an illegal instruction |
| SIGTERM | 15    | termination signal, telling to a process that he need to finish execution                  |
| SIGKILL | 9     | kill signal, a forceful signal that immediately terminates the process                     |
| SIGSTOP | 19    | stop signal, used to pause or suspend a process                                            |
| SIGTSTP | 20    | terminal stop signal, sent when you press Ctrl+Z                                           |
| SIGCONT | 18    | continue signal, used to resume a suspended process                                        |
| SIGCHLD | 17    | child signal, send to a parent process when a child process terminates or stops            |

To send a signal to a process we use a command `kill`, by default it sends `SIGTERM` to the process.
Overview:
```BASH
kill -L # listing available process signals
kill 1121 # send SIGTERM to a process with PID 1121
kill 100 200 300 # send SIGTERM to a few processes
kill -s SIGKILL 333 # send specific signal to a process
kill -s KILL 333 # same as above
kill -s 9 333 # same as above
kill -9 333 # same as above (-SIGKILL, -KILL also possible)
```

If you cannot find the PID of a program, you can use `pidof program`.
Also you can check the process existence with `kill -0`, it doesn't send a signal but instead checks if a process exists and if you have permission to signal it.

## Process Priorities

Linux uses a "niceness" scale from -20 to +19:
- -20: highest priority (least "nice" - greedy)
- 0: default priority
- +19: lowest priority (very "nice" - cooperative)
*Important: Only root can set negative nice values.*

The real process priority is based on Kernel Priority (kernel's internal value, 0-139).
```text
PRI (kernel priority) = 80 + NI (nice value)
```

So normal processes can only have kernel priority from 60 to 99.

We can check process priorities using `ps` (e.g.  `ps -eo pid,pri,ni,user.comm`)
Or by using `top`/`htop`.

Setting priorities:
```BASH
# start a process with custom priority
nice -n 15 ./long_task.sh # NI=15,PRI=95, low priority (high nice value)
sudo nice -n -10 ./critical_task.sh # NI=-10,PRI=80, high priority (requires root)

# change priority of running process
renice 10 1234 # make process 1234 lower priority (more nice)
sudo renice -12 555 # make process 555 higher priority (requires root)
sudo renice 5 -u username # change priority for all processes of a user
renice 10 -p 1 2 3 # change prirority for all processes in a process group
```

Also you can change them interactively via `top`/`htop`.

## Process Forking

Forking is the fundamental mechanism Linux uses to create new processes.
When process calls `fork()`, it clones itself into two identical processes:
1. Parent Process (original)
2. Child Process (the copy)

`fork()` returns:
- 0 to the child process:
- \>0 (child's PID) to the parent process
- -1 if fork failed
```text
BEFORE fork():          AFTER fork():

[Parent Process]        [Parent Process]
PID: 1000               PID: 1000
                        fork() returns 1001
                        
                        [Child Process]
                        PID: 1001
                        fork() returns 0
```
Modern Linux uses Copy-On-Write (COW) optimisation:
- PID (new, unique)
- PPID (set to parent's PID)
- File descriptors (opened files remain open in child)
- Memory space (initially identical)
- CPU registers
- Program counter (continues from after `fork()`)

Example of Basic Fork in Python:
```Python
import os

print(f'before fork - PID: {os.getpid()}')
pid = os.fork() # fork creates child process

if pid == 0:
	# child process
	print(f'child - PID: {os.getpid()}, PPID: {os.getppid()}')
	print(f'child - fork() returned: {pid}')
elif pid > 0:
	# parent process
	print(f'parent - PID: {os.getpid()}')
	print(f'parent - fork() returned: {pid}')
else: # pid = -1
	print('fork failed')
```
Output:
```BASH
before fork - PID: 1234
parent - PID: 1234
parent - fork() returned: 1235
child - PID: 1235, PPID: 1234
child - fork() returned: 0
```