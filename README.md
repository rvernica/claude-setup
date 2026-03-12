# Claude Code Setup

A shareable collection of Claude Code configurations: custom status line, motivational spinner messages, [peon-ping](https://github.com/PeonPing/peon-ping) voice notifications, and a [roborev](https://github.com/rares-fodor/roborev) wrapper with its own sound pack.

## What's Included

- **`settings.json`** — Claude Code settings with hooks, permissions, status line, and custom spinner verbs (motivational messages that replace the default "Thinking..." spinner)
- **`statusline.sh`** — Custom status bar showing model name, context window usage (color-coded), current directory, and git branch/status
- **`peon-ping/config.json`** — Default peon-ping config (peasant sound pack, 50% volume)
- **`roborev/claude-roborev`** — Wrapper script that launches Claude with a separate peon-ping config directory for roborev code reviews
- **`roborev/peon-ping-config.json`** — Roborev-specific peon-ping config (sc_battlecruiser sound pack)
- **`roborev/config.toml`** — Roborev daemon config pointing `claude_code_cmd` at the wrapper

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- [peon-ping](https://github.com/PeonPing/peon-ping) installed (provides the voice notification sounds)
- [jq](https://jqlang.github.io/jq/) (used by the status line script)
- (Optional) [roborev](https://github.com/rares-fodor/roborev) for automated code reviews

## Installation

### 1. Clone this repo

```bash
git clone https://github.com/YOUR_USERNAME/claude-setup.git
cd claude-setup
```

### 2. Install peon-ping

Follow the instructions at [https://github.com/PeonPing/peon-ping](https://github.com/PeonPing/peon-ping).

### 3. Copy settings

```bash
cp settings.json ~/.claude/settings.json
```

> **Note:** The hook paths in `settings.json` use `$HOME/.claude/hooks/peon-ping/peon.sh`. This relies on peon-ping being installed to that location. If your peon-ping is elsewhere, update the paths accordingly.

### 4. Set up the status line

```bash
cp statusline.sh ~/.claude/statusline.sh
chmod +x ~/.claude/statusline.sh
```

### 5. Set up peon-ping config

```bash
mkdir -p ~/.claude/hooks/peon-ping
cp peon-ping/config.json ~/.claude/hooks/peon-ping/config.json
```

### 6. (Optional) RoboRev setup

If you use roborev for automated code reviews with a different sound pack:

```bash
# Copy the roborev peon-ping config
mkdir -p ~/.claude/hooks/peon-ping-roborev
cp roborev/peon-ping-config.json ~/.claude/hooks/peon-ping-roborev/config.json

# Install the wrapper script
cp roborev/claude-roborev ~/.local/bin/claude-roborev
chmod +x ~/.local/bin/claude-roborev

# Copy the roborev daemon config
mkdir -p ~/.roborev
cp roborev/config.toml ~/.roborev/config.toml
```

The `config.toml` sets `claude_code_cmd` to the wrapper, so roborev invokes `claude-roborev` which sets `CLAUDE_PEON_DIR` to use the sc_battlecruiser sound pack. This way you can tell roborev sessions apart by sound.

## Customization

### Spinner Messages

Edit the `spinnerVerbs.verbs` array in `settings.json`. Set `mode` to `"append"` instead of `"replace"` to keep the defaults alongside your custom messages. The verb list is based on [this gist by topherhunt](https://gist.github.com/topherhunt/b7fa7b915d6ee3a7998363d12dc8c842).

### Sound Packs & Volume

Edit `peon-ping/config.json` (or `roborev/peon-ping-config.json`):
- `active_pack` — change the sound pack (e.g., `"peasant"`, `"sc_battlecruiser"`)
- `volume` — `0.0` to `1.0`
- `categories` — toggle which events trigger sounds
