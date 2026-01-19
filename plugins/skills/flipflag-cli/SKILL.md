---
name: flipflag-cli
description: FlipFlag CLI for task management, time tracking, and git branch creation. Use when starting or stopping work on tasks, tracking time, or managing feature/bugfix branches.
allowed-tools: Bash(flipflag:*), Bash(git branch:*), Bash(git rev-parse:*), Read
---

# FlipFlag CLI Skill

Manage development tasks with the FlipFlag CLI including time tracking and git branch management.

## Quick Reference

| Command | Description |
|---------|-------------|
| `flipflag start TASK-ID [options]` | Start work on a task |
| `flipflag stop TASK-ID` | Stop work on a task |
| `flipflag help` | Show available commands |

### Start Command Options

| Option | Default | Description |
|--------|---------|-------------|
| `--type=feature` | Yes | Task is a new feature |
| `--type=bugfix` | No | Task is a bug fix |
| `--branch` | Yes | Create/switch to git branch |
| `--no-branch` | No | Skip branch management |
| `--time` | Yes | Enable time tracking |
| `--no-time` | No | Disable time tracking |

## Usage Modes

### 1. With Explicit Arguments

When the user provides arguments, pass them directly without modification:

```bash
flipflag start EMD-1234 --type=bugfix --no-time
```

### 2. With Auto-Detection

When no task ID is provided, auto-detect from the current git branch:

```bash
# Get current branch
git rev-parse --abbrev-ref HEAD
```

Parse task ID from common branch patterns:
- `feature/EMD-1234` → `EMD-1234`
- `bugfix/TASK-567` → `TASK-567`
- `EMD-999-some-description` → `EMD-999`

### 3. Task Type Inference

Infer task type from the task ID when not explicitly specified:
- Contains "bug", "fix", or "hotfix" → `--type=bugfix`
- Otherwise → `--type=feature`

## Common Workflows

### Start a new feature with full tracking

```bash
flipflag start EMD-3200 --type=feature --branch --time
```

Creates:
- Task entry in `.flipflag.yml`
- Git branch `feature/EMD-3200`
- Time tracking interval

### Start a bugfix without time tracking

```bash
flipflag start EMD-3201 --type=bugfix --branch --no-time
```

### Stop current task

```bash
# Auto-detect task from branch
git rev-parse --abbrev-ref HEAD
# Then stop
flipflag stop EMD-3200
```

## Configuration File

FlipFlag stores task metadata in `.flipflag.yml`:

```yaml
EMD-3200:
  description: ""
  contributor: "dev@example.com"
  type: "feature"
  times:
    - started: "2025-01-15T10:00:00.000Z"
      finished: "2025-01-15T12:30:00.000Z"
    - started: "2025-01-16T09:00:00.000Z"
      finished: null
```

## Git Branch Naming

| Type | Branch Format |
|------|---------------|
| feature | `feature/TASK-ID` |
| bugfix | `bugfix/TASK-ID` |

Behavior:
- If branch exists → switches to it
- If branch does not exist → creates and switches to it
