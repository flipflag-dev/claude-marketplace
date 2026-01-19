# FlipFlag CLI Plugin for Claude Code

Task management, time tracking, and feature flag code review for Claude Code.

## Features

- **`/flipflag` command** - Start/stop work on tasks with automatic git branch creation and time tracking
- **`/ff-status` command** - Check current task status, time sessions, and contributor info
- **Feature Flag Reviewer agent** - Reviews code for proper FlipFlag SDK usage (React hooks & Node.js)

## Prerequisites

- FlipFlag CLI installed globally (`npm install -g @flipflag/cli`)

## Installation

### Option 1: Install from GitHub

```bash
claude /plugin marketplace add flipflag-dev/claude-marketplace
```

### Option 2: Clone and load locally

```bash
git clone https://github.com/flipflag-dev/claude-marketplace.git
claude --plugin-dir /path/to/claude-marketplace
```

### Option 3: Copy to project

Copy the plugin contents to your project's `.claude/` directory.

## Usage

### Starting a Task

```bash
# Start with explicit task ID
/flipflag start EMD-1234

# Start with task type
/flipflag start EMD-1234 --type=bugfix

# Start without time tracking
/flipflag start EMD-1234 --no-time

# Start without creating a git branch
/flipflag start EMD-1234 --no-branch
```

### Stopping a Task

```bash
# Stop specific task
/flipflag stop EMD-1234

# Stop current task (auto-detect from branch)
/flipflag stop
```

### Checking Status

```bash
# Show all tasks
/ff-status

# Show specific task details
/ff-status EMD-1234
```

### Feature Flag Code Review

The feature flag reviewer agent is triggered during code reviews or when examining feature flag implementations. It checks:

- React SDK: `useFlag()`, `useFlags()`, `useFlipFlagReady()` hooks
- Node SDK: `FlipFlagCore`, `isEnabled()`, `init()`, `destroy()`
- Naming conventions and consistency
- Both enabled/disabled path coverage
- Dead code from removed flags

## Components

| Component | Type | Description |
|-----------|------|-------------|
| `/flipflag` | Command | Start/stop tasks with time tracking and git branch management |
| `/ff-status` | Command | Display task status from `.flipflag.yml` |
| `feature-flag-reviewer` | Agent | Reviews code for proper FlipFlag SDK usage |
| `flipflag-cli` | Skill | CLI task management skill with usage reference |

## Configuration

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

| Task Type | Branch Format |
|-----------|---------------|
| feature | `feature/TASK-ID` |
| bugfix | `bugfix/TASK-ID` |

## Auto-Detection

The plugin can auto-detect task IDs from branch names:

- `feature/EMD-1234` → `EMD-1234`
- `bugfix/TASK-567` → `TASK-567`
- `EMD-999-some-description` → `EMD-999`

Task type is inferred from the task ID:
- Contains "bug", "fix", or "hotfix" → `bugfix`
- Otherwise → `feature`

## License

MIT
