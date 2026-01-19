---
name: ff-status
description: Check current task status from .flipflag.yml. Shows active tasks, time tracking, and contributor info.
argument-hint: "[TASK-ID]"
allowed-tools: Bash(git rev-parse:*), Read
---

# FlipFlag Status Command

Display current task status from the `.flipflag.yml` configuration file.

## Usage Examples

```
/ff-status
/ff-status EMD-1234
```

## Execution Instructions

### 1. Check for configuration file

First verify `.flipflag.yml` exists in the current directory.

If the file does not exist, respond:
```
No FlipFlag configuration found.
Run `/flipflag start TASK-ID` to create your first task.
```

### 2. Read the configuration file

Read `.flipflag.yml` and parse the YAML content.

### 3. Get current git branch context

```bash
git rev-parse --abbrev-ref HEAD 2>/dev/null
```

### 4. Display status

**If no task ID argument provided** - show all tasks:

```
FlipFlag Status
===============

Current Branch: feature/EMD-3200

Active Tasks:
  EMD-3200 (feature) - ACTIVE
    Contributor: dev@example.com
    Started: 2025-01-15 10:00 (2h 30m elapsed)
    Total tracked: 4h 15m

  EMD-3150 (bugfix)
    Contributor: dev@example.com
    Last worked: 2025-01-14
    Total tracked: 1h 45m
```

**If task ID argument provided** - show specific task:

```
Task: EMD-3200
Type: feature
Contributor: dev@example.com
Status: ACTIVE (time tracking running)

Time Sessions:
  1. 2025-01-15 10:00 - 12:30 (2h 30m)
  2. 2025-01-16 09:00 - ongoing

Total Tracked: 2h 30m + ongoing
```

### Calculating time

For each task, calculate:
- Total tracked time (sum of all finished sessions)
- Current session elapsed time (if `finished: null`)
- Mark as "ACTIVE" if current session is ongoing
- Highlight if the task matches the current git branch
