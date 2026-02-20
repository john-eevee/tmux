# Tmux Configuration - Complementary to Wezterm & Neovim

A comprehensive tmux configuration optimized for use with Wezterm and Neovim, featuring the beautiful Catppuccin Mocha theme.

## Architecture: Three-Layer Hierarchy

Your setup creates an efficient three-tier organization:

```
WezTerm (Terminal Emulator)
  └─ Tabs (Project Containers)
      └─ Tmux Session (per-project workspace)
          ├─ Window 1: Editor (nvim)
          ├─ Window 2: Build/Tools  
          ├─ Window 3: Logs
          └─ Each window can have multiple panes
```

**Why?**
- WezTerm tabs isolate projects visually
- Tmux sessions persist when you disconnect
- Tmux windows separate concerns (edit, build, monitor)
- Tmux panes subdivide windows for parallel work

## Features

- **Neovim-compatible keybindings** - Vi-style navigation and copy mode
- **Project-aware workflow** - Dedicated window shortcuts for editor/build/logs
- **Intuitive splits** - Use `|` and `-` for horizontal and vertical splits
- **Mouse support** - Click to select panes and resize
- **True color support** - Beautiful colors in terminal
- **Catppuccin Mocha theme** - Soothing pastel theme
- **Smart defaults** - Windows/panes start at 1, escape time reduced
- **Well-documented** - All keybindings explained in config

## Installation

The configuration is already installed at `~/.config/tmux/tmux.conf` and the Catppuccin theme plugin is installed at `~/.config/tmux/plugins/catppuccin/tmux`.

To reload the configuration in a running tmux session:
```bash
tmux source ~/.config/tmux/tmux.conf
```

Or use the keybinding: `Prefix + r`

## Keybindings Quick Reference

### Prefix Key
- **Prefix**: `Ctrl + b` (the tmux default, compatible with WezTerm)

### Window Management (Separate concerns: editor, build, logs)
| Key | Action |
|-----|--------|
| `Prefix + c` | Create new window (prompts for name) |
| `Prefix + e` | New editor window (launches nvim) |
| `Prefix + T` | New terminal window (prompts for name) |
| `Prefix + b` | New build/tools window (prompts for name) |
| `Prefix + g` | New logs window (prompts for name) |
| `Prefix + a` | Add new window (flexible, prompts for name) |
| `Prefix + M-t` | Auto-numbered tool window (tool1, tool2...) |
| `Prefix + ,` | Rename current window |
| `Prefix + n` | Next window |
| `Prefix + p` | Previous window |
| `Prefix + 0-9` | Jump to window number |
| `Prefix + w` | Kill current window / List windows |
| `Prefix + f` | Find window by name |

### Pane Management (Subdivisions within windows)
| Key | Action |
|-----|--------|
| `Prefix + \|` | Split horizontally (left/right) |
| `Prefix + -` | Split vertically (top/bottom) |
| `Prefix + h` | Move to left pane |
| `Prefix + j` | Move to down pane |
| `Prefix + k` | Move to up pane |
| `Prefix + l` | Move to right pane |
| `Prefix + H` | Resize pane left |
| `Prefix + J` | Resize pane down |
| `Prefix + K` | Resize pane up |
| `Prefix + L` | Resize pane right |
| `Prefix + z` | Toggle pane zoom (maximize/restore) |
| `Prefix + x` | Kill current pane |
| `Prefix + q` | Show pane numbers |

### Session Management (Per-project containers)
| Key | Action |
|-----|--------|
| `Prefix + d` | Detach from session (keep running) |
| `Prefix + s` | List all sessions (for project switching) |
| `Prefix + $` | Rename current session |
| `Prefix + C` | Create new project session (interactive) |
| `Prefix + q` | Kill current session |
| `Prefix + (` | Previous session |
| `Prefix + )` | Next session |

### Layout Management (Predefined pane arrangements)
| Key | Action |
|-----|--------|
| `Prefix + L` | Show layout menu |
| `Prefix + Space` | Cycle to next layout |
| `Prefix + 1` | Main vertical (1 large left, stacked right) |
| `Prefix + 2` | Main horizontal (1 large top, stacked bottom) |
| `Prefix + 3` | Tiled (all panes equal) |
| `Prefix + 4` | Even vertical (equal columns) |
| `Prefix + 5` | Even horizontal (equal rows) |

### Copy Mode (Vi-style text selection)
| Key | Action |
|-----|--------|
| `Prefix + [` | Enter copy mode |
| `v` | Begin selection (in copy mode) |
| `y` | Copy selection (in copy mode) |
| `Ctrl + v` | Rectangle selection toggle (in copy mode) |
| `Prefix + ]` | Paste |
| `q` | Quit copy mode |

### Other
| Key | Action |
|-----|--------|
| `Prefix + r` | Reload configuration |
| `Prefix + ?` | List all keybindings |

## Neovim Integration

The configuration uses Vi-style keybindings that work seamlessly with Neovim:

- **Navigation**: `h`, `j`, `k`, `l` for pane movement (same as Neovim)
- **Copy mode**: Vi keybindings for text selection and copying
- **Visual mode**: `v` to start selection, `y` to yank (copy)
- **True color support**: Ensures Neovim colorschemes display correctly

For seamless navigation between tmux panes and Neovim splits, consider using the [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) plugin.

## Workflow Examples

