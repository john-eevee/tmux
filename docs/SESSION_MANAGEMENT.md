# Tmux Session Management

A native tmux session management system without plugins, providing powerful session creation, switching, and lifecycle management.

## Features

- **Interactive Session Switching** - Quick fuzzy search (with fzf) or native chooser
- **Session Templates** - Predefined layouts for dev, full-dev, and simple setups
- **Session Metadata** - Automatic tracking of session creation, directory, and state
- **Session Snapshots** - Save and restore session state for recovery
- **Batch Operations** - Kill multiple sessions, rename, search by pattern
- **JSON Export** - Integrate sessions with other tools and scripts
- **No Plugin Dependencies** - Pure bash + tmux, no external tmux plugins required

## Quick Start

### Create a Session

From tmux (prefix = `Ctrl+a`):
```
Ctrl+a C
# Enter session name
```

From shell:
```bash
~/.config/tmux/bin/session-manager.sh create myproject ~/projects/myproject
```

### Create Session with Layout

From tmux:
```
Ctrl+a M-c   # Creates session with dev layout (editor, term, logs)
Ctrl+a M-f   # Creates session with full-dev layout (editor, build, term, logs)
```

From shell:
```bash
~/.config/tmux/bin/session-manager.sh create-from-template work dev ~/projects/work
```

### Switch Between Sessions

From tmux:
```
Ctrl+a S     # Interactive switcher (fzf if available, else tmux menu)
Ctrl+a s     # Native tmux session chooser
```

From shell:
```bash
~/.config/tmux/bin/session-manager.sh select
~/.config/tmux/bin/session-manager.sh attach work
```

### List Sessions

From tmux:
```
Ctrl+a M-s   # Show detailed session list
```

From shell:
```bash
~/.config/tmux/bin/session-manager.sh list
```

## Key Bindings

| Binding | Action |
|---------|--------|
| `Ctrl+a S` | Interactive session switcher (fzf) |
| `Ctrl+a C` | Create new session (prompt for name) |
| `Ctrl+a M-c` | Create session with dev layout |
| `Ctrl+a M-f` | Create session with full-dev layout |
| `Ctrl+a q` | Kill current session (with confirmation) |
| `Ctrl+a M-q` | Kill all other sessions |
| `Ctrl+a M-s` | List sessions with detailed information |
| `Ctrl+a M-l` | Snapshot current session state |
| `Ctrl+a s` | Native tmux session chooser |
| `Ctrl+a $` | Rename current session |
| `Ctrl+a d` | Detach from session |
| `Ctrl+a (` / `)` | Previous/next session |

## Session Manager Commands

The session manager script at `~/.config/tmux/bin/session-manager.sh` supports:

### Basic Commands

```bash
# Create a session
session-manager.sh create <name> [directory]
session-manager.sh create myproject ~/projects/myproject

# Create session with predefined layout
session-manager.sh create-from-template <name> <layout> [directory]
session-manager.sh create-from-template work dev ~/projects/work

# Interactive selector
session-manager.sh select

# List all sessions
session-manager.sh list

# Attach to a session
session-manager.sh attach <name>
```

### Session Control

```bash
# Kill a specific session
session-manager.sh kill <name>

# Kill all sessions except current
session-manager.sh kill-others

# Rename a session
session-manager.sh rename <old-name> <new-name>

# Search sessions by name (regex pattern)
session-manager.sh search <pattern>
```

### Advanced Features

```bash
# Create a snapshot of current session
session-manager.sh snapshot [name]

# Restore a session from snapshot
session-manager.sh restore <name>

# Apply layout to existing session
session-manager.sh apply-layout <session> <layout>

# Export sessions as JSON
session-manager.sh export-json
```

## Session Layouts

Three predefined layouts are available:

### `simple`
Basic session with default shell window. Good for quick tasks.

### `dev`
Development layout with three windows:
- `edit` - Neovim editor window
- `term` - Terminal for commands
- `logs` - Window for log monitoring

```
┌─────────────────────────────────┐
│ edit            │ term          │
│                 ├───────────────┤
│                 │ logs          │
└─────────────────────────────────┘
```

### `full-dev`
Extended development layout with four windows:
- `edit` - Neovim editor
- `build` - Build/compilation window
- `term` - General terminal
- `logs` - Log monitoring

