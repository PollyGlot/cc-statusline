# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Claude Code statusline script for macOS. It's a bash script designed to be used with Claude Code's `statusline.command` setting to display a custom status bar.

## What it displays

- Repository name (from project directory)
- Model name (e.g., "Claude Opus 4.5")
- Context window usage as a progress bar with percentage
- Session token usage (formatted as K/M suffixes)
- 5-hour and 7-day API usage percentages (color-coded: green <60%, yellow 60-89%, red ≥90%)
- Current git branch

## Dependencies

- `jq` - for JSON parsing
- `curl` - for API requests
- macOS `security` command - for Keychain access

## Installation

Configure in Claude Code settings:
```json
{
  "statusline.command": "/path/to/statusline-command.sh"
}
```

## How it works

The script receives JSON on stdin from Claude Code containing workspace and context window info. It also fetches API usage from Anthropic's API endpoint with a 60-second cache at `/tmp/claude-usage-cache.json`.

### API Authentication

The script supports three methods to authenticate with Anthropic's API (in priority order):

1. **Environment Variable - `CLAUDE_CODE_OAUTH_TOKEN`** (recommended)
   ```bash
   export CLAUDE_CODE_OAUTH_TOKEN="your-oauth-token"
   ```

2. **Environment Variable - `ANTHROPIC_API_KEY`** (alternative)
   ```bash
   export ANTHROPIC_API_KEY="sk-ant-..."
   ```

3. **macOS Keychain** (fallback) - OAuth token stored by Claude Code in Keychain entry `Claude Code-credentials`

The script tries each method in order and uses the first one that's available.

## Testing with Environment Variable

To test the API integration with your OAuth token (already set in your `.zshrc`):

```bash
# Your token is already configured in zsh, so just run:
echo '{"workspace":{"project_dir":"/Users/pollyglot/dev/cc-statusline"},"model":{"display_name":"Claude Haiku 4.5"},"context_window":{"current_usage":{"input_tokens":100,"cache_creation_input_tokens":0,"cache_read_input_tokens":0},"context_window_size":200000,"total_input_tokens":1500,"total_output_tokens":500}}' | /Users/pollyglot/dev/cc-statusline/statusline-command.sh
```

Or test with a fresh shell environment:

```bash
export CLAUDE_CODE_OAUTH_TOKEN="your-token-here"
/Users/pollyglot/dev/cc-statusline/statusline-command.sh < input.json
```

The script will cache the API response in `/tmp/claude-usage-cache.json` for 60 seconds to avoid excessive API calls.
