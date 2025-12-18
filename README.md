# claude-logs

A CLI tool to browse and view [Claude Code](https://docs.anthropic.com/en/docs/claude-code) session logs interactively.

## Why I Built This

Claude Code stores conversation logs as JSONL files in `~/.claude/projects/`, but browsing them manually is tedious. I wanted a quick way to:

- Find past sessions by project
- Preview conversations before opening
- Watch active sessions in real-time
- Check if Claude Code is currently running

## Features

- **Interactive Selection**: Use fuzzy finder (fzf) to browse projects and sessions
- **Session Preview**: See conversation snippets before opening
- **Real-time Monitoring**: Watch active sessions with `tail` command
- **Formatted Output**: Color-coded display of user/assistant messages and tool calls
- **Status Check**: See if Claude Code is running and view recent sessions

## Demo

![claude-logs demo](https://github.com/user-attachments/assets/676b1635-d00a-4396-a2f9-1478ae3d247b)

## Requirements

- macOS (uses `stat -f` for file timestamps)
- [fzf](https://github.com/junegunn/fzf) - Fuzzy finder
- [jq](https://jqlang.github.io/jq/) - JSON processor
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed

## Installation

### Using Homebrew (for dependencies)

```bash
brew install fzf jq
```

### Install the script

```bash
# Clone the repository
git clone https://github.com/wondercoms/claude-logs.git

# Make it executable
chmod +x claude-logs/claude-logs

# Add to your PATH (choose one)
# Option 1: Copy to local bin
cp claude-logs/claude-logs ~/.local/bin/

# Option 2: Add to PATH in your shell config
echo 'export PATH="$PATH:/path/to/claude-logs"' >> ~/.zshrc
```

## Usage

```bash
# Interactive mode - browse projects and sessions with fzf
claude-logs

# List all projects with session counts
claude-logs list

# View the latest session (select project first)
claude-logs latest

# Watch active session in real-time
claude-logs tail

# Check Claude Code status and recent sessions
claude-logs status

# Show help
claude-logs help
```

## Commands

| Command | Description |
|---------|-------------|
| (none) | Interactive mode - select project → session with fzf |
| `list` | Show all projects with session counts and last modified date |
| `latest` | Select a project, then view its most recent session |
| `tail` | Select a project, then watch its latest session in real-time |
| `status` | Check if Claude Code is running + show 3 most recent sessions |
| `help` | Show help message |

## Output Format

The tool formats JSONL logs into readable conversations:

```
━━━━━━━━━━ USER ━━━━━━━━━━
Your message here...

━━━━━━━━━━ CLAUDE ━━━━━━━━━━
Claude's response here...

🔧 [Tool: Read file]
```

## How It Works

Claude Code stores sessions in `~/.claude/projects/<project-hash>/` as JSONL files. This tool:

1. Scans the directory structure
2. Parses JSONL to extract user messages and assistant responses
3. Uses fzf for interactive selection with previews
4. Formats output with ANSI colors for readability

## License

MIT

## Contributing

Issues and PRs welcome!
