# Claude Code Statusline

A custom statusline script for Claude Code on macOS that displays useful session and API usage information.

## Screenshot

```
my-project | Opus | ████░░░░░░ 42% | 156.3K | 5h:23% 7d:45% | (main)
```

## What it displays

- **Repository name** - Current project directory name
- **Model** - Active Claude model (e.g., "Opus")
- **Context window** - Visual progress bar with percentage
- **Session tokens** - Total tokens used this session (formatted as K/M)
- **API usage** - 5-hour and 7-day utilization (color-coded: green <60%, yellow 60-89%, red ≥90%)
- **Git branch** - Current branch name

## Requirements

- macOS (uses Keychain for OAuth token)
- `jq` - Install with `brew install jq`
- Claude Code with OAuth authentication

## Installation

1. Copy the script to your Claude directory:

```bash
mkdir -p ~/.claude
cp statusline-command.sh ~/.claude/
chmod +x ~/.claude/statusline-command.sh
```

2. Add to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline-command.sh"
  }
}
```

3. Restart Claude Code to see the statusline.

## How it works

The script receives JSON on stdin from Claude Code containing:
- Workspace info (project directory)
- Model info
- Context window usage and size
- Session token counts

It also fetches API usage from Anthropic's OAuth endpoint using credentials stored in macOS Keychain, with a 60-second cache to avoid excessive API calls.
