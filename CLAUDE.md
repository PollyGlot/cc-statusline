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

The script receives JSON on stdin from Claude Code containing workspace and context window info. It also fetches API usage from Anthropic's OAuth endpoint using credentials stored in macOS Keychain, with a 60-second cache at `/tmp/claude-usage-cache.json`.
