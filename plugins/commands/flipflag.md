---
name: flipflag
description: Start or stop work on a task with FlipFlag CLI. Manages time tracking, git branches, and task metadata.
argument-hint: "<start|stop> [TASK-ID] [--type=feature|bugfix] [--no-time] [--no-branch]"
allowed-tools: Bash(flipflag:*), Bash(git rev-parse:*), Read
---

# FlipFlag Command

Execute FlipFlag CLI commands for task management.

## Usage Examples

```
/flipflag start EMD-1234
/flipflag start EMD-1234 --type=bugfix --no-time
/flipflag stop EMD-1234
/flipflag stop
```

## Execution Instructions

### Parse the arguments

1. First argument is the action (`start` or `stop`)
2. Second argument (if present and not starting with `--`) is the task ID
3. Remaining arguments are passed as CLI options

### For `start` action

If a task ID is provided:
```bash
flipflag start <task_id> [options]
```

If no task ID is provided, auto-detect from current branch:
```bash
git rev-parse --abbrev-ref HEAD
```

Then extract task ID from branch pattern:
- `feature/EMD-1234` → `EMD-1234`
- `bugfix/TASK-567` → `TASK-567`
- `EMD-999-some-description` → `EMD-999`

Apply task type inference if `--type` not specified:
- Task ID contains "bug", "fix", or "hotfix" → `--type=bugfix`
- Otherwise → `--type=feature`

### For `stop` action

If a task ID is provided:
```bash
flipflag stop <task_id>
```

If no task ID is provided, auto-detect from branch first.

## After Execution

Report the result to the user:
- Task started/stopped successfully
- Git branch created/switched (if applicable)
- Time tracking status
- Any errors encountered
