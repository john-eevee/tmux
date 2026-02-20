# Tmux Configuration Documentation

Complete reference for a professional-grade tmux setup with session management, keybindings, and workflow optimization.

## Quick Navigation

**Getting started?** → Start with [QUICKSTART.md](QUICKSTART.md)

**Looking for a key?** → Check [KEYBINDINGS.md](KEYBINDINGS.md)

**Want to learn workflows?** → Read [WORKFLOWS.md](WORKFLOWS.md)

**Understanding the design?** → See [ARCHITECTURE.md](ARCHITECTURE.md)

**Managing sessions?** → Review [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

## Documentation Structure

```
docs/
├── README.md                    ← You are here
├── QUICKSTART.md               → First 5 minutes
├── KEYBINDINGS.md              → All keyboard shortcuts (comprehensive)
├── WORKFLOWS.md                → Practical usage patterns
├── ARCHITECTURE.md             → Design decisions & concepts
└── SESSION_MANAGEMENT.md       → Advanced session control
```

## What This Configuration Provides

### Core Features

- **Native Session Management** - Create, switch, and manage tmux sessions without plugins
- **Vi-style Navigation** - Familiar hjkl movement throughout
- **Which-Key Menu** - Press `Ctrl+a /` to see all available commands
- **Project-Focused Shortcuts** - Quick window creation for development tasks
- **Session Templates** - Predefined layouts (dev, full-dev, simple)
- **Catppuccin Theme** - Beautiful Mocha color scheme

### Architecture

```
Terminal (wezterm/kitty/etc)
  └─ Tmux Session
      ├─ Window 1 (editor)
      │   ├─ Pane A
      │   └─ Pane B
      ├─ Window 2 (build)
      │   └─ Pane A
      └─ Window 3 (logs)
          └─ Pane A
```

Three levels of organization:
1. **Windows** - Separate concerns (editing, building, monitoring)
2. **Panes** - Split screen within a window
3. **Sessions** - Independent projects, can detach and reattach

## Key Bindings at a Glance

Prefix: `Ctrl+a`

| Category | Common Keys |
|----------|-----------|
| **Sessions** | `S` (switch), `C` (create), `q` (kill) |
| **Windows** | `c` (create), `n/p` (next/prev), `1-9` (jump) |
| **Panes** | `h/j/k/l` (navigate), `\|/-` (split), `H/J/K/L` (resize) |
| **Layouts** | `L` (menu), `Space` (cycle), `1-5` (presets) |
| **Copy Mode** | `[` (enter), `v` (select), `y` (copy), `]` (paste) |

See [KEYBINDINGS.md](KEYBINDINGS.md) for the complete reference.

## Files in This Configuration

### Configuration
- `~/.config/tmux/tmux.conf` - Main tmux configuration
- `~/.config/tmux/bin/session-manager.sh` - Session management script
- `~/.config/tmux/bin/session-quick.sh` - Quick session switcher

### State & Metadata
- `~/.config/tmux/.sessions/` - Session metadata and snapshots
- `~/.config/tmux/.session-layouts/` - Custom layout templates (future)

### Documentation
- `docs/README.md` - This file
- `docs/QUICKSTART.md` - Getting started guide
- `docs/KEYBINDINGS.md` - Complete keybinding reference
- `docs/WORKFLOWS.md` - Practical workflow examples
- `docs/ARCHITECTURE.md` - Design decisions
- `docs/SESSION_MANAGEMENT.md` - Advanced session control

## Installation

The configuration is ready to use. Verify it loads:

```bash
# Test configuration
tmux -f ~/.config/tmux/tmux.conf list-keys | head

# Or reload if tmux is running
Ctrl+a r
```

## Common Tasks

### Create a Session

```bash
# Interactive (in tmux)
Ctrl+a C
# Enter session name

# From shell
~/.config/tmux/bin/session-manager.sh create myproject ~/projects/myproject
```

### Switch Between Sessions

```bash
# Fuzzy finder (with fzf)
Ctrl+a S

# Native tmux chooser
Ctrl+a s
```

### Create Windows

```bash
# Generic window
Ctrl+a c

# For specific purposes, use interactive prompts
Ctrl+a T   # Terminal window
Ctrl+a b   # Build window
Ctrl+a g   # Logs window
Ctrl+a a   # Add any window
```

### Split a Window

```bash
Ctrl+a |   # Split horizontally (side-by-side)
Ctrl+a -   # Split vertically (top-bottom)
```

### Navigate Panes

```bash
Ctrl+a h   # Move left
Ctrl+a j   # Move down
Ctrl+a k   # Move up
Ctrl+a l   # Move right
```

### Manage Sessions

```bash
# List sessions with details
Ctrl+a M-s

# Detach (keep running)
Ctrl+a d

# Kill current session
Ctrl+a q

# Kill all other sessions
Ctrl+a M-q
```

## Tips & Tricks

### Use Layouts for Quick Pane Arrangement

```bash
Ctrl+a L       # Show layout menu
Ctrl+a 1       # Main vertical (1 large left, stacked right)
Ctrl+a 2       # Main horizontal (1 large top, stacked bottom)
Ctrl+a Space   # Cycle to next layout
```

### Maximize a Pane

```bash
Ctrl+a z       # Toggle zoom (temporary maximize)
```

### Session Snapshots

```bash
# Save session state
Ctrl+a M-l

# Restore later from shell
~/.config/tmux/bin/session-manager.sh restore myproject
```

### Show All Keybindings

```bash
# Native tmux list
Ctrl+a ?

# Interactive which-key menu
Ctrl+a /
```

## Troubleshooting

### Configuration won't load

```bash
# Check for syntax errors
tmux -f ~/.config/tmux/tmux.conf list-keys

# Reload in running tmux
Ctrl+a r

# Or from shell
tmux source-file ~/.config/tmux/tmux.conf
```

### Session manager scripts not found

```bash
# Verify scripts exist
ls -la ~/.config/tmux/bin/

# Make executable
chmod +x ~/.config/tmux/bin/*.sh

# Test directly
~/.config/tmux/bin/session-manager.sh list
```

### Colors look wrong

Check that your terminal supports 256 colors:

```bash
echo $TERM
# Should output: xterm-256color or similar
```

### Can't attach to session

```bash
# List all sessions
tmux list-sessions

# Reset tmux server if needed
tmux kill-server
tmux new-session -s test
```

## Customization

### Change Prefix Key

Edit `tmux.conf` (around line 47):

```bash
# Current:
set -g prefix C-a
bind C-a send-prefix

# To use Ctrl+b (tmux default):
set -g prefix C-b
bind C-b send-prefix
```

### Add Custom Keybindings

Add to `tmux.conf`:

```bash
# Example: Quick project session
bind M-p command-prompt -p "Project name:" { 
  run-shell "~/.config/tmux/bin/session-manager.sh create-from-template '%% 'dev'" 
}
```

### Create Custom Layouts

Define layouts in `tmux.conf`:

```bash
bind -n M-1 select-layout main-vertical
bind -n M-2 select-layout main-horizontal
```

## Performance & Requirements

- **Dependencies**: bash, tmux (3.0+), fzf (optional)
- **Memory**: Minimal (scripts are lightweight)
- **Startup**: Instant (no external plugins)

## Getting Help

1. **See a command in the menu?** Use the which-key menu (`Ctrl+a /`)
2. **Need a full list?** Use `Ctrl+a ?` for native tmux bindings
3. **Want examples?** Check [WORKFLOWS.md](WORKFLOWS.md)
4. **Confused about architecture?** Read [ARCHITECTURE.md](ARCHITECTURE.md)

## What's Next?

After reviewing this documentation, explore these areas:

1. **First time?** Follow [QUICKSTART.md](QUICKSTART.md)
2. **Learn patterns** in [WORKFLOWS.md](WORKFLOWS.md)
3. **Understand design** in [ARCHITECTURE.md](ARCHITECTURE.md)
4. **Advanced sessions** in [SESSION_MANAGEMENT.md](SESSION_MANAGEMENT.md)

## Related Tools

This configuration works best with:
- **Terminal Emulator**: wezterm, kitty, iterm2, gnome-terminal
- **Editor**: Neovim (especially with smart tmux integration)
- **Shell**: bash, zsh, fish
- **Finder**: fzf (for fuzzy session selection)

## Version & Updates

- **Current Version**: 2.0
- **Last Updated**: 2026-02-20
- **tmux Minimum**: 3.0+

---

**Happy multiplexing!** 🚀
