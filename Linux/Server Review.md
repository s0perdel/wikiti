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
==todo==