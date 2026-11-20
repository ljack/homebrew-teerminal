# Teerminal Homebrew Tap

Public Homebrew tap distributing Developer ID signed and Apple notarized macOS builds of Teerminal.

## Editions

- **`teerminal` (Full)**: Full build with all supported AI coding harnesses and Skippy / Master AI orchestration.
- **`teerminal-company` (Company)**: Restricted build for corporate laptops — GitHub Copilot and local models (plus plain shells) only, with no Skippy / Master AI or external consumer agent harnesses.

## Requirements

- macOS 26 (Tahoe) or later
- [Homebrew](https://brew.sh)

## Install

No GitHub login or personal access token is required.

```bash
# Full edition
brew install --cask ljack/teerminal/teerminal

# Company edition (GitHub Copilot + local models only)
brew install --cask ljack/teerminal/teerminal-company
```

If `/Applications` is not writable without administrator privileges (common on managed corporate laptops), install into `~/Applications`:

```bash
brew install --cask --appdir=~/Applications ljack/teerminal/teerminal-company
```

This installs `Teerminal.app`, Python (`python@3.14`), `tmux`, and the `teerminalctl` and `tt` CLI tools.

Launch Teerminal from Launchpad, Spotlight, or `open -a Teerminal`.

## Upgrade

```bash
brew upgrade --cask teerminal
# or for the Company edition:
brew upgrade --cask teerminal-company
```

## Switching Editions

The two casks conflict because they share the `Teerminal.app` bundle name and CLI binaries, while both preserve your workspaces, presets, and session state in `~/Library/Application Support/Teerminal`:

```bash
brew uninstall --cask teerminal
brew install --cask ljack/teerminal/teerminal-company
```

## Optional: Harness Integration

To let coding agents launched inside Teerminal coordinate or spawn subagent tabs via MCP and skills, register the bundled CLI once (adjust the app path to `~/Applications/Teerminal.app` if installed per-user):

```bash
CLI=/Applications/Teerminal.app/Contents/Resources/cli

# Claude Code
claude mcp add --scope user teerminal -- /usr/bin/python3 "$CLI/tools/teerminal_mcp.py"
mkdir -p ~/.claude/skills && ln -sfn "$CLI/Resources/skills/teerminal-subagents" ~/.claude/skills/

# Codex
codex mcp add teerminal -- /usr/bin/python3 "$CLI/tools/teerminal_mcp.py"

# Grok
grok mcp add --scope user teerminal /usr/bin/python3 -- "$CLI/tools/teerminal_mcp.py"

# Antigravity (agy)
agy mcp add teerminal /usr/bin/python3 "$CLI/tools/teerminal_mcp.py"
```
