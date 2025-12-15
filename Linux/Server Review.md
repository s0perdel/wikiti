---
tags:
  - todo
---
Here is the [[Maintenance Checklist]] for Linux servers.

## Uptime and Load

Checking how long the system has been running and the CPU load is very important.
We have some tools for that:

| Tool        | Installation | Description                                                                                           |
| ----------- | ------------ | ----------------------------------------------------------------------------------------------------- |
| `uptime`    | by default   | show the uptime and the load averages for the past 1, 5 and 15 minutes; 1.0 means 100% for 1 core     |
| `uprecords` | require      | provides the record of your uptimes; it's a daemon that tracks uptimes and records them to a database |
| `top`       | by default   | gives an in-depth view of the resource usage at the current moment                                    |
| `glances`   | require      | more deep `top`, show available cores, network properties, disks etc.                                 |

## Authentication Logs

To monitor the system authentication log file we need to access:
- Ubuntu/Debian: `/var/log/auth.log`
- Fedora/Red Hat/CentOS: `/var/log/secure`
Or there are stored login information in binary databases:
- Logs of last login sessions: `/var/log/wtmp`
- Logs of the current login sessions: `/var/run/utmp`
- Logs of the bad login attempts: `/var/log/btmp`
There are some useful tools to access those files without decoding:
### `/var/log/wtmp` (Login History Database)

It records successful logins/logouts.
`last` - command for checking.

| Command         | Description                            |
| --------------- | -------------------------------------- |
| `last`          | show a listing of last logged in users |
| `last -10`      | show last 10 logins/logouts            |
| `last username` | show only username's logins/logouts    |

### `/var/run/utmp` (Current Logins Database)

It tracks currently logged-in users.
We have the following tools for monitoring:

| Command                 | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `users`                 | show only the usernames of logged-in users         |
| `who`                   | show online users and their additional information |
| `w`                     | `who` + shows their CPU, connection type, etc      |
| `last -f /var/run/utmp` | decode the main file, works like `who`             |

### `/var/log/btmp` (Failed Login Database)

It records failed login attempts.
We use the command `lastb` (works same as `last` but for `/var/log/btmp`).

## Available Memory and Disk

Memory management is always very important.
There are tools for that:

| Command             | Description                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `free -h`           | the flag is for human-readable format, command shows total, used, free, shared, cache and available memory (swap as well) |
| `top`               | provides an overview of the running processes and their resource usage, including memory usage, available memory etc.     |
| `vmstat`            | provides states about system memory usage                                                                                 |
| `vmstat -s`         | `vmstat` but detailed                                                                                                     |
| `cat /proc/meminfo` | `/proc/meminfo` contains detailed information about system memory usage                                                   |

## Running Services

Linux server run various services including web, database, DNS, mail servers, etc. 
Deep diving into services will be in a separate topic, so here will be described only few tools to monitor running services.

| Command                | Purpose              | Example                                    |
| ---------------------- | -------------------- | ------------------------------------------ |
| `systemctl list-units` | list all services    | `systemctl --type=service --state=running` |
| `ss -tulpn`            | show listening ports | `ss -tulpn \| grep :22`                    |
| `lsof -i`              | find service by port | `lsof -i :80`                              |
| `journalctl -u`        | service logs         | `journalctl -u docker -f`                  |
| `ps aux --sort`        | resource usage       | `ps aux --sort=%mem`                       |