### Example 1: New Project Session
```bash
# Create a new wezterm tab (Ctrl+a c in wezterm)

# Create tmux session
tmux new-session -s myproject

# Create specialized windows
Ctrl+a e  # Editor (nvim)
Ctrl+a T  # Terminal 1 (prompts for name)
Ctrl+a T  # Terminal 2 (prompts for name)
Ctrl+a b  # Build/tools
Ctrl+a g  # Logs monitor

# Or use auto-numbered approach
Ctrl+a M-t      # Creates tool1
Ctrl+a M-t      # Creates tool2

# Navigate between windows
Ctrl+a 1  # Go to editor
Ctrl+a 2  # Go to first custom window
Ctrl+a 3  # Go to next...
# Or: Ctrl+a w to list all windows by name
```

### Example 2: Editor with Split Terminal
```bash
# In your editor window
Ctrl+a |  # Split horizontal (left editor, right terminal)
Ctrl+a -  # Could further split into top/bottom

# Navigate panes
Ctrl+a h  # Focus left (editor)
Ctrl+a l  # Focus right (terminal)
```

### Example 3: Multi-Monitor Build System
```bash
# Create 4-pane layout
Ctrl+a -  # Split vertical (top/bottom)
Ctrl+a |  # Split each horizontal (now 4 panes)
Ctrl+a 3  # Apply tiled layout to arrange evenly

# Now run:
# - Left top: compiler
# - Right top: test runner
# - Left bottom: git watch
# - Right bottom: logs
```

### Example 4: Session Persistence
```bash
# Start work
tmux new-session -s work
Ctrl+a e  # Editor
# ... do work ...

# Need to switch projects?
Ctrl+a d  # Detach (session keeps running)

# Later, check what's running
tmux list-sessions

# Switch back
tmux attach-session -t work
# Everything is still there!
```

### Example 5: Quick Multi-Project Switching
```bash
# Project 1 (in wezterm tab 1)
tmux new-session -s frontend
Ctrl+a e
# work...

# Project 2 (in wezterm tab 2)
tmux new-session -s backend
Ctrl+a e
# work...

# Project 3 (in wezterm tab 3)
tmux new-session -s devops
Ctrl+a e
# work...

# Switch between projects via wezterm tabs or
Ctrl+a s  # List sessions and select
```

## Theme Customization

The Catppuccin Mocha theme is configured with:
- Rounded window status style
- Directory and session information in status bar
- Default background for transparency support

To customize the theme, edit the following variables in `tmux.conf`:
```tmux
set -g @catppuccin_flavor "mocha"  # Options: latte, frappe, macchiato, mocha
set -g @catppuccin_window_status_style "rounded"  # Options: basic, rounded, slanted, custom
set -g @catppuccin_status_modules_right "directory session"
```

## Tips & Best Practices

### Session Management
1. **One session per project** - Name sessions after projects: `tmux new -s myproject`
2. **Detach and reattach** - Use `Prefix + d` to detach. Reattach with `tmux attach -t myproject`
3. **Kill sessions cleanly** - Use `Prefix + q` to kill the current session when done
4. **List all sessions** - `Prefix + s` shows all running sessions across projects

### Window Organization
1. **Standardize layout** - Create editor/build/logs pattern for consistency
2. **Name windows** - Use `Prefix + ,` to rename windows descriptively
3. **Quick access** - Use number shortcuts (1-9) to jump between windows
4. **Focus mode** - Use `Prefix + z` to maximize a pane, press again to restore

### Pane Management
1. **Split strategically** - Use `|` for side-by-side (editor/terminal), `-` for stacked layouts
2. **Use layouts** - `Prefix + L` shows preset arrangements that might fit your needs
3. **Mouse resizing** - Drag pane borders with mouse to adjust sizes
4. **Maximize zoom** - `Prefix + z` is useful when you need full-screen focus on one pane

### Keyboard Efficiency
1. **Learn hjkl navigation** - Same keys work in tmux, vim, and nvim
2. **Use repeat** - Some commands like resize (`H/J/K/L`) repeat without re-pressing Prefix
3. **Copy mode** - `Prefix + [` for text selection, `y` to copy, `Prefix + ]` to paste
4. **System clipboard** - Selections automatically copy (may need xclip/wl-clipboard)

### Integration with Wezterm
- **Don't conflict** - Wezterm prefix (`Ctrl+a`) is same as tmux, but they don't interfere
- **Use tabs for projects** - Each wezterm tab = one project/tmux session
- **Detach workflow** - Detach from tmux (`Ctrl+a d`) but keep wezterm tab open
- **Clean exit** - Kill tmux session when done with project (`Ctrl+a q`)

### Integration with Neovim
- **Separate levels** - Tmux is outer (sessions/windows/panes), nvim is inner (buffers/splits)
- **No keybinding conflicts** - Tmux `Ctrl+a` vs nvim `Space` leader
- **Quick switches** - Split panes (tmux) for parallel editing and terminal work
- **Stay in context** - Use dedicated windows to avoid context switching

## Troubleshooting

### Colors not displaying correctly
Ensure your terminal emulator supports true color. Test with:
```bash
tmux info | grep Tc
```

### Theme not loading
Verify the plugin is installed:
```bash
ls ~/.config/tmux/plugins/catppuccin/tmux/
```

If missing, reinstall:
```bash
mkdir -p ~/.config/tmux/plugins/catppuccin
git clone -b v2.1.3 https://github.com/catppuccin/tmux.git ~/.config/tmux/plugins/catppuccin/tmux
```

### Keybindings not working
Reload the configuration:
```bash
tmux source ~/.config/tmux/tmux.conf
```

Or restart tmux completely:
```bash
tmux kill-server
tmux
```

## Resources

- [Tmux Cheat Sheet](https://tmuxcheatsheet.com/)
- [Catppuccin Tmux Theme](https://github.com/catppuccin/tmux)
- [Tmux Manual](https://man.openbsd.org/tmux)
