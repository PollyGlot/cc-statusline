# Claude Code Statusline

A custom statusline script for Claude Code on macOS that displays useful session and API usage information.

## Screenshot

![Example statusline](example.png)

## What It Displays

- **Repository name** - Current project directory name with git branch: `project(branch)`
- **Model** - Active Claude model (e.g., "Opus")
- **Context window** - Visual progress bar with percentage
- **Session tokens** - Total tokens used this session (formatted as K/M)
- **API usage** - 5-hour and 7-day utilization (color-coded: green <60%, yellow 60-89%, red ≥90%)

## Requirements

- macOS (uses Keychain for OAuth token)
- `jq` - Install with `brew install jq`
- Claude Code with OAuth authentication

## Installation

### With AI Agent

Copy and paste this prompt into Claude Code:

```
Use the statusline-setup agent to download and install this script:
https://raw.githubusercontent.com/PollyGlot/cc-statusline/main/statusline-command.sh
```

### Manual Install

1. Copy the script to your Claude directory:

```bash
mkdir -p ~/.claude
curl -o ~/.claude/statusline-command.sh https://raw.githubusercontent.com/PollyGlot/cc-statusline/main/statusline-command.sh
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

## How It Works

The script receives JSON on stdin from Claude Code containing:
- Workspace info (project directory)
- Model info
- Context window usage and size
- Session token counts

It also fetches API usage from Anthropic's OAuth endpoint using credentials stored in macOS Keychain, with a 60-second cache to avoid excessive API calls.