```
┌─────────────────────────────────┐
│ edit    │ build   │ term        │
│         ├─────────┤             │
│         │ logs    │             │
└─────────────────────────────────┘
```

## Session Metadata & State

### Automatic Metadata

When a session is created, the manager automatically saves:
- Session name
- Starting directory
- Creation timestamp

Metadata files are stored in `~/.config/tmux/.sessions/`

Example metadata file: `~/.config/tmux/.sessions/myproject.meta`
```
SESSION_NAME=myproject
START_DIR=/home/user/projects/myproject
CREATED=2024-02-19T10:30:45Z
```

### Session Snapshots

Save the current state of a session for recovery:

```bash
# From tmux
Ctrl+a M-l

# From shell
session-manager.sh snapshot myproject

# Restore later
session-manager.sh restore myproject
```

Snapshots are stored in `~/.config/tmux/.sessions/`

## Integration Examples

### Shell Alias
Add to your `.bashrc` or `.zshrc`:

```bash
alias tm='~/.config/tmux/bin/session-manager.sh'
alias tms='~/.config/tmux/bin/session-manager.sh select'
alias tml='~/.config/tmux/bin/session-manager.sh list'
```

### Create Session from Project Directory

```bash
# Create a session for current directory
tm create "$(basename $PWD)" "$PWD"

# Or add to directory-specific profile
cd ~/projects/myproject
tm create myproject .
```

### Batch Operations

```bash
# Kill sessions matching pattern
tm search "old-" | xargs -I {} tm kill {}

# List all sessions as JSON
tm export-json | jq '.[] | .name'
```

## Directory Structure

```
~/.config/tmux/
├── bin/
│   ├── session-manager.sh      # Main session management script
│   └── session-quick.sh        # Quick session switcher
├── .sessions/
│   ├── myproject.meta          # Session metadata
│   └── myproject.snapshot      # Session state snapshot
├── .session-layouts/           # Custom layout definitions (future)
└── tmux.conf                   # Main configuration
```

## Tips & Tricks

### Auto-create Session on Shell Entry

Add to your shell profile:

```bash
# Auto-create or attach dev session on new shell
if [ -z "$TMUX" ]; then
    if ! tmux has-session -t main 2>/dev/null; then
        ~/.config/tmux/bin/session-manager.sh create-from-template main dev
    fi
    tmux attach-session -t main
fi
```

### Session-based Project Switching

```bash
# Create function in shell profile
project() {
    if [ $# -eq 0 ]; then
        ~/.config/tmux/bin/session-manager.sh select
    else
        local project_dir="$HOME/projects/$1"
        if [ -d "$project_dir" ]; then
            ~/.config/tmux/bin/session-manager.sh create-from-template "$1" dev "$project_dir"
        else
            echo "Project directory not found: $project_dir"
        fi
    fi
}
```

Then use:
```bash
project myapp    # Create or switch to myapp session
project          # Interactive selector
```

### Monitor Sessions with Watch

```bash
# Keep running in a window to monitor all sessions
watch -n 2 '~/.config/tmux/bin/session-manager.sh list'
```

## Performance Notes

- Session manager operations are lightweight (pure bash + tmux)
- Metadata and snapshots are minimal (text files)
- No background services or daemons required
- FZF fuzzy matching (optional) is faster than native tmux chooser

## Troubleshooting

### Session not found error

Check if session exists:
```bash
tmux list-sessions
```

### Cannot create session with same name

Sessions must have unique names. Either:
- Choose a different name: `tm create newname`
- Kill the existing session: `tm kill oldname`
- Rename the existing session: `tm rename oldname newname`

### Metadata files not found

Metadata files are automatically created in `~/.config/tmux/.sessions/`

If directory doesn't exist, create it:
```bash
mkdir -p ~/.config/tmux/.sessions
```

### FZF not available

The quick switcher (`Ctrl+a S`) will automatically fall back to native tmux chooser if fzf is not installed. To use fzf:

```bash
# Install fzf
brew install fzf      # macOS
apt install fzf       # Debian/Ubuntu
```

## Future Enhancements

- [ ] Custom layout templates via configuration files
- [ ] Session presets for specific projects
- [ ] Automatic session recovery on tmux crash
- [ ] Session bookmarks and favorites
- [ ] Session synchronization across panes
- [ ] Integration with direnv for auto-session switching
